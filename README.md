# WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS

For this project, we are going to implement a LAMP stack which is a combination of these technologies - Linux, Apache, MySQL, PHP or Python, or Perl

### SETTING UP LINUX (**L**)
First off, we have to sign into our `AWS management console` and then setup the resources needed for this project. If you do not have an `AWS` account, head over to this [link](https://portal.aws.amazon.com/billing/signup#/start) to sign up for an account.

Launch a new EC2 instance of the `t2.micro` family with the Ubuntu Server 20.04 LTS (HVM) AMI under the `free tier`
![image](https://user-images.githubusercontent.com/22638955/131181775-aea02590-998a-4fac-a99b-9c6434a87c62.png)

![image](https://user-images.githubusercontent.com/22638955/131182624-732eb5ca-ac03-4a4d-9447-d2f9affd41a1.png)

**NB**: Remember to save your `.pem` key when creating your instance.

To [ssh](https://searchsecurity.techtarget.com/definition/Secure-Shell) into my  EC2 instance, I made use of Mobaxterm. You can watch this [video](https://www.youtube.com/watch?v=g8XiC9Q2EEE&t=612s) to learn how to connect to your EC2 instance.

### INSTALLING APACHE AND UPDATING THE FIREWALL (**A**)




Thanks to [darey.io](darey.io) for the opportunity to get into the DevOps sphere and also granting me access to projects like these.
