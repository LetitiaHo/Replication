# Replication
*Implementing the code by van Schijendel & Linzen (2018)*
# 1. Launching Amazon EC2 Instance
### 1. Create an AWS account
Create an Amazon EC2 server by creating an AWS account. You will be creating a server that will provide you with access to GPUs, which will be required to run the neural networks training and testing in seconds instead of hours or days.

Beware that some of these steps may require permissions from Amazon, including the account set up and EC2 Limit increase. I recommend skimming the whole guide to get an idea of where you might have to factor in waiting time.
### 2. Extend your access to AWS EC2 servers with GRUs
Go to your [EC2 Management Console](https://aws.amazon.com/console/), click the *Limits* link on the top of the menu on the left. Locate *Running On-Demand p3.2xlarge instances* in the list of instances that come up and click `Request limit increase`. Under *Requests*, select `US East (Ohio)` for *Region*, and `p3.2xlarge` for *Primary Instance Type*, and `2` for *New limit value*.
### 3. Wait
Wait for Amazon to approve of the limit increase. This might take a day or two.
### 4. Get a key
Go to *Key Pairs* under *Network & Security* on the left side menu at your [EC2 Management Console](https://aws.amazon.com/console/). Click `Create Key Pair`, give your key an easy name to type and remember, click `Create`. Your `.pem` key file should automatically download. Save it to a directory you are sure to remember.
### 5. Launch your instance
Go to your [EC2 Management Console](https://aws.amazon.com/console/), click `Launch Instance` and search for the *Deep Learning AMI (Ubuntu) Version* and click `Select`. Within the list of instance types that comes up, select *p3.2xlarge* and click `Review and Launch` at the bottom. Click <Launch>. Select the key pair you created in **Step 4**, check the box and click `Launch Instances`.
### 6. Connect to your instance
Your new instance should now appear in your [EC2 Management Console](https://aws.amazon.com/console/). From now I will assume `/XPATH/` refers to the path where a certain file is stored (e.g. `/XPATH/key.pem` refers to the file path of your Amazon key `.pem` file). 

You will need **1.** the file path to your Amazon key `.pem` file from **Step $**. **2.** the *Public DNS* of your instance, you can find this on your [EC2 Management Console](https://aws.amazon.com/console/), when you select your instance, the menu at the bottom will display the properties of your instance, its *Public DNS* will be listed there. You may also find instructions with screenshots [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html).

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
Copy and past the link that comes up into your browser, this will allow you to access your EC2 server from your browser.

# 2. Preparing files
While within your Jupyter notebook, select the *New* menu on the upper right corner and scroll down to click on `Terminal`. All the following lines of code are executed in the commnand line unless stated otherwise.
### 1. Get Pytorch v0.3.0
``` 
pip install http://download.pytorch.org/whl/cu80/torch-0.3.0.post4-cp36-cp36m-linux_x86_64.whl
```
### 2. Get the adaptive LM 
```
git clone https://github.com/vansky/neural-complexity
git checkout tags/v1.0.0
```
You should now have a file in your directory called `neural-complexity`.
### 3. Get the [base LM weights](https://s3.amazonaws.com/colorless-green-rnns/best-models/English/hidden650_batch128_dropout0.2_lr20.0.pt)
Click on the link to download the file. Go back to the original Jupyter notebook directory tab in your browser, upload the LM weights `.pt` file into the `neural-complexity` directory.
### 4. Get the [model vocabulary](https://s3.amazonaws.com/colorless-green-rnns/training-data/English/vocab.txt)
Click on the link to download the `vocab.txt`. Go to your Jupyter notebook directory and upload the file into the `neural-complexity` directory. 
### 5. Get the Extended Penn Tokenizer
Go to your home directory and get the file by running:
```
cd
git clone https://github.com/vansky/extended_penn_tokenizer.git
```
### 6. Get Modelblocks
Go to your home directory and get the file by running:
```
cd
git clone https://github.com/modelblocks/modelblocks-release.git
```
### 7. Get the NaturalStories corpus
Go to your home directory and get the file by running:
```
cd
git clone https://github.com/languageMIT/naturalstories.git
```
### 8. 
Go to the `modelblocks-release` directory then run the following lines of code to split the NaturalStories corpus into separate `.linetoks` files to be used for testing.
```
cd modelblocks-release
mkdir config  
echo '/XPATH/naturalstories/' > config/user-naturalstories-directory.txt  
echo '/XPATH/extended_penn_tokenizer/' > config/user-tokenizer-directory.txt  
make workspace  
cd workspace  
make genmodel/naturalstories.linetoks  
cat genmodel/naturalstories.linetoks | head -n 57 > genmodel/naturalstories.0.linetoks  
cat genmodel/naturalstories.linetoks | head -n 94 | tail -n 37 > genmodel/naturalstories.1.linetoks  
cat genmodel/naturalstories.linetoks | head -n 149 | tail -n 55 > genmodel/naturalstories.2.linetoks  
cat genmodel/naturalstories.linetoks | head -n 204 | tail -n 55 > genmodel/naturalstories.3.linetoks  
cat genmodel/naturalstories.linetoks | head -n 249 | tail -n 45 > genmodel/naturalstories.4.linetoks  
cat genmodel/naturalstories.linetoks | head -n 313 | tail -n 64 > genmodel/naturalstories.5.linetoks  
cat genmodel/naturalstories.linetoks | head -n 361 | tail -n 48 > genmodel/naturalstories.6.linetoks  
cat genmodel/naturalstories.linetoks | head -n 394 | tail -n 33 > genmodel/naturalstories.7.linetoks  
cat genmodel/naturalstories.linetoks | head -n 442 | tail -n 48 > genmodel/naturalstories.8.linetoks  
cat genmodel/naturalstories.linetoks | head -n 485 | tail -n 43 > genmodel/naturalstories.9.linetoks  
cat genmodel/naturalstories.{0,1,2,3,4,5,6}.linetoks > genmodel/naturalstories.fairy.linetoks  
cat genmodel/naturalstories.{7,8,9}.linetoks > genmodel/naturalstories.doc.linetoks
```
