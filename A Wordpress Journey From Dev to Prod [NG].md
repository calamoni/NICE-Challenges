<p>// A Wordpress Journey From Dev to Prod [NG] //<br>
// This tutorial is for learners, if you need something more copy and paste refer to the TDLR at the bottom // </p>

# Requests:
1. Install a web server, this is a CentOS operating system. Redhat enterprise

2. Migrate the development website to the production server that is located in the running container on the development server

3. Add HTTPS support for the website, this can be done with a self signed key on openSSL

4. Update the vulnerable plugin WP-File Manager // This is easiest done doing first

# Section 1: 
<p><strong> BEFORE DOING THIS STEP PLEASE CONTINUE TO STEP 4 AND RETURN TO THIS STEP UNTIL FURTHER NOTICE </strong><br>
<strong> Install a Web Server and Make HTTP Available to the Local Network </strong></p>

We want to install a web server on our production server(Prod-Web), if we do the command `uname -a` we can determine that this a CentOS system. // This can also be found in the banner when you first load the virtual machine. 

Since we are using a CentOS system, we will be using the <strong> yum package manager </strong>. The available web server for CentOS or yum is httpd, so we can install the web server doing the command `sudo yum install httpd`. // You may be prompted to confirm that you want to install dependencies for this package, you can enter `y` and click enter to proceed. 

We will then need to start this service using `sudo systemctl start httpd`. 

Great, we installed a web server! // Commands used: `uname -a`, `sudo yum install httpd`, `sudo systemctl start httpd`

# Section 2:
<p><strong> Migrate the development website to the production server that is located in the running container on the development server </strong><br>
<strong> This is the most extensive part of this tutorial, please care to pay attention to each step </strong></p>

1. We will log into the development web server(Dev-web), as said in the requests we are migrating the development site to the production server. 

2. We will be using docker to complete this task, if we type the command `docker container ls`, we can see the containers that are listed. There will only be one container running, marked by its container ID.

3. I will not be going to specifics on how containers work in this tutorial, but essentially they are mini operating systems seperated from the host operating system to peform specific tasks like running a web server. 

4. We can enter the container using the command `docker exec -it your_container_id_HERE bash`, ensure you are replacing your_container_id_HERE with your container ID.

5. If you notice, we are in the /var/www/html directory where our wordpress site is located, we will be copying this directory <strong> html </strong> to our home directory in the Development web server. 

6. We can now exit the docker container using `exit` 

7. Ensure that we are in the home directory using `pwd`, it output your directory <strong> /home/playerone </strong> 

8. From here we can use the command `docker cp container:id/var/www/html .` Now when you display your contents in the home directory using `ls` it should show the directory <strong> html </strong>

9. Login to the Prod-Web server and go to the /var/www/ directory and follow the next step to transfer the directory.

<strong> YOU MIGHT NEED TO CHANGE PERMISSIONS ON HTML DIRECTORY BEFORE USING SCP IF YOU GET THE ERROR PERMISSION DENIED. REFER TO STEP #13 IN THIS MODULE </strong> 

10. Now we can transfer this directory that is in our home directory from the Prod-Web server using the command `scp -r playerone@dev-web:/home/playerone/html`. We use the -r because we are not transferring a single file but an entire directory. 

~~Once we have this file transferred, we can then go to the <source> /etc/httpd/httpd.conf </source> and under the Modules section we can include the following:
`LoadModule php5_module modules/libphp5.so`~~

11. Install PHP using `sudo yum install PHP` on <strong> Prod-Web </strong> Server

12. Under the /etc/httpd/httpd.conf config file, there will be a DirectoryIndex, we will add <strong> index.php </strong> next to <source> index.html

13. Change permissions on the html directory in the directory <source> /var/www/html </source>, we will do this by using chmod 0755 html

14. We will cd into the directory `cd html` then change all the files within the directory using `find . -type f -exec chmod 0644 {] \;`

# Section 3:
<p><strong> Add HTTPS support for the website, this can be done with a self signed key on openSSL </strong><br>
This is where we will create a an SSL key/certificate and add it to our config file</p>

We first need to retrieve the ssl.conf file for httpd, we can do this by using `sudo yum install mod_ssl` 

We will go to the Prod-Web and then navigate to home directory cd ~

We will then generate our SSL keys using the following commands `sudo openssl genrsa -out ca.key 2048`, `sudo openssl req -new -key ca.key -out ca.csr` `sudo openssl x509 -req -days 365 -in ca.csr -signkey ca.key -out ca.crt`

Then we will copy these to the necessary directories using `sudo cp ca.crt /etc/pki/tls/certs`, `sudo cp ca.key /etc/pki/tls/private/ca.key`, `sudo cp ca.csr /etc/pki/tls/private/ca.csr` 

We will then go to <strong> /etc/httpd </strong> and we will change the ssl.conf to // SSLCertificateFile <strong> /etc/pki/tls/certs/ca.crt </strong> and //SSLCertificateKeyFile <strong> /etc/pki/tls/private/ca.key </strong> 

# Section 4:
<strong> This step is easier to do first, since you can update the plugin and then refer back to step 1 and continue on. Automatically transferring over the updated plugin </strong> 

Start in Dev-Web server, we can then bash into the docker container using `docker exec -it your_container_name_HERE bash`, ensure that the your_container_name_HERE is the container ID. You can find container IDs using `docker container ls`. There should be only one container available. 

Once we are in the container, we will move our directory to `cd /html/wp-content/plugins/` 

Then we will update the plugin using `wp update wp_filemanager`. 








