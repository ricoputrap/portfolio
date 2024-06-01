---
layout: ../../layouts/post.astro
title: Deploying a Static Website on a Cloud Server using Docker & Caddy
description: Discover the process of configuring a web server using Docker and Caddy, empowering you to deploy your static website on a cloud server with ease and efficiency.
dateFormatted: June 1st, 2024
---

## Introduction
When creating a simple static website consisting of HTML and CSS files on our computer, we can view it in a browser by copying the file's address or dragging and dropping the file (typically it is an `index.html`). However, if we want others to access our website on their own devices, we need to take additional steps. One such step is deploying our website on a cloud server. We can use Docker and Caddy for that.

Caddy is a lightweight web server that simplifies the process of serving websites securely. It supports automatic HTTPS, easy configuration with the Caddyfile, and integrates well with Docker. With Caddy, you can easily set up TLS certificates and manage website routing and file serving.

## Tutorial
In this tutorial, we will walk through the steps to deploy your static website on a cloud server using Docker and Caddy. Let's get started!

1. **SSH to the Cloud Server**
   
   To begin, SSH into your cloud server using either the IP address or domain name. If the domain is not yet configured, you can use the IP address temporarily. For example:

   ```bash
   ssh <username>@<public IP address>
   ```

   Replace `<username>` with your actual username for the cloud server and `<public IP address>` with the server's public IP address. For example:

   ```bash
   ssh john@203.0.113.10
   ```

   This command establishes a secure shell (SSH) connection to your cloud server, allowing you to execute commands remotely.


2. **Install Docker**
   
   If Docker is not already installed on your cloud server, you need to install it by following the tutorial in this article: [*Install Docker Engine on Ubuntu*](https://docs.docker.com/engine/install/ubuntu/)


3. **Install Caddy using Docker**
   
   Configure the `docker-compose.yml` file to install Caddy using Docker. Specify the desired version of Caddy and define the volumes and ports. Here is an example configuration:
   
   ```yaml
   version: "3.7"
   
   services:
     caddy:
       image: caddy:<version>
       restart: unless-stopped
       ports:
         - "80:80"
         - "443:443"
         - "443:443/udp"
       volumes:
         - $PWD/Caddyfile:/etc/caddy/Caddyfile
         - $PWD/site:/srv
         - caddy_data:/data
         - caddy_config:/config
   
   volumes:
     caddy_data:
     caddy_config:
   ```


4. **Configure the Caddyfile**
   
   Create a `Caddyfile` in the same directory as the `docker-compose.yml` file. Use a text editor to define the configuration for serving the static website. Run `touch Caddyfile` to create a new file and run `vi Caddyfile` to write configuration in the file. Here is a minimal configuration example:
   ```
   yourdomain.com {
    root * /path/to/your/website
    file_server
   }
   ```
   - **`yourdomain.com`**: This is the hostname or domain name for which you're configuring the server block. Replace it with your actual domain.
   - **`root * /path/to/your/website`**: This directive sets the root directory where your website's files are located. Replace **`/path/to/your/website`** with the actual path to your website's files within the Caddy Docker container.
   - **`file_server`**: This directive enables Caddy's built-in file server, allowing it to serve the static files present in the specified root directory.


5. **Adjust the content of `Caddyfile`**
   
   Based on the default configuration in the `docker-compose.yml` above, the root directory for your website files is `/srv` within the Caddy Docker container. Replace `/path/to/your/website` with `/srv`.
   
   If your domain name is `google.com`, replace `[yourdomain.com](http://yourdomain.com)` with `google.com`.
   
   Here is the revised version of the `Caddyfile`:
   
   ```
   google.com {
       root * /srv
       file_server
   }
   ```


6. **Store Website Files**
   
   Store the static files of your website in a directory on the server, which will later be mounted to the `/srv` directory within the Caddy Docker container. Based on the `docker-compose.yml` file above, the directory for your website files is `./site`.
   
   Typically, the minimum requirement is an `index.html` file. You can also include additional supporting files such as CSS, JavaScript, or images.

   If you're working with a React application, run `npm run build` on your local machine and upload the resulting files to the server directory.


7. **Run Docker Compose**
   
   Run the Docker Compose command to start the Caddy server and deploy your static website. In your SSH terminal, navigate to the directory where your `docker-compose.yml` file is located. Then, execute the following command:
   ```
   docker-compose up -d
   ```

   This command will start the Caddy server in detached mode (-d flag), allowing it to run in the background. It will use the configuration specified in the docker-compose.yml file to create and manage the Caddy container.

   If everything is set up correctly, you should see the Caddy container start up and the static website files being served. You can verify this by accessing your domain in a web browser. The website should now be accessible to everyone through their web browsers.


### Conclusion
By following the steps outlined in this tutorial, you can successfully deploy a static website on a cloud server using Docker and Caddy. Ensure that your domain is properly configured, and your website will be accessible to users through their browsers.