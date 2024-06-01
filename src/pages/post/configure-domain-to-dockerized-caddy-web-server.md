---
layout: "../../layouts/post.astro"
title: Configuring a Domain to your Dockerized Caddy Web Server
description: Learn the step-by-step process of configuring a domain to your Dockerized Caddy web server, ensuring your website is accessible through a custom domain name and leveraging the power of containerization for efficient web hosting.
dateFormatted: June 1st, 2024
---

## Introduction

In this tutorial, I will guide you through the process of configuring a domain for your dockerized Caddy web server. We will cover the steps for DNS configuration and web server configuration using Caddy and Docker.

Actually, the main steps will be explained in the first section: **A. DNS Configuration**. However, in case you haven’t read my previous tutorial about it (*[Deploying a Static Website on a Cloud Server using Docker & Caddy](https://ricoputra.my.id/docs/deploy-static-web-using-docker-caddy/)*), I will also explain the steps for configuring your web server (**B. Web Server Configuration**).

## Tutorial

### A. DNS Configuration

1. **Purchase a Domain**
   
   Start by purchasing a domain from a domain registrar of your choice. For example, let's use the domain `learncoding.com`.

2. **Access DNS Management**
   
   Log in to your domain registrar's website and navigate to the DNS management section for the `learncoding.com` domain.

3. **Create a DNS Record**
   
   Add a new DNS record with the following details
   - Name: `@` → represents the root domain or the domain itself (not a subdomain)
   - Type: `A` → a type of DNS record used to map a domain name to an IPv4 address
   - TTL: `14400` → stands for “**Time To Live**” — represents the lifespan or **expiration time** of a DNS record. `TTL 14400` means that the DNS record will be considered valid and cached by DNS resolvers and other systems for 14.400 seconds (~ 4 hours).
   - RDATA: `203.0.113.10` → the public IPv4 address associated with the domain name
    
Save the DNS record. It may take some time for the changes to propagate.

By following above configuration to create a new DNS record in your DNS Management page of `learncoding.com`, later user will be able to see the web content configured in your server (`203.0.113.10`) by accessing `learncoding.com` in the browser.


### B. Web Server Configuration

1. **SSH to Server**
    
   Establish an SSH connection to your server using either your domain (if you've done the steps above: DNS Configuration) or the public IP address. 
   
   Use the following command in your terminal:
    
   ```bash
   # using domain
   ssh <username>@<yourdomain.com>
    
   # using public IP address
   ssh <username>@<your public IP address>
   ```
    
   For example, your domain is `[learncoding.com](http://learncoding.com)`, the public IP address is `203.0.113.10`, and the username is `john`. You can modify the SSH script above.
    
   ```bash
   # using domain
   ssh john@learncoding.com
    
   # using public IP address
   ssh john@203.0.113.10
   ```
    
2. **Create and Configure Caddyfile**

   In the server's root directory, create a new file named `Caddyfile` without any file extension (`touch Caddyfile`). Modify the content of the Caddyfile using a text editor (`vi Caddyfile`):
    
   ```yaml
   learncoding.com {
      root * /srv
      file_server
   } 
   ```
    
3. **Create and Configure Docker Compose**
    
   In the same directory as the `Caddyfile`, create a new file named `docker-compose.yml`. Modify the content of `docker-compose.yml` with the following configuration:

   ```yaml
   version: "3.9"
    
   services:
      caddy:
         image: caddy:latest
         restart: unless-stopped
         ports:
            - "80:80"
            - "443:443"
            - "443:443/udp"
         volumes:
            - ./Caddyfile:/etc/caddy/Caddyfile
            - ./yourwebapp:/srv
            - caddy_data:/data
            - caddy_config:/config
    
   volumes:
      caddy_data:
      caddy_config:
    ```
    
4. **Prepare a Static Web Files**
    
   Based on the content written in `docker-compose.yml` file above, the static web files for your website should be stored in a directory named `yourwebapp/` that is located in the same root directory as `Caddyfile`. You can execute `mkdir yourwebapp` in your SSH terminal to create a new `yourwebapp/` directory.
    
   After that, go into the directory, create a new HTML file (`index.html`), and modify the content of that new HTML file.
    
   ```yaml
   # 1. entering the directory
   cd yourwebapp
    
   # 2. create a new HTML file
   touch index.html
    
   # 3. modify the content
   vi index.html
   ```
    
   Write a very simple content to your `index.html`. You follow an example below.
    
   ```html
   <!DOCTYPE html>
   <html lang="en">
      <head>
         <meta charset="UTF-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Learncoding.com</title>
      </head>
      <body>
         <h1>Welcome To Learncoding.com</h1>
      </body>
   </html>
   ```
    
    Save it by pressing `Q` on your keyboard to exit the Vim edit mode. Then, you should type `:q` in your Vim text editor to save the content you’ve written in the HTML file above.
    
5. **Execute Deployment**
    
   Execute the deployment process of your website by executing this script in your SSH terminal.
    
   ```html
   sudo docker compose up -d
   ```
    
6. **Access Your Website**
   
   Finally, after the docker compose process is finished, you can open your domain (`learncoding.com`) in a browser.

Congratulations! You have successfully configured a domain for your dockerized Caddy web server. Your website should now be accessible using your domain name. Enjoy your newly deployed website! 🎉🎉🎉