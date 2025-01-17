# Docker-Study-Notes

**Greetings! Now that I'm finally wrapping up my Linux essentials studies, I'll be moving over to Docker for a while. Realistically, I'll be studying Python in January but I'll be doing Docker here and there to get the hang of it. It's interests me more than Python but less than Terraform. I think I need to start here first though. So once again, I'll be publicly documenting my notes. I hear Cloud and DevOps jobs rely heavily on Docker so we need to learn this too! I won't be taking any certifications for this as of yet.**

## 01.03.2025

**Today's Topics**

* Docker - Deep Dive

Lab 4 - Tagging and Pushing Images to DockerHub

I don't have a Docker Hub account but this lab was exactly what it says in the title. Pretty easy. 

Lab 3 - Creating Images Using a Dockerfile

So we built a multi-stage container here which I still don't completely understand the purpose of this (yes, make the image smaller and use and/or discard artifacts) but I went ahead and tried to make one to the best of my ability. They gave us the instructions as to what to put in the Dockerfile but not the actualy commands layers. So I went and did that on my own. I actually got about 80% of it right but I've also seen this multi-stage file before. I missed a few formatting things like the quotation marks around the ENV VAR value and I forgot to put a label on it. There were two other things but I think I overall get the gist of how these are configured. 

Lab 2 - Docker Volumes

This lab was also pretty straightforward. We had to create a Docker volume and mount it during the docker container run process. I've never done the mount version. Only the persistent storage version of the volume. But that was pretty much the entire lab. I'll inspect the mount after the container starts. 


Lab 1 - Docker Networking

We created a MySQL database that needs to communicate with an NGINX server so we made containers for both. We want the MySQL container to be on the localhost network and the NGINX container to be on both the frontend network and the localhost networks. So we used `docker network create <network name>` to create the localhost and frontend bridge networks. We assigned both containers to these networks using the `docker run --network` command and flags. Since we didn't have these container images on our local host, Docker went and fetched the images from Docker Hub. Eventually, we used the `docker network connect localhost frontend-app` command to attach the NGINX container to both networks. We did `docker inspect` to check the network configs for the NGINX container. Both networks and IPs were listed here. 

Pretty straightforward lab. 

_________________

We can save and load images into a tar file as well. Use `docker image save` and `docker image load` as well. 

You can do `docker container top` to see the processes running in your container. 

Use the `--restart` flag to set the policy when the container stops. You have no (don't automatically restart), on-failure (only restart if the container exits do to an error), always (always restart no matter what), and unless-stopped (always restart unless the container was manually stopped). 

The instructor is showing me portainer.io and Watchtower. I did a few labs with Watchtower. Apparently, portainer.io is a GUI version of managing containers. Nice. 


## 01.02.2025

**Today's Topics**

* Docker - Deep Dive

I had to review environment variables for both Linux and Docker. Environment variables can be persistent and used globally in any shell for that server CLI. Familiar env vars are $PATH and $HOME. You can define your own env vars though. We do this in our Dockerfile so we can create our own env vars in our container runtime. You can have these predefined in the Dockerfile or just set them in the file so you'll be forced to pass a value to them in the `docker build --env` command. Just set them as default for good practice. 

You can also have a volume created for you in the `docker build` process by using the `VOLUME` layer in the Dockerfile. I wonder why people don't just do this instead. 

ENTRYPOINT and CMD do the same thing but there are a few differences. I'll have to go back over what they are. Not really seeing the application point of it even with Chat GPT. 

Lets talk about the `.dockerignore` file. I was wondering why you would need this because I'm like, just don't include the files that you don't want copied over. But say you pull a repo of files for your container from Git. Your GitHub repo might have additional necessary files like a README.md and etc that are useful for documentation but obviously you don't need this when building your container. Say you're pulling repos from GitHub all the time and using them for your container. This will be a lot of manually excluding the files you don't want. So you can compile a `.dockerignore` file to automatically block out those files when creating a container. 

You can build images/containers directly from a tar file. Just a heads up. 


## 01.01.2025

**Today's Topics**

* Docker - Deep Dive

Okay so lets do a refresher on `yum` because lately, a lot of tutorials have been downloading a tool's package repo off of a website and then installing it that way. I'm just used to using `yum install` to do this automatically. So, `yum` is a package MANAGER. It can install, remove, clean up, etc all kinds of packages and dependencies. So when you use `yum install <tool>`, it will check it's repos (online or locally) to see if it can find the package and all it's dependencies. It will automatically download the dependencies. Then it will install the package. When you do `yum remove`, it will remove the package but not it's dependencies or configuration files. "The yum-config-manager tool is part of the yum-utils package and is used to manage repository configurations on your system." So I had Chat GPT explain me to why we need to add the Docker repo using the yum-config-manager. First, lets talk about `yum-config-manager`. How is this different from `yum` and why do we need it? Also, why is it packaged with `yum-utils` and why do we need to download this? So `yum` allows us to automatically install, remove, list, and search for packages. It can only search the repos managed by it's distro (so lets say RHEL or CentOS for example). If there are packages not managed by the distro, `yum` will not be able to find it online or locally. So Docker is a third party software. It's not managed by any Linux distro. So `yum` wouldn't be able to find it nor does `yum` allow you to add repos from third-party software sites. `yum-config-manager` does, however, allow you to add repots and much more. 

Why doesn't Docker just put it's repo on the Linux distros? Well, Docker is third-party owned and is in charge of keeping their repos up to date. So it's easier to make things smoother if they just managed their repos. Well why doesn't `yum-config-manager` automatically come with the Linux distro? Well remember. We try to keep Linux distros lightweight so they consume less resources. You may not need `yum-config-manager` for your system so we package it separately. `yum-config-manager` is packaged with the `yum-utils` package. Once you install that, you can use `yum-config-manager` to add repos, disable repos, and view repo configs. It automatically creates the `yum.repos.d` file for your repos as well. Hopefully that clears all of that up. 

___________________

First, uninstall the old versions of Docker (I'm sure there are a list of Docker package apps that you can copy and paste but for example, you have docker-client, docker-client-latest, docker-common, docker-latest, docker-latest-logrotate, docker-logrotate, docker-engine). Then, you need to install yum-utils, device-mapper-persistent-data, and lvm2. Apparently this is still a thing so here: `sudo yum install -y yum-utils device-mapper-persistent-data lvm2`. After you install your Docker repo, enable it and start it using `systemctl`. Then add your user to the Docker group.

Docker checks the local system for a container image and if it doesnt find it there, it will look at the Docker registry online. Docker image - read-only template with instructions for creating a Docker container. Container - runnable instance of an image. Containers can be attached to more than one network. Docker services - scales containers across multiple Docker daemons (hosts). 

Docker client talks to the Docker engine. The Docker client is the CLI that interacts with the Docker daemon. This is basically just the Bash CLI commands you get when you install Docker. So while you're in Bash and you use `docker build`, the docker build part is the Docker client. It's the tool on the host machine. Here's the entire process of creating a container:

![Image](DockerEngineProcess.png)

Use the CLI to execute a command
Docker client uses the appropriate API payload
POSTs to the correct API endpoint
The Docker daemon receives instructions
The Docker daemon calls containerd to start a new container
The Docker daemon uses gRPC (a CRUD style API)
containerd creates an OCI bundle from the Docker image
Tells runc to create a container using the OCI bundle. 
runc interfaces with the OS kernel to get the constructs needed to create a container
This includes namespaces, cgroups, etc
The container process is started as a child process
Once the container starts, runc will exit
The container is up and running. 

___________________

Docker storage location on Linux: /var/lib/docker/<Storage-Driver>/. You need to create the volume first and then create the container. Then, mount the volume inside of the container. Deleting a container does not delete the volume. Use the `docker volume -h` to see all the sub commands for creating volumes. The usual suspects are ls, create, inspect, rm, and prune. 

Seems like we don't need to worry about bind mounts. Lets break this next part down: `-v <VOLUME-NAME>:<TARGET> <IMAGE> `. This is how you mount persistent storage for a container. You use the `-v` flag for volume. The <VOLUME-NAME> is the name of the volume you created using `docker volume`. The <TARGET> is the directory you created ON THE CONTAINER that the container writes to but the data actually goes to the same volume on the HOST. Think of the container directory as more of a shortcut. So the directory is on the container but it goes to the host's directory. The <IMAGE> is just the image you running for the container. 

___________________

Lets talk about the Dockerfile. It's the instructions on how to build an image. You use the `docker image build` command. Every Dockerfile starts with a FROM layer. This creates the base image. COPY adds files from the Docker client's current directory (so whatever the host machine's current directory is). RUN builds your application with make. CMD specifies what command to run within the container. The `.dockerignore` file, you include a copy of the list of files and directories you want excluded and they won't get copied over. 




## 12.31.2024

**Today's Topics**

* Docker - Deep Dive

So we're going to skip the Docker Swarm and Kubernetes section of learn Docker by doing. I'm looking for a cloud role and I think those put me more into DevOps territory which I would like to learn. I just don't think it's mission critical for my success right now. That's being said, we're going to move onto the Docker - Deep Dive course and make our way through that. Then I'll be done with Docker. 


## 12.30.2024

**Today's Topics**

* Learn Docker by Doing

Lab 18 - Using Grafana with Prometheus for Alerting and Monitoring

Okay so this lab was actually pretty cool. We made a bunch of containers including Prometheus and Grafana using Docker Compose. We then access Grafana using port 3000 on the server PIP. We put in our username and password and then added our new Grafana data source named Prometheus which we ended up specifying using our private IP and Prometheus container port number (9090). We then imported our Docker dashboard to Grafana. We had a long JSON file (not sure where this came from) and once we added it, all of our container metrics popped up in the Dashboard! That was super cool. Definitely makes monitoring easier and more graphical. I need to find out what all was in this JSON file. 


Lab 17 - Monitoring Containers with Prometheus

How cool. We finally got to Prometheus. We first created a Docker Compose file that spun up cAdvisor and a Prometheus container. I didn't know Prometheus was a container. But we mapped both Pro and cAdvisor to ports 9090 and 8080, respectively. We also made the Prometheus container dependent on the cAdvisor container (I have no clue what cAdvisor does). Once we ran `docker-compose up -d`, we were able to go to the server PIP on each port to run metrics in Pro and cAdvisor which was really nice. 


Lab 16 - Build Services with Docker Compose

We are building a blogging container using Ghost. Ghost is similar to Word Press. We input some information into the docker-compose.yml file. We had to specify the Docker Compose version which we chose 3. It's the standard format for Docker Compose. We then specified the Ghost and MySQL containers we're going to create. 

Both containers contained the `restart: always` info which says always restart this container when this service stops. We also had port mapping on the Ghost container. We also created persistent volumes for both containers and we specified the file location on the host machine. The Ghost section also has the `depends_on: - mysql` info which says only start the Ghost container after the MySQL container starts. 

The Ghost container section has `database__*` arguments that are the specific format that Ghost requires from a database in order to draw information from that database. This is very specific to Ghost. 

That's pretty much the entire lab. 


Lab 15 - Load Balancing Containers

So we actually used Docker Compose and Swarm to create multiple containers and a load balancer all in one go. The Docker Compose file was in YAML. We also had to configured the nginx.conf file for all of the port connections and such. I'm not very familiar with this file. I'll be honest. Oh I forget, we also specified the network each of the 4 containers (3 web-app containers and an NGINX LB container) would spawn on. We then used `docker-compose up --build -d` to build all the containers. 

We then used Docker Swarm to load balance between these containers. We had to make a docker swarm join token that we pasted onto the workstation server. That became a part of the swarm. The original server was the manager. After that, we ran a `docker service create` command on the manager node and inputted `--replicas=2` in order to make two instances of this nginx-app. 

I actually messed up the final command because I missed the "x" on the "nginx" word at the end of the command. This actually still created the service but it didn't work. Then I couldn't rerun the command. I had to do `docker service ls` to see which service was running and then `docker service rm nginx-app` to remove it. Then I reran the command and it worked. 

Lab 14 - Adding Labels and Metadata

Once again, I don't have a Docker Hub account (might just make one) but overall, we just wanted to add labels and metadata to our containers such as Build Version, Date, and Application name. If we're using Watchtower, the version and date are definitely going to change. This allows for that process to be automatic. 

So apparently arguments (ARG) can be defined during the build process of the container. You actually give the container these values when you do `docker build`. They are only used for the build process and are not stored afterwards. 

In order to store them, you can assign the container LABELS containing the ARG information. LABELS add the metadata. Now there's a specific format the LABELS need to be in. I'll have to see where they got this from. But the LABELS can be viewed later

Also I will say when creating the Dockerfile, I was able to create most of it by myself outside of the ARG and LABELS. 

More notes. So the LABEL format for the lab was `org.label-schema.build-date`. I asked Chat GPT is this arbitrary and they said yes but most people use this format so it's universally accepted. So you do not have to use the above format. You just should. `org.label-schema.*` is really the base format. 

Also, the ARGs were specified in the Dockerfile but were not given a default value. So instead of BUILD_VERSION=v1.1, it was just left blank. Then, the LABEL for that argument was just assigned the variable value of whatever the user puts in for that ARG. This is why this is important. If you specify an ARG in the Dockerfile that was not given a default value (like v1.1), then you HAVE to specify a value during the `docker build` command. If you don't, the build will fail. So in a way the build is forcing you to specify this metadata information or you can't build the image. That's pretty smart. So it feels like double work but it's just making sure Docker pros don't skip over documentation steps. 


Lab 13 - Updating Containers with Watchtower

"Watchtower is a container that updates all running containers when changes are made to the image that it is running." I've never heard of this. It also checks for changes every 30 seconds. 

We basically created an image, pushed it to Docker Hub, created the Watchtower image and tested it out. Then Watchtower pushed the new image to Docker Hub. Pretty cool. Now, I don't have a Docker Hub account so I didn't finish this entire lab but what I did do was go over all the FROM, RUN, ADD, WORKDIR, and CMD commands to make sure I knew exactly what they did. That was helpful. 

FROM tells you the base image your container will run on. RUN specifies commands to enter WHILE the container is in the build stage. ADD is basically like COPY where you can copy files from your host machine to your container directory tree but apparently ADD can also fetch from URLS and work with tar files. WORKDIR specifies the main directory for your container so you don't have to keep putting the file path. CMD specifies a command to run when the container starts up which is not the same as when it's in the building stage. 

I know there are more but this is a start. 

## 12.29.2024

**Today's Topics**

* Learn Docker by Doing


Lab 12 - Container Logging

I didn't write my notes for this lab right after I did it so I forget a little bit of what we had to do. I remember us manually configuring where we send the logs for our Docker tasks. Log files are automatically created for each container when they are spun up. They are formatted in JSON and stored in `/var/lib/docker/containers/<container_id>/<container_id>-json.log`. Docker has a logging driver that does this but you can specify a different logging mechanism such as syslog to redirect these log messages. 


## 12.28.2024

**Today's Topics**

* Learn Docker by Doing


Lab 11 - Building Smaller Images with Multi-Stage Builds

We added a "build" stage to our container in the previous lab. We decided to build this container in stages which resulted in a slightly smaller image size and a reduced number of layers. I'll have to ask Chat GPT what this is needed for. 

Lab 10 - Dockerize a Flask Application




Lab 9 - Docker Container Networking

Okay we went over Docker networking! So that was pretty cool. I know at the start of the lab, we had 3 networks: bridge, host, and none. I'm not sure if this is just a general thing or something A Cloud Guru did. We saw the networks using `docker network ls` and we created our own network using `docker network create <network name>`. You can assign a container a network by using the `--network <network name>` option when doing `docker run`. We didn't go into breaking up the CIDRs but this was pretty useful. 

So the bridge, host, and none networks come by default on all Docker environments. The bridge network is the default network that all containers are placed in. This allows the containers to talk to each other and each other only. The host network is the network of the host machine running all the containers. If you want a container to not be isolated from the host, you can place it in this network. The none network is for total isolation for a container. The container only has a loopback address. No network connectivity. 

You can use 

`docker network create 
--driver bridge 
--subnet=192.168.1.0/24 
my_custom_network`

to create your own network. This is the basics. You can do more. 


Lab 8 - Building Container Images Using Dockerfiles

We created a Docker file from scratch. It installed HTTPD and ran a bunch of apt updates and things. We made 3 versions of this container image. So if you only do `docker build -t widgetfactory:0.2 .`, Docker will build name your docker image and give it a tag but it will only look for a file name Dockerfile specified in the current directory (hence the dot at the end). You can direct Docker to a different file in a different directory if you want. Keep that in mind. For example, `docker build -t my-image -f MyDockerfile .` will build an image named `my-image` using the Dockerfile `MyDockerFile` in the current directory. 

## 12.27.2024

**Today's Topics**

* Learn Docker by Doing

Lab 7 - Storing Container Data in Azure Blob Storage

We did the same exact thing using Azure. This time, we already had a storage account created. We then got the storage account name and keys exported into our Linux environment. We then downloaed `blobfuse` (Azure Blob Storage FUSE) in order to mount our storage account. The rest of the process was the same. 


Lab 6 - Storing Container Data in Google Cloud Storage

We pretty much did the same thing with GCP. So we needed to export the Google project number and store in our BUCKET variable. We use `gsutils` (Google Storage Utility) to interact with our Google Cloud Storage service. We also us `gcsfuse` (Google Cloud Storage FUSE) to mount our storage bucket. So we went through that entire process. I've never used GCP before so this was new. But it was nice going through the process again to see which parts were native to AWS and which were native to GCP. 


Lab 5 - Storing Container Data in AWS S3

Very nice. So I'm very unfamiliar with AWS although I know that S3 this is your storage solution. We pretty muched logged into the AWS CLI via the Bash CLI. We created a mount point for the S3 bucket and mounted it using s3fs (which stands for S3 File System). This allows you to interact with your S3 bucket like it was a regular part of your local filesystem. We then ran an HTTPD container and wrote some files to it while it displayed a static webpage. The file systems were automatically added to the S3 directory. Pretty cool. Well, convenient. 


Lab 4 - Storing Container Data in Docker Volumes

Some containers will have a storage volume created for them. Containers like NGINX wont but containers for Postgres will. Docker will automatically assign these volumes a volume name and it will be located on the hosting system under `/var/lib/docker/volumes/`. You can view these volumes using `docker volume ls`. You can also give your volume a name using `docker volume create <volume name>`. If you use the `--rm` command when running a container, your volume will be deleted once you stop running the container. You can use `docker volume prune` to clean up the unused volumes. We also went over how to backup different volumes but I'll have to revisit this. 



## 12.26.2024

**Today's Topics**

* Learn Docker by Doing

Okay so this entire course is labs and I was told the prereqs were to do Docker Deep Dive amongst other courses I'm unfamiliar with. I was only going to do this and Docker Certified Associate (DCA). Now, all of these labs have solutions so I think I'll still do it just to get Docker commands under my fingers and be aware of certain concepts. But if the labs get too difficult, I'm going to jump ship and only do DCA. Just an FYI.

Lab 1 - Initializing the Docker Environment

Basically all we did was installed Docker and all of its package dependencies. We also added the cloud_user to the Docker group. Simple. 

Lab 2 - Working with Prebuilt Docker Images

Taking random notes. Lets break down this command: `docker run --name httpd -p 8080:80 -d httpd:2.4`. The `docker run` portion just sas run this container. So it will start up the container. The `--name httpd` portion gives this container a custom name. If you didn't include this, The Moby Project would give the container a name. The `-p 8080:80` portion tells everyone to map port 8080 of the local host (the left side is always the local host) to the port 80 of the container (right side is always the container). The `-`d part says run in detached mode so the container is running in the background and you aren't seeing all the log messages. Just frees up the terminal. The `httpd:2.4` portion just says the image and version you want to run. So it's basically the object. 

Now, everything was pretty straightforward but I don't understand why we ran a copy of these websites in HTTPD and NGINX. That was confusing to me. 

Detour,
curl (short for Client URL) is a command-line tool used to send HTTP/HTTPS requests and interact with web servers or APIs. It's very versatile and supports various protocols like HTTP, HTTPS, FTP, and more.

Lab 3 - Handcrafting a Container Image

This one was definitely harder to follow. We created our imaages and containers but we went into the containers and pulled down custom website code for each one. Not sure why we removed certain directories but we eventually ran all three containers and curled to each on there respective TCP port. 






## 12.25.2024

**Today's Topics**

* Docker Quick Start

Make sure you add the Docker users to the Docker group if you'll be having people outside of the root user running Docker commands. You can make it one of their secondary groups. Just use `usermod` and make sure you use the -a option to append the new Group to the user instead of completely erasing the user's secondary group (or primary group). 

Remember, you have to enable and then start Docker. Then you may have to log out and log back into your server. 

The Docker Hub has a registry where you can host your containers in their repository (whether private or public). 

It seems like you can either do `docker image pull` or `docker pull`. I wonder if there is a difference. Chat GPT said `docker image pull` is more explicit and that `docker pull` is pretty much shorthand for `docker image pull`. Docker tags are to give you some additional information on the image or container such as the version. The Image ID is the unique identifier for that container or imaage. 

Let's breakdown the different components of the Dockerfile. You can call the file whatever. But the first section is FROM. Foe example, RUN ubunt:16.04. This will determine your OS image and and version. Then under that, you can tell the Dockerfile to run commands. So do RUN apt-get update for example. RUN apt-get install -y python3 for another example. This will create a container running on Ubunte 16:04 that will update it's packages and install Python3. Try to use the -y option because those additional CLI prompts can break your container deployment. You can also give your Dockerfile a LABEL which is bsically a description. 

Afterwards, use `docker build .` which will build up that dockerfile you created. The "." just says build it from the current directory. You can probably specifiy a path here I would assume. The basics were cool. I'm sure it gets much more complicated. 

To run the container, use `docker container run --name <name of the container> <Image ID>'. I think you would put the Image ID just in case your container doesn't have a name but I think including the name will give it a name. You actually don't have to use the entire Image ID number. Just enough to distinguish the container from the other containers. 

For container management, use `docker images` and `docker container ls`. You can also use `docker ps -a` to see which your containers started or stopped. 

The difference between `docker container run` and `docker container start` is run will create the new container and start it even if the container doesn't exist. Start will start a stopped container that already exists. 

You can do `docker container rm <container name>` to remove a container. 

You can log into your Docker account via the Linux CLI using `docker login`. After you're logged in, you can give your local containers tags and push the container to your online repo using `docker push <container name>:<version>`. You can retrieve it from your online repo using `docker pull <same info>`. 

Not sure the entire point of Docker volumes. Now obviously, your containers may use up storage and create artifacts that need to be collected. I'm going to ask Chat GPT though. 


## 12.24.2024

**Today's Topics**

* Essential Container Concepts 

Chapter 7 - Apache Mesos

I'm not learning this lol. I don't see the point. I've never even heard of it. 

_______________
Chapter 6 - Kubernetes

We, the administrator, talks to the Kubernetes master that talks tothe worker node. You can have multiple worker nodes. 

Pod - a group of containers. It can be one container or multiple containers. The container or containers live in a pod. 

Pods can have a shared set of resources if they want like a namespace or network. You can do this per pod instead of per container. 

Worker nodes used to be referred to as minion nodes. Just a random note. 

Kubelet - the agent for managing the node and communication with master. Remember, Docker is a container runtime. So is LXC and other options. 

Kube-proxy - it runs the network proxy on each node. Just think networking and API calls. 

Kubernetes also has its own namepsaces. Don't get too involved with this. We'll go back over this when we get to Kubernetes (if we even do this). 

_______________
Chapter 5 - Docker Swarm

The Docker version of Kubernetes. You can do all of this from the Docker CLI so the learning curb is probably smaller. 

We went over a few commands to deploy multiple containters at once using `docker service`. This is new to me so I'm not necessarily taking notes. Plus, I planned on using Kubernetes instead of Docker Swarm. Cool stuff though. 

Docker image - A lightweight, standalone, read-only template for creating containers. Think of it as the blueprint to create your container. 

You run different Linux services/servers like NGINX and HTTPD on containers because it separates the service from the hosting system. You can run it all in a container similar to a microservice. So, the (Docker) imgae is all the information needed to spin up that specific (Docker) container. The container (which could be a custom container, NXGINX, HTTPD, Python, whatever) runs in an isolated environment separate from the host system. You don't want any of this running on the host system if you're interested in containers. Obviously you can but this defeats the purpose of containers. 

_______________
Chapter 4 - Docker

Docker was open-sourced in 2013. DotCloud created Docker as a Python script. It eventually become a lot of microservices so they had to regroup. 

Docker is an application that was written to make it easier to run other applications in containers. Docker daemon > REST API > Client Docker CLI. It's not a replacement for LXC. It's just trying to bring in those powerful functionalities. Think of applications more than machines for Docker. You can also track versions of containers as well. 

When we install Docker, we install the Docker daemon (docker.d). The REST API sends communication to the docker.d via the Docker CLI. We don't usually write our own APIs for Docker. 

"The instructor installed yum-utils, lvm2, and device-mapper-persistent-data before starting the Docker installation because these packages are prerequisites or utilities that enhance the Docker setup process. Here's a breakdown of why each package is used:"

Duly noted. The instructor also used `yum-config-manager -add-repo <Docker URL>`. I still need to get more familiar with the different parts of the yum command but it seems like it's just pulling the Docker install repo down but not actually installing it yet. Yeah I see it's in /etc/yum.repo.d/

Now they're going to install it. I feel like this is a way to install the Docker version that you want specifically. You don't necessarily have to do it this way. You can eventually just use `systemctl` to enable Docker. You can test if Docker is working by running the container `docker run hello-world` which just runs the container hello-world. The container just prints hello world. 

Use `docker image pull <image name>:<image version>` to grab an image from the Docker Hub. You can use `docker history <image name>` to see some info on your image. Use `docker images` to see all of your images. 


Lab 1 - Installing Docker

We're going to do exactly as the title of the lab. Lets install the necessary package dependencies and then download the Docker repo that we want (make sure it's in /etc/yum.repos.d). Then we'll install it and run the hello-world container for proof. 

Lab 2 - Working with Docker Images

We just used `docker image pull` for a few different services. Very straightforward lab. 

_______________
Chapter 3 - LXC / LXD

LXD - Linux Container Daemon

`exec` stands for execute. So execute the following command. Also, you may need to specify `/bin/bash/` because some containers don't automatically assume you want Bash as your shell environment. 

Think of LXC as lightweight VM replacement (more general-purpose), not so much a container. It's somewhere in the middle. Podman and Docker are more in common than LXC and Docker. 

Lab - Installing LXC/LXD
Pretty straightforward although I need to get used to using Ubuntu and not RHEL or CentOS. We installed `lxd` using `snap` and then grabbed an image using `lxc launch`. Afterwards, we ran the container using `lxc exec <container name> -- /bin/ash` which I've never used the ash shell environment. It still worked similar to Bash. 

_______________
Chapter 2 - Components

Lab - Creating a Chrooted Environment
We copied a bunch of files into a self-made `/home/elba` directory. I guess this would be the place for the `chroot` environment. I'm not sure why we copied those specific files. I'll have to ask Chat GPT. But eventually, we did `chroot /home/elba /bin/bash` which I guess the second part creates the environment with a Bash shell. Then we were in our new root environment. So that was pretty cool. Very isolated. 


Lets talk about cgroups (control groups). Think of cgroups as resource managers for containers. 

blkio (block io) - lets you limit and measure the amount of I/Os for each group of processes. It allows you to set throttle limits for each of the groups. 

cpu - allows you to monitor CPU usage by a group of processes enabling you to set weights and keep track of usage per CPU

cpuacct - generates automatic reports on CPU resources used by tasks in a cgroup.

cpuset - allows you to pin groups of processes to one CPU or to groups of a process

devices - allows or denies access to devices by tasks in a cgroup

freezer - suspends or resumes tasks in a cgroup. The sigstop signal is sent to the whole container.

memory - sets limits on memory use by tasks in a cgroup and generates automatic reports on memory resources. 

net_cls - tags network packets with a classid that allows the identification of packets originating from a particular cgroup task. (cls - class) (think of QoS)

net_prio - provides a way to set the priority of network traffic dynamically. 



There are six Linux namespaces. What are namespaces? It limits the ability of a process to be able to see a system resource. A Cgroup limits the ability of a process to be able to access a system resource. 

User namespace - isolates security related identifiers such as user IDs and group IDs. So think in terms of UIDs and GUIDs. It has it's only isolated set of them. 

Inter-Process Communication namespace - isolates system resources from a process, while giving processes created in an IPC namespace visibility to each other allowing for interprocess communication. It still allows containers to be able to communicate.

Unix Timesharing (UTS) namespace - allows a single system to appear to have a different host and domain names to different processes. 

Mount namespace - controls the mountpoints that are visible to each container

PID namespace - provides processes with an independent set of process IDs (PIDs). 

Network namespace - virtualizes the network stack. Each container can have its own network. 

Big idea: A namespace doesn’t create a full “virtualized kernel,” but it divides the host kernel’s functionality into isolated slices such as the above namespaces. So each container gets it's own set of the above kernel resources. 

LXC - Linux Containers. 
