# lemp_stack_on_AWS_EC2
In this project you will implement LEMP stack, but with an alternative Web Server – NGINX and PHP.

Preparing prerequisites
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

Sign in to AWS free tier account and create a new EC2 Instance of t2.nano family with Ubuntu Server 22.04 LTS (HVM) image. 

Open an SSH client.

Locate your private key file. The key used to launch this instance is lamp_server_key.pem

Run this command, if necessary, to ensure your key is not publicly viewable.
 chmod 400 lamp_server_key.pem

Connect to your instance using its Public DNS:
 ec2-44-201-161-161.compute-1.amazonaws.com

Example:

 ssh -i "lamp_server_key.pem" ubuntu@ec2-44-201-161-161.compute-1.amazonaws.com
