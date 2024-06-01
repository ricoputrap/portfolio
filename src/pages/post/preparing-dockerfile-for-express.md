---
layout: "../../layouts/post.astro"
title: Preparing Dockerfile for an Express Web App
description: Get to know of how to prepare a Dockerfile to run an Express web app.
dateFormatted: June 1st, 2024
---

1. Create a directory for the express project
    
    ```bash
    mkdir project
    ```
    
2. Go to the directory
    
    ```bash
    cd project
    ```
    
3. Initialize a Node JS project. Answer all the question prompted on the terminal. Finally a `package.json` file will be created in the `project/` directory
    
    ```bash
    npm init
    ```
    
4. Install `express`
    
    ```bash
    npm install express
    ```
    
5. Create a file for writing the express web app.
    
    ```jsx
    vi app.js
    ```
    
6. Write the code in `app.js` as below
    
    ```jsx
    const express = require("express");
    const app = express();
    
    app.get("/", (req, res) => {
    	res.send({
    		data: "HELLO WORLD"
    	});
    });
    
    app.listen(5000, () => {
    	console.log("START SERVER AT PORT 5000");
    });
    ```
    
7. Create and write a `Dockerfile` as below
    
    ```docker
    FROM node:18-alpine
    
    # create "app" directory in the docker image
    RUN mkdir app
    
    # set "app" directory as the root working directory
    WORKDIR app
    
    # copy some files from the local machine into "app" directory in docker image
    COPY app.js .
    COPY package*.json .
    
    # install dependencies at build stage
    RUN npm install
    
    # tell the develooper if this container will be run at port 5000/TCP
    EXPOSE 5000
    
    # run the program (web app) when the container is running
    CMD node ./app.js
    ```
    
8. Build the image. Let say you are currently opening `root` directory where the `project` directory is located *`root` is the parent directory of `project`*)
    
    ```bash
    docker build -t ricoputra/project-express project
    ```
    
9. Create a container from the image and set the PORT as well
    
    ```bash
    docker container create --name ricoputra_project-express -p 5000:5000  ricoputra/project-express
    ```
    
10. Start/run the container
    
    ```bash
    docker container start ricoputra_project-express
    ```
    
11. Open the web app on the browser by entering this url: `localhost:5000/` and you will see the response below:
    
    ```json
    {
    	"data": "HELLO WORLD"
    }
    ```