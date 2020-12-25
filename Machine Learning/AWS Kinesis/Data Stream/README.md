Refer my blog: https://iamvigneshc-mydigitalworld.blogspot.com/2020/12/machine-learning-on-aws_24.html

## Implement a Data Ingestion Solution Using Amazon Kinesis Data Streams

### Create a Kinesis Data Stream

You are a data science consultant for a company called Globomantics, analyzing live temperature feeds. Your primary responsibility is to gather real time data from temperature sensors, and ingest this into a Kinesis Data Stream so that logs can be further analyzed. You will be configuring the Kinesis Data Stream, starting out initially with one shard.

1. Login to the AWS Console using the user name, Password, and Open AWS console button provided by this lab.

1. Under Find Services, type in and then click Kinesis.

1. Ensure Kinesis Data Streams is selected, and then click the Create data stream button.

1. For Data stream name enter RawStreamData.

1. For Number of open shards enter 1.

1. Click Create data stream.

Wait for about a minute until the Status of your data stream is Active, at which point it will be ready to accept data streams or a sequence of records.


### Connect to an EC2 and Configure Live Temperature Feeds to the Data Stream

Schedule a Python script to send live temperature feeds using the Kinesis API to the data stream you created in the previous challenge.

1. In the upper-left, click Services, enter EC2 into the search, and click EC2.

1. In the left panel, under Instances click Instances.
Note: You will see an instance named nalyticsEngine in a Running state, which was created for you when you started this lab.

1. Select the instance AnalyticsEngine, click Connect, ensure the EC2 Instance Connect tab is selected, then click Connect.
Note: A new browser tab will open to a Linux command prompt.

1. At the command prompt, enter the following two command, replacing <AWS Access Key ID> and <AWS Secret Access Key> with the CLI CREDENTIALS values provided by this lab.

      export AWS_ACCESS_KEY_ID='<AWS Access Key ID>'

      export AWS_SECRET_ACCESS_KEY='<AWS Secret Access Key>'

Note: For example, the first command would look something like

      export AWS_ACCESS_KEY_ID='AKIASY3GMJRF5PXADOMT'

- Enter cat > sensorstream.py, and paste in sensorstream.py source code, press enter, then press Ctrl+D.
Note: This command creates a script you will next execute, and note there are other ways to do this, such as using vi.

- Run the command python sensorstream.py

Note: This ingests live temperature feeds to your Kinesis Data Stream using the python kinesis connector API. If you get an error that ends with something similar to boto.exception.NoAuthHandlerFound: No handler was ready to authenticate. 1 handlers were checked. ['HmacAuthV4Handler'] Check your credentials, then there was an issue with task 4. Double-check things, and re-do that task.

This will start generating temperature feeds, and you will observe continuous sensor data including "iotValue".


### Monitor Incoming Data to the Kinesis Data Stream

You generated live temperature feeds and connected them to your Kinesis Data Stream using the Kinesis API. You will monitor the incoming traffic being ingested to the Kinesis Data Stream and based on the increase in load you will increase the number of shards.

1. Go back to the AWS Console browser tab.

1. In the upper-left, click Services, enter Kinesis into the search, and click Kinesis.

1. In the left-hand menu click Data streams, then click the RawStreamData link.

1. At the RawStreamData page, if needed, click the Monitoring tab.
Note: There are various Stream metrics which you can scroll down and see, such as Incoming data - sum (Bytes), Incoming data - sum (Count), Put record - sum (Bytes), Put record latency - average (Milliseconds), and Put record success - average (Percent).

1. Hover over the Incoming data - sum (Bytes) panel, in its upper-right click the three vertical dots, then click on View in metrics.

1. In the new browser tab that opens to the Metric page, select a Number graph.
Note: Wait if needed until IncomingRecords is above 1k, which in this scenario will indicate you need more resources to handle the streaming data. The following tasks will show you how to do this by increasing the number of shards.

1. Go back to the browser tab open to the RawStreamData page, and click the Configuration tab.

1. Click on Edit, increase the Number of open shards to 2, then click Save changes.
Note: This will handle a greater amount of streaming data.

1. After about a minute, you will see a panel saying Stream capacity was successfully updated for this data stream.

