+++
title = "Triển khai hàm Lambda ImageManager "
date = 2020
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++
#### Triển khai hàm Lambda ImageManager 

Bạn sẽ chỉnh sửa đoạn code trong **handleRequest** trong tập tin **LambdaFunctionHandler.java** để những tập tin có contentType không phải là **image/jpeg** sẽ bị xóa bởi hàm Lambda. Điều này mô phỏng một tình huống, trong đó nếu có một tập tin tải lên sai content type, tập tin này sẽ bị loại bỏ và không xử lý. Nếu tập tin đó là **image/jpeg**, hàm Lambda sẽ thu nhỏ hình ảnh và chuyển hình thu nhỏ tới một target bucket để ứng dụng có thể sử dụng chúng.

1. Mở file **LambdaFunctionHandler.java** có đường dẫn **src/main/java/idevelop.lambda.s3handler/LambdaFunctionHandler.java**
* Tìm dòng code dưới đây
```
context.getLogger().log("CONTENT TYPE: " + contentType);
```
* Sau dòng code trên, thêm đoạn code sau để gọi 2 xử lý - một cho tập tin hình ảnh và một cho các tập tin không phải hình ảnh
```
switch ( contentType )
{
	case "application/x-directory":
		System.out.println("application/x-directory detected - ignoring");
		break;

	case "image/jpeg":

		System.out.println("image/jpeg detected");
		InputStream objectData = null;
		objectData = response.getObjectContent();
		handleJPEG(bucket, key, objectData);
		break;

	default:
		handleAllOtherContentTypes(bucket, key);
		break;
}
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-001.png?featherlight=false&width=90pc)
2. Thêm đoạn code dưới đây vào cuối của class **LambdaFunctionHandler**
```
private void handleJPEG(String bucketName, String key, InputStream imageStream) {

	final int THUMBNAIL_WIDTH = 100;
	final int THUMBNAIL_HEIGHT= 100;

	try
	{
		System.out.println("Starting resize process...");

		System.out.println(String.format(
				"Starting resize process for %s/%s of type image/jpg", bucketName, key));

		// Resize the image
		System.out.println("     Reading image stream from S3");
		BufferedImage image = ImageIO.read(imageStream);
		System.out.println("          done");

		final BufferedImage thumbnailImage = new BufferedImage(THUMBNAIL_WIDTH, THUMBNAIL_HEIGHT, BufferedImage.TYPE_INT_RGB);
		final Graphics2D graphics2D = thumbnailImage.createGraphics();
		graphics2D.setComposite(AlphaComposite.Src);

		graphics2D.setRenderingHint(RenderingHints.KEY_INTERPOLATION,RenderingHints.VALUE_INTERPOLATION_BILINEAR);
		graphics2D.setRenderingHint(RenderingHints.KEY_RENDERING,RenderingHints.VALUE_RENDER_QUALITY);
		graphics2D.setRenderingHint(RenderingHints.KEY_ANTIALIASING,RenderingHints.VALUE_ANTIALIAS_ON);

		System.out.println("     Drawing image...");
		graphics2D.drawImage(image, 0, 0, THUMBNAIL_WIDTH, THUMBNAIL_HEIGHT, null);
		System.out.println("          done");
		graphics2D.dispose();

		System.out.println("     Opening output file...");
		File fileThumbnail = new File("/tmp/thumbnail.jpg");
		System.out.println("          done");
		System.out.println("     Writing output file...");
		ImageIO.write(thumbnailImage, "jpg", fileThumbnail);
		System.out.println("          done");

		//
		// Now put the finished object into the /processed subfolder
		// Note that the filename manipulation here is not meant to be
		// production-ready and robust! It will break if files without extensions
		// are uploaded!
		//
		String fileName = key.substring( key.lastIndexOf('/') + 1, key.length() );
		String fileNameWithoutExtn = fileName.substring(0, fileName.lastIndexOf('.'));
		System.out.println("     Pushing output file to processed folder...");
		s3.putObject(bucketName, "processed/" + fileNameWithoutExtn + ".thumb.jpg", fileThumbnail);
		System.out.println("          done");
	}
	catch(Exception e)
	{
		System.out.println(String.format(
				"Error processing JPEG image from stream for %s/%s", bucketName, key));
		System.out.println(e.getMessage());
	}
	finally
	{
		System.out.println("Ended resize");
	}
}

private void handleAllOtherContentTypes(String bucketName, String key) {

	System.out.println(String.format(
			"%s/%s is an unsupported file type. It will be deleted.", bucketName, key));

	s3.deleteObject(bucketName, key);
	System.out.println("     Done!");
}
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-002.png?featherlight=false&width=90pc)
3. Thêm đoạn code sau vào phần import ở đầu file **LambdaFunctionHandler.java** và lưu lại
```
import java.awt.AlphaComposite;
import java.awt.Graphics2D;
import java.awt.RenderingHints;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.InputStream;
import javax.imageio.ImageIO;
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-003.png?featherlight=false&width=90pc)

{{%attachments title="LambdaFunctionHandler file" style="orange" pattern="LambdaFunctionHandler.java"/%}}

4. Nếu gặp khó khăn, bạn có thể tham khảo file **LambdaFunctionHandler.java** mẫu ở trên.
5. Trong **Command Prompt**, chuyển đường dẫn đến project **TestLambda**
* Chạy lệnh dưới đây để build project
```
mvn package
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-004.png?featherlight=false&width=60pc)
* Kết quả trả về một tập tin có tên **s3handler-1.0.0.jar** trong thư mục **target** của project **TestLambda**
6. Truy cập [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Nhập ```TestLambda``` vào ô tìm kiếm và nhấn **Enter**.
* Click **TestLambda**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-005.png?featherlight=false&width=90pc)
7. Để cung cấp function package, trong phần **Code soure**
* Click **Upload from**
* Click **.zip or .jar file**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-006.png?featherlight=false&width=90pc)
8. Click **Upload**, Chọn file **s3handler-1.0.0.jar** trong thư mục **target** của project **TestLambda**
* Click **Save**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-007.png?featherlight=false&width=90pc)
9. Sẽ mất một chút thời gian để tải lên
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-008.png?featherlight=false&width=90pc)
10. Truy cập [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Nhập ```idevelop-imagemanager``` vào ô tìm kiếm
* Click S3 bucket có tên bắt đầu bằng **idevelop-imagemanager**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-009.png?featherlight=false&width=90pc)
11. Click thư mục **uploads/**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-010.png?featherlight=false&width=90pc)
12. Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-011.png?featherlight=false&width=90pc)
13. Click **Add files**
* Chọn file **Puppy.jpg** chúng ta đã tải về
* Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-012.png?featherlight=false&width=90pc)
14. Click **Close**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-013.png?featherlight=false&width=90pc)
15. Click **idevelop-imagemanager-**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-014.png?featherlight=false&width=90pc)
16. Click vào thư mục **processed**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-015.png?featherlight=false&width=90pc)
17. Chúng ta đã tải ảnh **Puppy.jpg** lên thư mục **uploads**. Chúng ta sẽ thấy có một hình ảnh thu nhỏ được tạo và lưu trữ tại thư mục **processed/**. Thư mục **processed/** sẽ được tạo tự động bởi hàm Lambda khi nó tạo các hình ảnh thu nhỏ.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-016.png?featherlight=false&width=90pc)
18. Truy cập [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Nhập ```TestLambda``` vào ô tìm kiếm và nhấn **Enter**.
* Click **TestLambda**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-017.png?featherlight=false&width=90pc)
19. Click **Monitor**
* Click **View logs in CloudWatch**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-018.png?featherlight=false&width=90pc)
20. Click **Log stream** đầu tiên trong bảng **Log streams**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-019.png?featherlight=false&width=90pc)
21. Chúng ta hãy xem quá trình diễn ra như thế nào trong **CloudWatch logs**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-020.png?featherlight=false&width=90pc)
22. Tải lên một tập tin bất kỳ không phải hình ảnh vào thư mục **uploads/** (làm tương tự bước 10, 11, 12 và 13)
* Click **Close**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-021.png?featherlight=false&width=90pc)
23. Ta sẽ thấy rằng tệp tin **Module1.template.yaml** vừa tải lên đã bị xóa
{{% notice note %}} 
Tệp tin **Module3.template.yaml** chúng ta đã tải lên trước khi cập nhật lại hàm **Lambda** nên nó không bị xóa
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-022.png?featherlight=false&width=90pc)
24. Làm tương tự bước 18, 19, 20 và 21 để xem quá trình thực hiện như thế nào trong **CloudWatch log**.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-023.png?featherlight=false&width=90pc)
25. Truy cập [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Nhập ```idevelop-imagemanager``` vào ô tìm kiếm
* Click S3 bucket có tên bắt đầu bằng **idevelop-imagemanager**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-024.png?featherlight=false&width=90pc)
26. Click thư mục **processed/**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-025.png?featherlight=false&width=90pc)
27. Click tệp tin **Puppy.thumb.jpg**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-026.png?featherlight=false&width=90pc)
28. Click link trong mục **Object URL**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-027.png?featherlight=false&width=90pc)
29. Chúng ta sẽ nhận được thông báo **Access Denied** vì S3 bucket policy chưa cho phép người dùng ẩn danh có thể có truyền đọc các tập tin trong bucket.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-028.png?featherlight=false&width=90pc)
30. Click **idevelop-imagemanager**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-029.png?featherlight=false&width=90pc)
31. Chọn thư mục **processed/**
* Click **Actions**
* Click **Make public using ACL**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-030.png?featherlight=false&width=90pc)
32. Click **Make public**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-031.png?featherlight=false&width=90pc)
33. Truy cập lại vào tệp tin. Bây giờ ta đã có thể xem được nội dung của tập tin.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-032.png?featherlight=false&width=90pc)