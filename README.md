# WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS

For this project, we are going to implement a LAMP stack which is a combination of these technologies - Linux, Apache, MySQL, PHP or Python, or Perl

### SETTING UP LINUX (**L**)
First off, we have to sign into our `AWS management console` and then setup the resources needed for this project. If you do not have an `AWS` account, head over to this [link](https://portal.aws.amazon.com/billing/signup#/start) to sign up for an account.

Launch a new EC2 instance of the `t2.micro` family with the Ubuntu Server 20.04 LTS (HVM) AMI under the `free tier`
![image](https://user-images.githubusercontent.com/22638955/131181775-aea02590-998a-4fac-a99b-9c6434a87c62.png)

![image](https://user-images.githubusercontent.com/22638955/131738526-662463a0-0975-4a02-baa8-b7aaed6b7848.png)

**NB**: Remember to save your `.pem` key when creating your instance.

To [ssh](https://searchsecurity.techtarget.com/definition/Secure-Shell) into my  EC2 instance, I made use of Mobaxterm. You can watch this [video](https://www.youtube.com/watch?v=g8XiC9Q2EEE&t=612s) to learn how to connect to your EC2 instance.

### INSTALLING APACHE AND UPDATING THE FIREWALL (**A**)
Install Apache using Ubuntu’s package manager ‘apt’:
```
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2
```
Verify that `apache2` is running as a Service in our OS
```
sudo systemctl status apache2
```
If apache is running, we should get a message like bwlow:
![image](https://user-images.githubusercontent.com/22638955/131739758-a75ccb2d-9f77-4d1d-a1eb-ed183226871b.png)

To receive traffic to our web server, we need to open `TCP port 80` in our EC2 instance/machine. `TCP port 80` is the default port that web browsers use to access web pages on the Internet. We would go into our AWS console, security group for our EC2 instance, and then edit inbound rules. End result should be like below:
![image](https://user-images.githubusercontent.com/22638955/131740300-b29c177b-d22b-41ae-a04e-380f1b2e7c59.png)

Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

First, let us try to check how we can access it locally in our Ubuntu shell, run:
```
curl http://localhost:80
or
curl http://127.0.0.1:80
```
The `curl` command above is used to request our apache HTTP server on port 80 (specifying no port works also). For the first curl command, we tried accessing our server via the DNS name, and the second curl command is accessing by the IP address. Running the aove command gives us an output like below - 
![image](https://user-images.githubusercontent.com/22638955/131741933-36c2b819-937f-4ba2-abd0-6e7520829200.png)

We would now be testing to see if our apache server can respond to requests from the internet:
```
http://<Public-IP-Address>:80
```
![image](https://user-images.githubusercontent.com/22638955/131742187-06457df1-690b-4dd3-94af-0bffb23adabb.png)

Seeing the above page means our web server is correctly installed and accessible through our firewall.






Thanks to [darey.io](darey.io) for the opportunity to get into the DevOps sphere and also granting me access to projects like these.
