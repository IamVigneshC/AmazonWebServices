## Administer and Use the API Gateway on AWS

### Import an Existing API into Amazon API Gateway

Your company, Perfect Pets, runs its customer-facing Node.js Express line-of-business (LOB) web application from a local data center. The Perfect Pets development team wants to migrate the application entirely to AWS using API Gateway, AWS Lambda, and AWS DynamoDB. You’ve been asked to deploy API Gateway and import the LOB application’s REST API.


1. Go to the service API Gateway.

1. On the API Gateway page, in the REST API tile, click the Import button.
Note: Don't choose REST API Private by mistake.

1. Click OK to dismiss the popup, then on the Create new API page, make the sure the following settings are correct, and then click Import to complete the process.

Create new API: Example API

Endpoint type: Regional

Warnings: Fail on warnings

You should now see a list of resources predefined in the PetStore API. At the left-hand side of the page from the navigation pane, click APIs to see a list of your APIs. Note that you can define and maintain multiple APIs within your Amazon API Gateway instance.


### Test the API in the API Gateway Console


Now that you’ve imported the API, you should test it to ensure its HTTP integrations with the on-premises back-end remain functional.

1. From the API Gateway dashboard's APIs list, click the PetStore link.
   Note: If you aren't already at the API Gateway dashboard, you can get there via Services > API Gateway.

1. In the Resources list, click the /pets resource's GET method.

![API] (https://github.com/IamVigneshC/AmazonWebServices/blob/main/AWS%20API%20Gateway/API.png)

1. In the /pets - GET - Method Execution pane, click TEST. Next, click the Test button at the bottom of the page to invoke the method.
   Note: Scroll down the page and observe the following results:

   - There is an HTTP Status code 200 (which means the request succeeded).

   - The JavaScript Object Notation (JSON) Response Body lists three pet definitions.

   - The full HTTP Endpoint request URI was http://petstore.execute-api.us-east-1.amazonaws.com/petstore/pets.

   - This URI (a URL in this case too) takes the form http(s)://<api-name>.execute-api<aws-region>/<api-name>/<resource-name>

1. Now run a second test, this time invoking the GET method of the /{petId} resource. For path, specify a {petId} value of 2, then click Test.
   Scroll down the page and note the results:

   - The JSON Response Body returns only the cat definition, which has the ID of 2.

   - The full HTTP Endpoint request URI was http://petstore.execute-api.us-east-1.amazonaws.com/petstore/pets/2
   
   
   ### Deploy the API to a Stage
   
   Although you can test the API within the AWS API Gateway console, you must publish (deploy) the API to make it accessible. Deployment stages enable you to deploy your API incrementally, and they support version rollback if necessary.

1. Click Actions > Deploy API.

1. In the Deploy API window, provide the following values and then click Deploy:

Deployment stage: [New Stage]

Stage name: dev1

Stage description: Development stage 1

Deployment description: Initial API deployment

Note: If you see it, you may safely ignore the warning which states User does not have ListWebACLs....

Now you'll make a change to your API, publish the change, and then compare results from invoking methods from each stage.

1. In the left-hand navigation pane, click Resources.

1. In the Resources tree, click /pets's GET .

1. In the /pets - GET - Method Execution pane, click Method Request.

1. Under Settings, open the API Key Required property drop-down list box (click the little grey pencil icon), and choose true. Make sure to click the little grey checkbox icon to apply this change!

1. Click Actions > Deploy API. Specify the following values, and then click Deploy:

Deployment stage: [New Stage]

Stage name: dev2

Stage description: Development stage 2

Deployment description: Second API deployment

Note: If you see it, you may safely ignore the warning which states User does not have ListWebACLs....

1. In the Stages tree, expand dev2, select /pet's GET method, and open the Invoke URL in a new tab or window.

Note: the URL includes dev2, the stage name. You should receive a Forbidden error because you enabled API key authorization for this stage.

1. In the Stages tree, expand dev1, select /pet's GET method, and open the Invoke URL in a new tab or window.

This time you should receive a proper JSON response because the dev1 stage has no authorization enabled.


    [

      {

        "id": 1,

        "type": "dog",

        "price": 249.99

      },

      {

        "id": 2,

        "type": "cat",

        "price": 124.99

      },

      {

        "id": 3,

        "type": "fish",

        "price": 0.99

      }

    ]
    
### Optimize the API to Improve its Responsiveness
    
AWS API Gateway includes a number of product features that improve the responsiveness, stability, and security of your hosted APIs. You want to enable two of these features now.

1. In the Stages tree, click dev1, and ensure you’re in the Settings tab.

1. Under Cache Settings, click the check box to select Enable API cache.

1. Under Enable throttling, set the Rate to 5000 requests per second, and set Burst to 1000 requests.

1. Click Save Changes to complete the configuration.
