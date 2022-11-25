# ACIT 2420 Week 12 Lab

## Repository

### Team Member

+ **Simon Freeman**
  + *A01074210*
+ **Nai Yen Lin**
  + *A01320713*


## Directory Overview
![image_directory_overview](Images/directory_overview.png)


## Step 1

Install **NGINX** with clear instruction
  ```bash
  sudo apt update && sudo apt install nginx 
  ```
The results shall be similar to image below
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

Here is sample code to create the file.  
  ```bash
  vim [ip_address_of_your_host]
  ```

Screenshots of codes in *sever block* are provided below.
![image_step3_creating_server_file_wsl](Images/step3_creating_server_file_wsl.jpg)


## Step 4

Upload the files to **your server** and move them to **appropriate directories**

Use stfp to move files.  
  ```bash
  stfp -i ~/.ssh/[ssh_key_file] [your_destination_host]@[your_destination_ip_address]
  ```
Screenshot of a successful transfer.
![image_step4_sftp_Put](Images/step4_sftp_Put.jpg)

Make **directory** for your ***index.html*** 
  ```bash
  sudo mkdir -p /var/www/[your_server_ip_address]/html
  ```

Screenshot is avaliable.
![image_step4_mkdir_html](Images/step4_mkdir_html.jpg)

Move your ***index.html*** to appropriate directory
  ```bash
  sudo mv index.html /var/www/[your_server_ip_address]/html
  ```

Move your ***server file*** to appropriate directory
  ```bash
  sudo mv [your_server_file_name] /etc/nginx/sites-avaliable/
  ```

Screenshot is avaliable.
![image_step4_move_server_file](Images/step4_move_server_file.jpg)


Check your **nginx syntax** to with command
  ```bash
  sudo nginx -t
  ```

Screenshot is avaliable.
![image_step4_nginx_syntax](Images/step4_nginx_syntax.jpg)


Check **permissions** on related files
  ```bash
  sudo chmod +x /etc/nginx/sites-avaliable/[your_server_ip]
  ```

Screenshot is avaliable.
![image_step4_execute_permissions_nginx](Images/step4_execute_permissions_nginx.jpg)


Create **Symbolic link** for related files
  ```bash
  sudo ln -s /etc/nginx/sites-avaliable/[your_server_ip] /etc/nginx/sites-enabled/
  ```

Screenshots are avaliable.
![image_step4_symbolic_link_create](Images/step4_symbolic_link_create.jpg)

![image_step4_symbolic_link_created](Images/step4_symbolic_link_created.jpg)


## Step 5

Restart **nginx service**

Codes are provided.
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl restart nginx.service
  sudo systemctl enable nginx.service
  systemctl status nginx.service
  ```

Screenshot is avaliable.
![image_step5_restarting_service_nginx](Images/step5_restarting_service_nginx.jpg)


## Step 6

Check if your **webiste** is avaliable by checking ***ip address***

Screenshot is avaliable.
![image_step6_HTTP_Website](Images/step6_HTTP_Website.jpg)


## Step 7

Setup **firewall** using **UFW**

Enable **UFW** with command.
  ```bash
  sudo ufw enable
  ```

Screenshot is avaliable.
![image_step7_sudo_ufw_enable](Images/step7_sudo_ufw_enable.jpg)

Allowing HTTP and OpenSSH.
  ```bash
  sudo ufw allow "Nginx HTTP"
  sudo ufw allow "OpenSSH"
  ```

Screenshots are avaliable.
![image_step7_openSSH_allow](Images/step7_openSSH_allow.jpg)

![image_step7_HTTP_Allow](Images/step7_HTTP_Allow.jpg)

Optional command for deleting existing rules.
  ```bash
  sudo ufw delete [row index of unwanted rule]
  ```

Screenshot is avaliable.
![image_step7_sudo_ufw_delete](Images/step7_sudo_ufw_delete.jpg)


Check **UFW** status.
  ```bash
  sudo ufw status
  ```

Screenshot is avaliable.
![image_step7_sudo_ufw_status](Images/step7_sudo_ufw_status.jpg)


## Step 8

Check if your **webiste** is accessible by ***ip address*** and ***SSH***

Access by ***ip address***
![image_step8_HTTP_website_alive](Images/step8_HTTP_website_alive.jpg)

Access by ***SSH***
  ```bash
  ssh -i ~/.ssh/[ssh_key_file] [your_ssh_host]@[your_ssh_ip_address]
  ```

Screenshot is avaliable.
![image_step8_ssh_login](Images/step8_ssh_login.jpg)



