## Implement a Data Ingestion Solution Using Amazon Kinesis Video Streams

### Create a Kinesis Video Stream for Media Playback

Globomantics is an analytics firm which handles computer vision projects for object detection, image classification, and image segmentation. Your role as a Data Architect is to stream real time video feeds from a source to AWS for further analytics. You'll create a Kinesis Video Stream where live video feeds will later be ingested

1. Log in to the AWS Console using the user name, Password, and Open AWS console button provided by this lab.

1. Under Find Services, type in and then click Kinesis Video Streams.

1. Click on Create.

1. For Video stream name enter VP52M8OQZ10HOQRB, and click on Create video stream.

You will be at the Video streams page for VP52M8OQZ10HOQRB, and in the Video stream info tab the Status will be Active.


### Configure Java SDK Producer Library to Stream Video Feeds


1. In the upper-left click Services, then type in and click on EC2.

1. Click on Instances (running) and select the instance PluralSightAnalyticsEngine.

1. Click on Connect, and at the EC2 Instance Connect tab click Connect.
Note: A new browser tab (or window) will open to a Linux command prompt. The EC2 was created for you when you started this lab, and the OS is Ubuntu.

1. At the command prompt enter git clone https://github.com/ps-interactive/lab_aws_implement-data-ingestion-solution-using-amazon-kinesis-video-streams.git
Note: This clones the Amazon Kinesis Video Streams Producer SDK at the path /home/ubuntu.

1. Now enter cd lab_aws_implement-data-ingestion-solution-using-amazon-kinesis-video-streams/ 

Enter the following commands:

       sudo apt update -y 

       sudo apt install maven -y

       sudo apt install default-jdk -y

       sudo apt install git-all -y

Note: Enter the commands in the given order. They install the required applications to build and run the producer.

- To compile and assemble the producer, enter the command mvn clean compile assembly:single

- Replace <Access Key ID> and <Secret Access Key> with the values under this lab's CLI CREDENTIALS section, then run the following command:

            java -classpath target/amazon-kinesis-video-streams-producer-sdk-java-1.11.0-jar-with-dependencies.jar -Daws.accessKeyId=<Access Key ID> -Daws.secretKey=<Secret Access Key> -Dkvs-stream=VP52M8OQZ10HOQRB -Djava.library.path=/home/ubuntu/lab_aws_implement-data-ingestion-solution-using-amazon-kinesis-video-streams/src/main/resources/lib/ubuntu/ com.amazonaws.kinesisvideo.demoapp.DemoAppMain

DEBUG lines will be output, indicating the creation of a continuous flow of video frames to the video stream you made in the last challenge.


### Check the Media Playback for the Kinesis Video Stream Created

1. Back in the AWS Console browser tab, in the upper-left click Services, then type in and click on Kinesis Video Streams.

1. In the left-hand menu click Video streams, then click on the VP52M8OQZ10HOQRB link.

1. Expand the Media playback section.

You'll observe real time video feeds from the producer library, which you will see as a video of a building with passing traffic.
