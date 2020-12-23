## Use Lambda Functions within a Stateless Architecture on AWS

Your company is using Amazon S3 for file storage (using an S3 bucket) where all users can put and delete their files. When a user uploads or deletes a file, your company requires an entry to be immediately created in the database table. This entry contains the file metadata, like the file’s name, creation time, size of the file, Bucket, Event, and so on.



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
