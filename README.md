# Replication
# 1. Launching Amazon EC2 Instance
### 1.
Create an Amazon virtual server by creating an AWS account. You will be creating a server that will provide you with access to GPUs, which will be required to run the neural networks training and testing in seconds instead of hours or days.

Beware that some of these steps may require permissions from Amazon, including the account set up and EC2 Limit increase. I recommend skimming the whole guide to get an idea of where you might have to factor in waiting time.
### 2.
Go to your [EC2 Management Console](https://aws.amazon.com/console/), click the `Limits` link on the top of the menu on the left. Locate `Running On-Demand p3.2xlarge instances` and click `Request limit increase`. Under `Requests`, select `US East (Ohio)` for `Region`, and `p3.2xlarge` for `Primary Instance Type`, and `2` for `New limit value`.
### 3. 
Wait for Amazon to approve of the limit increase. This might take a day or two.
### 4.
Go to `Key Pairs` under `Network & Security` on the left side menu at your [EC2 Management Console](https://aws.amazon.com/console/). Click `Create Key Pair`, give your key an easy name to type and remember, click `create`. Your `.pem` key file should automatically download. Save it to a directory you are sure to remember.
### 5.
Go to your [EC2 Management Console](https://aws.amazon.com/console/), click <Launch Instance> and search for the <Deep Learning AMI (Ubuntu) Version> and click <select>. Within the list of instance types that comes up, select <p3.2xlarge> and click <Review and Launch> at the bottom. Click <Launch>. Select the key pair you created in **step 4**, check the box and click <Launch Instances>.
### 6.
Your new instance should now appear in your [EC2 Management Console](https://aws.amazon.com/console/). From now I will assume </XPATH/> refers to the path where a certain file is stored (e.g. </XPATH/key.pem> refers to the file path of your Amazon key <.pem> file. To launch your instance:
``` cd /XPATH/<your key>.pem 
```