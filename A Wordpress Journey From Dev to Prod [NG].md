// A Wordpress Journey From Dev to Prod [NG] //

# Requests:
1. Install a web server, this is a CentOS operating system. Redhat enterprise

2. Migrate the development website to the production server that is located in the running container on the development server

3. Add HTTPS support for the website, this can be done with a self signed key on openSSL

4. Update the vulnerable plugin WP-File Manager

# Part 1: 
<strong> Install a Web Server and Make HTTP Available to the Local Network </strong>

We want to install a web server on our production server, if we do the command `uname -a` we can determine that this a CentOS system. // This can also be found in the banner when you first load the virtual machine. 

Since we are using a CentOS system, we will be using the <strong> yum package manager </strong>. The available web server for CentOS or yum is httpd, so we can install the web server doing the command `sudo yum install httpd`. 

We will then need to start this service using `sudo systemctl start httpd`. 

Great, we installed a web server! // Commands used: `uname -a`, `sudo yum install httpd`, `sudo systemctl start httpd`

# Part 2:


