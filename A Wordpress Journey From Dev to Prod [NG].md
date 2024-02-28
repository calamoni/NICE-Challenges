<p>// A Wordpress Journey From Dev to Prod [NG] //<br>
// This tutorial is for learners, if you need something more copy and paste refer to the TDLR at the bottom // </p>
# Requests:
1. Install a web server, this is a CentOS operating system. Redhat enterprise

2. Migrate the development website to the production server that is located in the running container on the development server

3. Add HTTPS support for the website, this can be done with a self signed key on openSSL

4. Update the vulnerable plugin WP-File Manager // This is easiest done doing first

# Part 1: 
<p><strong> BEFORE DOING THIS STEP PLEASE CONTINUE TO STEP 4 AND RETURN TO THIS STEP UNTIL FURTHER NOTICE </strong><br>
<strong> Install a Web Server and Make HTTP Available to the Local Network </strong></p>

We want to install a web server on our production server(Prod-Web), if we do the command `uname -a` we can determine that this a CentOS system. // This can also be found in the banner when you first load the virtual machine. 

Since we are using a CentOS system, we will be using the <strong> yum package manager </strong>. The available web server for CentOS or yum is httpd, so we can install the web server doing the command `sudo yum install httpd`. // You may be prompted to confirm that you want to install dependencies for this package, you can enter `y` and click enter to proceed. 

We will then need to start this service using `sudo systemctl start httpd`. 

Great, we installed a web server! // Commands used: `uname -a`, `sudo yum install httpd`, `sudo systemctl start httpd`

# Part 2:
<p><strong> Migrate the development website to the production server that is located in the running container on the development server </strong><br>
<strong> This is the most extensive part of this tutorial, please care to pay attention to each step </strong></p>

We will log into the development web server(Dev-web), as said in the requests we are migrating the development site to the production server. 

We will be using docker to complete this task, if we type the command `docker container ls`, we can see the containers that are listed. There will only be one container running, marked by its container ID.

I will not be going to specifics on how containers work in this tutorial, but essentially they are mini operating systems seperated from the host operating system to peform specific tasks like running a web server. 

We can enter the container using the command `docker exec -it your_container_id_HERE bash`, ensure you are replacing your_container_id_HERE with your container ID.

If you notice, we are in the /var/www/html directory where our wordpress site is located, we will be copying this directory <strong> html </strong> to our home directory in the Development web server. 

We can now exit the docker container using `exit` 

Ensure that we are in the home directory using `pwd`, it output your directory <strong> /home/playerone </strong> 

From here we can use the command `docker cp container:id/var/www/html /home/playerone <strong> OR </strong> .` Now when you display your contents in the home directory using `ls` it should show the directory <strong> html </strong>





