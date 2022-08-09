+++
title = "Deploy ImageManager Lambda Function"
date = 2020
weight = 1
chapter = false
pre = "<b>4.1. </b>"
+++
#### Deploy ImageManager Lambda Function

You will edit the code in the **handleRequest** override (in **LambdaFunctionHandler.java**) so that if the **contentType** is not **image/jpeg** the file will be deleted by the Lambda function. This simulates a scenario where, if a file with the wrong content type is uploaded, the handler will discard the file, and not process it. Further, if the file is **image/jpeg** the Lambda function should resize the image and move it to a target bucket so your application can use it.

1. Open the **LambdaFunctionHandler.java** file whose path is **src/main/java/idevelop.lambda.s3handler/LambdaFunctionHandler.java**
* Find the following line
```
context.getLogger().log("CONTENT TYPE: " + contentType);
```
* After that line, add switch logic to call two different handlers - one for an image type and another for a non-image type
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
2. Add the following content at the end of the **LambdaFunctionHandler.java** file, just before the closing brace.
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
3. Add the following content at the head of the **LambdaFunctionHandler.java** file and save
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

4. If you are confused, you can see the above **LambdaFunctionHandler.java** file.
5. In the **Command Prompt**, navigate to the directory of the **TestLambda** project
* Execute the following command to build
```
mvn package
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-004.png?featherlight=false&width=60pc)
* When the target JAR is built, which is in the **target** folder of the **TestLambda** project and whose name is **s3handler-1.0.0.jar**
6. Go to [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Type ```TestLambda``` to the search bar and press **Enter**.
* Click **TestLambda**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-005.png?featherlight=false&width=90pc)
7. To provide the function package, in the **Code soure** section
* Click **Upload from**
* Click **.zip or .jar file**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-006.png?featherlight=false&width=90pc)
8. Click **Upload**, Select file **s3handler-1.0.0.jar** in the **target** folder of the **TestLambda** project
* Click **Save**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-007.png?featherlight=false&width=90pc)
9. It will take a few moments to upload
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-008.png?featherlight=false&width=90pc)
10. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Type ```idevelop-imagemanager``` to the search bar
* Click the S3 bucket whose name start with **idevelop-imagemanager**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-009.png?featherlight=false&width=90pc)
11. Click **uploads/**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-010.png?featherlight=false&width=90pc)
12. Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-011.png?featherlight=false&width=90pc)
13. Click **Add files**
* Select file **Puppy.jpg** we downloaded previously
* Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-012.png?featherlight=false&width=90pc)
14. Click **Close**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-013.png?featherlight=false&width=90pc)
15. Click **idevelop-imagemanager-**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-014.png?featherlight=false&width=90pc)
16. Click the **processed** folder
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-015.png?featherlight=false&width=90pc)
17. We uploaded the **Puppy.jpg** image to the **uploads** folder. We will see a thumbnail of the image is placed in the **processed/** folder in the root of the same bucket. The **processed/** folder will be created automatically by the Lambda function when it creates the output thumbnail file.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-016.png?featherlight=false&width=90pc)
18. Go to [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Type ```TestLambda``` to the search bar and press **Enter**.
* Click **TestLambda**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-017.png?featherlight=false&width=90pc)
19. Click **Monitor**
* Click **View logs in CloudWatch**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-018.png?featherlight=false&width=90pc)
20. Click the first **Log stream** in the **Log streams** table
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-019.png?featherlight=false&width=90pc)
21. We will see the process in the **CloudWatch logs**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-020.png?featherlight=false&width=90pc)
22. Upload a non-image file to the **uploads/** folder (Do the same step 10, 11, 12 and 13)
* Click **Close**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-021.png?featherlight=false&width=90pc)
23. We will see the **Module1.template.yaml** file is automatically deleted
{{% notice note %}} 
The **Module3.template.yaml** file we had uploaded before we update the **Lambda** function. Therefore, it is not automatically deleted
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-022.png?featherlight=false&width=90pc)
24. Do the same the step 18, 19, 20 and 21 to see the process in the **CloudWatch logs**.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-023.png?featherlight=false&width=90pc)
25. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Type ```idevelop-imagemanager``` to the search bar
* Click the S3 bucket whose name starts with **idevelop-imagemanager**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-024.png?featherlight=false&width=90pc)
26. Click the **processed/** folder
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-025.png?featherlight=false&width=90pc)
27. Click the **Puppy.thumb.jpg** file
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-026.png?featherlight=false&width=90pc)
28. Click the link in the **Object URL** section
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-027.png?featherlight=false&width=90pc)
29. We will receive an Access Denied message. This is because the S3 bucket policy is not set to allow anonymous read access to the bucket.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-028.png?featherlight=false&width=90pc)
30. Click **idevelop-imagemanager**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-029.png?featherlight=false&width=90pc)
31. Select the **processed/** folder
* Click **Actions**
* Click **Make public using ACL**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-030.png?featherlight=false&width=90pc)
32. Click **Make public**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-031.png?featherlight=false&width=90pc)
33. Access to this file again. You will then be able to view the processed files in the web browser.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.1-deploy-imagemanager-lambda/deploy-imagemanager-lambda-032.png?featherlight=false&width=90pc)