# ACIT 2420 Week 12 Lab

## Serving an HTML Document and Creating a Firewall
### Team Member

+ **Simon Freeman**
  + *A01074210*
+ **Nai Yen Lin**
  + *A01320713*

## Directory Overview
![image_directory_overview](Images/directory_overview.png)

## Setup
Create a new droplet in Digital Ocean called **web-one**

This will be the hostname for your server.

All the files created should be done in your WSL, and brought over to your server.

## Step 1

Install **NGINX** with the following command:
  ```bash
  sudo apt update && sudo apt install nginx 
  ```

You will be prompted to continue after being informed that additional disk space will be used. Type **Y**, and press enter to continue.

![step1_nginx_install_yes](Images/step1_nginx_install_yes.jpg)

Afterwards, you will be taken to a screen asking which services should be restarted. Press tab to go to ok, and hit enter.

![step1_press_ok](Images/step1_press_ok.jpg)

You should see a message similar to image below once Nginx is installed:

![image_step_1nginx_install_ok](Images/step1_nginx_install_ok.jpg)

## Step 2

Create a simple **HTML** file called ***index.html***

Here is sample code shall look like.  
  ```html
    <!DOCTYPE html>
    <html lang="en">
    <html>
        <head>
            <meta charset="UTF-8" />
                <title>Example Site for 2420</title>
        </head>
        <body>
            <h1>Success!</h1>
            <h2 style="color: red;">All your internets are belong to us!</h2>
        </body>
    </html>
  ```

Screenshots of command are provided below.
  ```bash
  vim index.html
  ```
![image_step2_vim_index.html](Images/step2_vim_index.html.jpg)



## Step 3

Write a **NGINX server block**

Here is sample code to create the file:
  ```bash
  vim [ip_address_of_your_host]
  ```

Here is sample code for server block:
  ```bash
    server {
        listen 80;
        listen [::]:80;

        root /var/www/[ip_address_of_your_host]/html;
        index index.html;

        server_name [ip_address_of_your_host];

        location / {
                try_files $uri $uri/ =404;
        }
    }
  ```

Screenshots of codes in *sever block* are provided below.

![image_step3_creating_server_file_wsl](Images/step3_creating_server_file_wsl.jpg)


## Step 4

Upload the files to **your server** and move them to **appropriate directories**

Use stfp to move files. To access your sftp, enter the following command:

  ```bash
  stfp -i ~/.ssh/[ssh_key_file] [your_destination_host]@[your_destination_ip_address]
  ```
To move your file to your server, enter the following command:
```bash
put [name of your file]
```
Screenshot of a successful transfer.

![image_step4_sftp_Put](Images/step4_sftp_Put.jpg)

Make **directory** for your ***index.html*** inside your server:
  ```bash
  sudo mkdir -p /var/www/[your_server_ip_address]/html
  ```

Screenshot is available.

![image_step4_mkdir_html](Images/step4_mkdir_html.jpg)

Move your ***index.html*** to appropriate directory in your server.
  ```bash
  sudo mv index.html /var/www/[your_server_ip_address]/html
  ```

Move your ***server file*** to appropriate directory in your server.
  ```bash
  sudo mv [your_server_file_name] /etc/nginx/sites-available/
  ```

Screenshot is available.

![image_step4_move_server_file](Images/step4_move_server_file.jpg)


Check your **nginx syntax** with the following command to make sure there are no errors in your configuration:
  ```bash
  sudo nginx -t
  ```

If successful, it should look like the screenshot below.
![image_step4_nginx_syntax](Images/step4_nginx_syntax.jpg)


Verify permissions are set correctly by checking in the directory of the file with the following command:



  ```bash
  ls -l
  ```

Permissions should look like the screenshot below:

![image_step4_execute_permissions_nginx](Images/step4_execute_permissions_nginx.jpg)


Create a **Symbolic link** for related files (server block file)
  ```bash
  sudo ln -s /etc/nginx/sites-available/[your_server_ip] /etc/nginx/sites-enabled/
  ```

Screenshots are available.

![image_step4_symbolic_link_create](Images/step4_symbolic_link_create.jpg)

![image_step4_symbolic_link_created](Images/step4_symbolic_link_created.jpg)


## Step 5

Restart **nginx service** before enabling it

To restart and enable your nginx service, use the code provided below:
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl restart nginx.service
  sudo systemctl enable nginx.service
  systemctl status nginx.service
  ```

Screenshot is available.

![image_step5_restarting_service_nginx](Images/step5_restarting_service_nginx.jpg)


## Step 6

Check if your **HTML document** is active by typing the ***ip address*** you assigned to it in your browser of choice.

Your html document should appear in your browser like in the screenshot provided below:
![image_step6_HTTP_Website](Images/step6_HTTP_Website.jpg)


## Step 7

Setup **firewall** using **UFW**

Enable **UFW** with this command:
  ```bash
  sudo ufw enable
  ```

Screenshot is avalaible below. When prompted to proceed with the operation, type and enter Y to proceed:

![image_step7_sudo_ufw_enable](Images/step7_sudo_ufw_enable.jpg)

Allow HTTP and OpenSSH connections:
  ```bash
  sudo ufw allow "Nginx HTTP"
  sudo ufw allow "OpenSSH"
  ```

If successful, it should look like the screenshots below:

![image_step7_openSSH_allow](Images/step7_openSSH_allow.jpg)

![image_step7_HTTP_Allow](Images/step7_HTTP_Allow.jpg)

If you accidentally add an unintended rule, you can delete it by specifying which row it appears in with the following command:

 ```bash
  sudo ufw enable
  ```
Once you have found which row it appears in, use the following command to remove the fule:

  ```bash
  sudo ufw delete [row index of unwanted rule]
  ```

Example of deleting a rule:    

![image_step7_sudo_ufw_delete](Images/step7_sudo_ufw_delete.jpg)


Check **UFW** status to verify you have the correct rules:
  ```bash
  sudo ufw status
  ```

Your UFW status should look similar to this:
![image_step7_sudo_ufw_status](Images/step7_sudo_ufw_status.jpg)


## Step 8

Check if your **HTML document** is accessible by ***ip address*** and ***SSH***

To verify it is available through ***ip address***, visit the HTML document by typing the ip address into your browser of choice:

![image_step8_HTTP_website_alive](Images/step8_HTTP_website_alive.jpg)

To verify it is available through ***SSH***, attempt to login to your ssh server:
  ```bash
  ssh -i ~/.ssh/[ssh_key_file] [your_ssh_host]@[your_ssh_ip_address]
  ```

![image_step8_ssh_login](Images/step8_ssh_login.jpg)


If you have completed all these steps, everything should be set up correctly.