# Replication
*Implementing the code by van Schijendel & Linzen (2018)*
# 1. Launching Amazon EC2 Instance
### 1. Create an AWS account
Create an Amazon virtual server by creating an AWS account. You will be creating a server that will provide you with access to GPUs, which will be required to run the neural networks training and testing in seconds instead of hours or days.

Beware that some of these steps may require permissions from Amazon, including the account set up and EC2 Limit increase. I recommend skimming the whole guide to get an idea of where you might have to factor in waiting time.
### 2. Extend your access to AWS servers with GRUs
Go to your [EC2 Management Console](https://aws.amazon.com/console/), click the *Limits* link on the top of the menu on the left. Locate *Running On-Demand p3.2xlarge instances* in the list of instances that come up and click `Request limit increase`. Under *Requests*, select `US East (Ohio)` for *Region*, and `p3.2xlarge` for *Primary Instance Type*, and `2` for *New limit value*.
### 3. Wait
Wait for Amazon to approve of the limit increase. This might take a day or two.
### 4. Get a key
Go to *Key Pairs* under *Network & Security* on the left side menu at your [EC2 Management Console](https://aws.amazon.com/console/). Click `Create Key Pair`, give your key an easy name to type and remember, click `Create`. Your `.pem` key file should automatically download. Save it to a directory you are sure to remember.
### 5. Launch your instance
Go to your [EC2 Management Console](https://aws.amazon.com/console/), click `Launch Instance` and search for the *Deep Learning AMI (Ubuntu) Version* and click `Select`. Within the list of instance types that comes up, select *p3.2xlarge* and click `Review and Launch` at the bottom. Click <Launch>. Select the key pair you created in **Step 4**, check the box and click `Launch Instances`.
### 6. Connect to your instance
Your new instance should now appear in your [EC2 Management Console](https://aws.amazon.com/console/). From now I will assume `/XPATH/` refers to the path where a certain file is stored (e.g. `/XPATH/key.pem` refers to the file path of your Amazon key `.pem` file. 

You will need **1.** the file path to your Amazon key `.pem` file. **2.** the *Public DNS* of your instance, you can find this on your [EC2 Management Console](https://aws.amazon.com/console/), when you select your instance, the menu at the bottom will display the properties of your instance, its *Public DNS* will be listed there. You may also find instructions with screenshots [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).

To launch your instance, go to terminal and type:
``` 
cd /XPATH/<your key>.pem 
chmod 0400 <your key>.pem
ssh -L localhost:8888:localhost:8888 -i <your key>.pem ubuntu@<Public DNS>
```
You will asked whether you want to continue connecting, answer *yes*. Once you are connected, type:
```
jupyter notebook
```
# 2. Preparing files
While within your Jupyter notebook, select *New* on the upper right corner and scroll down to click on `Terminal`. All the following lines of code are executed in the commnand line unless stated otherwise.
### 1. Get Pytorch v0.3.0
``` 
pip install http://download.pytorch.org/whl/cu80/torch-0.3.0.post4-cp36-cp36m-linux_x86_64.whl
```
### 2. Get the adaptive LM 
```
git clone https://github.com/vansky/neural-complexity
```
### 3. Download the [base LM weights](https://s3.amazonaws.com/colorless-green-rnns/best-models/English/hidden650_batch128_dropout0.2_lr20.0.pt)

