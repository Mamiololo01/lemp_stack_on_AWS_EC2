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
 
STEP 1 – Installing the Nginx Web Server
In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to 
install this package.

Since this is our first time using apt for this session, start off by updating your server’s package index. Following that, you can use apt install to get 
Nginx installed:

sudo apt update
sudo apt install nginx
When prompted, enter Y to confirm that you want to install Nginx. Once the installation is finished, the Nginx web server will be active and running on your 
Ubuntu 20.04 server
To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

sudo systemctl status nginx
If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Clouds!

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is default port that web brousers use to access web pages in the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:



Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

First, let us try to check how we can access it locally in our Ubuntu shell, run:

curl http://localhost:80
or
curl http://127.0.0.1:80
These 2 commands above actually do pretty much the same – they use ‘curl’ command to request our Nginx on port 80 (actually you can even try to not specify 
any port – it will work anyway). The difference is that: in the first case we try to access our server via DNS name and in the second one – by IP address 
(in this case IP address 127.0.0.1 corresponds to DNS name ‘localhost’ and the process of converting a DNS name to IP address is called "resolution"). 
We will touch DNS in further lectures and projects.

As an output you can see some strangely formatted test, do not worry, we just made sure that our Nginx web service responds to ‘curl’ command with some payload.

Now it is time for us to test how our Nginx server can respond to requests from the Internet.
Open a web browser of your choice and try to access following url

http://<Public-IP-Address>:80
 
 
Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:

curl -s http://169.254.169.254/latest/meta-data/public-ipv4
The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.

If you see following page, then your web server is now correctly installed and accessible through your firewall.

<img width="797" alt="Screenshot 2023-03-30 at 13 48 31" src="https://user-images.githubusercontent.com/67044030/228922100-dcc5e3e8-6d30-4e24-8074-a31c86f5b4a2.png">


In fact, it is the same content that you previously got by ‘curl’ command, but represented in nice HTML formatting by your web browser.
 
 You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

To install these 2 packages at once, run:

sudo apt install php-fpm php-mysql
When prompted, type Y and press ENTER to confirm installation.

You now have your PHP components installed. Next, you will configure Nginx to use them
