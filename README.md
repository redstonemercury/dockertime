# It's Docker Time!!!

This repo is intended as a learning tool to teach some basics of the Docker platform through demo files and exercises. It's meant to complement to the slides/talk found here: https://slides.com/jessicamajor/dockertime If you randomly stumbled upon this, please enjoy! Fair warning that you may be better off starting with Docker's tutorials on their site: https://docs.docker.com/engine/userguide/containers/dockerizing/

To play around with this repo and learn some Docker, check out the steps, which are each intended to build on the prior step's concept and teach you a new Docker thing.

Come back to this README after checking out each step to find out what commands to run and things to play with.

## Step 0.5 - Install Docker:

Before you can start working with Docker, you'll need to have it installed. Follow through the installation instructions here: https://docs.docker.com/engine/installation/

## Step 1 - Pull an NGINX image and run a simple webserver:

Ater checking out step1, you should now have two directories called `finn` and `jake` and `index.html` files residing in each of them. We're going to setup a simple webserver to serve some static files.

Let's get an nginx container:

`sudo docker pull nginx`

By default, this will pull the latest nginx container to docker. View all the images you now have:

`sudo docker images`

You should see the nginx image listed now.

So let's do something with this image, and run an nginx container to serve up a file. To do this, we'll use `docker run` and pass the options we care about:

`sudo docker run -p 81:80 --name jovial_jake -v `pwd`/jake:/usr/share/nginx/html:ro -d nginx`

To break this command down:

* `docker run` is the command that starts a container
* `-p 81:80` binds port 81 on your localhost to port 80 on the container; this is nginx so it runs on port 80 (http) or 443 (https) by default
* `--name jovial_jake` is a name for the container so we can easily identify it later; if you do not set this docker will generate you a (usually amusing) one
* `-v \`pwd\`/jake:/usr/share/nginx/html:ro` mounts the `jake` subdirectory in your current working directory to /usr/share/nginx/html in the container, in a read only mode
* `-d` tells this to run in detached mode, so essentially daemonizes it
* `nginx` tells docker run which image to start the container off of

Some of this information you know because of the details given on README for the nginx container found on [Docker Hub](https://hub.docker.com). Like the location in the container to mount your static content to, and for those less familiar with nginx, the port it uses. Some of the information you can infer based on available [docker run](https://docs.docker.com/engine/reference/commandline/run/) options.

Now run docker ps:

`sudo docker ps`

What do you see? Should be something that indicates your container is running!

```
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                         NAMES
6a151168628f        nginx:latest        "nginx -g 'daemon of   12 minutes ago      Up 12 minutes       443/tcp, 0.0.0.0:81->80/tcp   jovial_jake 
```

To verify this is up and running, and serving up our static file, you can either curl down, or use a web browser to navigate to port 81 on your localhost:

`curl localhost:81`

Pretty mathematical!

If you want more practice, go through these same steps for `finn`. Keep in mind that port 81 is taken on your machine, so you'll need to bind it to a different one, and names need to be unique, so don't use `jovial_jake` for `finn`. If you're just seeing a second picture of jake when you navigate to your new port on localhost, make sure you changed the volume to mount in the `docker run` command.

## Step 1.5 - How to destroy containers:

If you screwed up any of the steps above at any points along the way, don't worry! It's really easy to destroy a container and then you can reuse the name.

First, stop the container by name (or if you forgot to set a name, you can use the container hash found in `docker ps`):

`sudo docker stop jovial_jake`

You should see it stopped in `docker ps` but you'll still see it listed as stopped when using the `--all` flag:

`sudo docker ps --all`

You won't be able to reuse it until the container is gone, since stopping and starting containers is non destructive. Remove the container entirely:

`sudo docker rm jovial_jake`

After the container has been removed, you can start one back up with that same name and Docker won't yell at you.
