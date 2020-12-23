## Use Lambda Functions within a Stateless Architecture on AWS


### Create a Lambda Function with a Role

Your company is using Amazon S3 for file storage (using an S3 bucket) where all users can put and delete their files. When a user uploads or deletes a file, your company requires an entry to be immediately created in the database table. This entry contains the file metadata, like the file’s name, creation time, size of the file, Bucket, Event, and so on.


![Product](https://github.com/IamVigneshC/AmazonWebServices/blob/main/AWS%20Lambda/Resources/Lambda.JPG)


1. Open the Services menu in the top navigation bar, and from the Compute section, select Lambda. 

1. In the navigation pane on the left, click on Functions. (This is probably chosen by default.)

1. Click Create function.

1. Select Author from scratch (should be selected by default), and enter the following values:

   - Function name: my-function
   - Runtime: Python 3.8

1. Under Permissions, expand Choose or create an execution role, and select Create a new role with basic Lambda permissions. (This should be chosen by default.)

1. Click Create function. An AWS Lambda function named my-function will be created with a basic execution role.

1. Open the Services menu in the top navigation bar, and from the Security, Identity, & Compliance section, select IAM.

1. In the navigation pane on the left, click on Roles.

1. Click Create role.

1. Choose
Lambda
Allow Lambda functions to call AWS Services on your behalf.
, and click on Next: Permissions.

1. Under Attach permissions policies, check the following two policies:

   - AmazonDynamoDBFullAccess
   - AmazonS3ReadOnlyAccess

1. Click Next: Tags.

1. Click Next: Review.

1. For the Role name, enter lambda-s3-dynamodb-role. 

1. Click Create role.

1. Open the Services menu in the top navigation bar, and from the Compute section, select Lambda. Click my-function, then click on the Permissions tab.

1. Beside Execution role, click Edit, and enter the following:

   - Description: lambda-s3-dynamodb-role

   - Existing role: choose lambda-s3-dynamodb-role from the drop-down

1. Click Save. (If you see Unsaved changes, refresh the page.)

To view the resources and actions that your function has permission to access, click on the Lambda Function my-function, and then click the Permissions tab, and under this click on AWS Key Management Service. 


### Add an S3 Trigger to the Lambda Function

![Product](https://github.com/IamVigneshC/AmazonWebServices/blob/main/AWS%20Lambda/Resources/S3.JPG)


1. (This is likely not needed.) Open the Services menu in the top navigation bar, and from the Compute section, select Lambda. Click my-function.

1. Click Add trigger.

1. From the drop-down under Trigger configuration, scroll down and select S3. Then fill in the following values, and click Add:

   - Bucket: choose lambda-s3-plural-<random_name> from the drop down.

   - Event type: All object create events (default value).
     If added properly, an S3 trigger will appear below:

1. Click Add trigger, choose S3 and the same Bucket as before, but this time select an Event type of Permanently deleted. Then click Add.


### Add Code to the Lambda Function

1. Click on my-function, and scroll down to the Function code section.

1. Copy and paste the code below into the online editor, overwriting any code that already exists and Save:

         import boto3 
         from uuid import uuid4 
         def lambda_handler(event, context): 
         s3 = boto3.client("s3") 
         dynamodb = boto3.resource('dynamodb') 
         for record in event['Records']:     
                 bucket_name = record['s3']['bucket']['name'] 
                 object_key = record['s3']['object']['key']  
                 size = record['s3']['object'].get('size', -1)   
                 event_name = record ['eventName']  
                 event_time = record['eventTime']         
                 dynamoTable = dynamodb.Table('lambda-s3-table')  
                 dynamoTable.put_item(  
                          `Item={'RequestId': str(uuid4()), 'Bucket': bucket_name, 'Object': object_key,'Size': size, 'Event': event_name, 'EventTime':  
                          `event_time})
                          


### Trigger Lambda Function Code

![Product](https://github.com/IamVigneshC/AmazonWebServices/blob/main/AWS%20Lambda/Resources/Trigger.JPG)

1. Open the Services menu in the top navigation bar, and from the Storage section, select S3.

1. Click on preconfigured bucket lambda-s3-plural-<random_name>.

1. Click Upload, and add any small file from your local system. Sample_Cloud_Lab.docx is used in the examples here. Then click Upload.

1. Open the Services menu in the top navigation bar, and from the Database section, select DynamoDB.

1. In the navigation pane on the left, choose Tables, and select lambda-s3-table.

1. Click on the Items tab, and click on the new entry that is there to view its details

1. Head back to the lambda-s3-plural-<random_name> S3 bucket, and upload a new file. links.txt will be used as an example here. Then, delete the existing one. For example, do the following:

   - Click on lambda-s3-plural-<random_name> bucket, and upload a new file links.txt. (Choose a small file from your own computer.)

   - Delete the existing file Sample_Cloud_Lab.docx (the one you uploaded before).

1. Head back to the Items tab of the lambda-s3-table, and click the refresh button. You will see two new entries 


### Use AWS Lambda to Trigger Email Notification on S3 Events

1. Open the Services menu in the top navigation bar, and from the Security, Identity, & Compliance section, select IAM.

1. Click on Roles, then click Create role.

1. Choose 
Lambda
Allow Lambda functions to call AWS Services on your behalf
, and click on Next: Permissions.

1. Under Attach permissions policies, choose the below the policy:

   - AmazonSESFullAccess

   - AWSLambdaExecute

1. Click Next: Tags, and then Next: Review, and enter a Role name of lambda-ses-role.

1. Click Create role.

1. Go back to the Lambda page like in previous Challenges, click my-function, and then click on the Permissions tab.

1. Beside Execution role, click Edit, and enter the following:

   - Description: lambda-s3-ses-role

   - Existing role: Choose lambda-ses-role from the drop down

1. Click Save. (If you see Unsaved changes, refresh your browser.)

1. Copy and paste the code below into the online editor for my-function, overwriting any code that already exists. Replace both hard-coded email addresses with your own email address.

        import json 
        import boto3 
        def lambda_handler(event, context): 
        for i in event["Records"]: 
              action = i["eventName"] 
              ip = i["requestParameters"]["sourceIPAddress"] 
              bucket_name = i["s3"]["bucket"]["name"] 
              object =i["s3"]["object"]["key"]  
        client = boto3.client("ses") 
        subject = str(action) + 'Event from ' + bucket_name 
        body = """  
                  <br>    
                  This is a notification mail to inform you regarding {} event.     
                  The object {} is deleted.    
                  Source IP: {}    
         """.format(action, object, ip) 
         message = {"Subject": {"Data": subject}, "Body": {"Html": {"Data": body}}}  
         response = client.send_email(Source = "abc@example.com", Destination = {"ToAddresses": ["abc@@example.com"]}, Message = message) 
       
1. Open the Services menu in the top navigation bar, and from the Customer Engagement section, select Simple Email Service.                 

1. In the navigation pane on the left, click Email Addresses.

1. Click Verify a New Email Address. Enter your email address. Check your email, and follow the instructions in the Verification email you will receive.

1. Go to the S3 page and click on the preconfigured bucket lambda-s3-plural-<random_name>.

1. Upload a new file pics.jpg, and delete the existing one links.txt. (Just examples, use files from your own system, links.txt is the file you uploaded previously.)

1. Check your mail

1. Open the Services menu in the top navigation bar, and from the Management & Governance section, select CloudWatch.

1. In the navigation pane on the left, click Log groups, click /aws/lambda/my-function, click on the most recent entry under Log stream




