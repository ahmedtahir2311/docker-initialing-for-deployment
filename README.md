# Docker Installation in Ubuntu

## Step 1 Installing Docker

First, update your existing list of packages:
$ sudo apt update

Next, install a few prerequisite packages which let apt use packages over HTTPS:
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common

Then add the GPG key for the official Docker repository to your system:
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

Add the Docker repository to APT sources:
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"

Next, update the package database with the Docker packages from the newly added repo:
$ sudo apt update

Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
$ apt-cache policy docker-ce

You’ll see output like this, although the version number for Docker may be different:

### Output of apt-cache policy docker-ce

docker-ce:
Installed: (none)
Candidate: 18.03.1~ce~3-0~ubuntu
Version table:
18.03.1~ce~3-0~ubuntu 500
500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 18.04 (bionic).

Finally, install Docker:
$ sudo apt install docker-ce

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:
$ sudo systemctl status docker

The output should be similar to the following, showing that the service is active and running:

### Output

● docker.service - Docker Application Container Engine
Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
Active: active (running) since Mon 2021-08-09 19:42:32 UTC; 33s ago
Docs: https://docs.docker.com
Main PID: 5231 (dockerd)
Tasks: 7
CGroup: /system.slice/docker.service
└─5231 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. We’ll explore how to use the docker command later in this tutorial.

## Step 2 Executing the Docker Command Without Sudo (Optional)

By default, the docker command can only be run the root user or by a user in the docker group, which is automatically created during Docker’s installation process. If you attempt to run the docker command without prefixing it with sudo or without being in the docker group, you’ll get an output like this:

### Output

docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.

If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:

sudo usermod -aG docker ${USER}

To apply the new group membership, log out of the server and back in, or type the following:

su - ${USER}

You will be prompted to enter your user’s password to continue.

Confirm that your user is now added to the docker group by typing:

$ id -nG

### Output

sammy sudo docker

If you need to add a user to the docker group that you’re not logged in as, declare that username explicitly using:

$ sudo usermod -aG docker username

The rest of this article assumes you are running the docker command as a user in the docker group. If you choose not to, please prepend the commands with sudo.

Let’s explore the docker command next.

# Setting DockerFile in a project

## to check all the available images in the project

$ docker image ls

## setup a Docker File in Project

1. Make a DockerFile

2. Set up a Base Image
   -The base image would be a layer of another imge which in our case is a node image
   -Node image is collection of node modules running on a apline server
   -- To check for your desire node image visit https://hub.docker.com/_/node

3. Set up the Base Directory
   -WORKDIR /app
   --it could be any name

4. To avoid copy all the code First copy Package.Json
   -Copy Package. Json .

5.Installing NPM (will only insatll packages that are new else it will use Cache ) not the first time
-RUN npm install

6. Copy all of your Local Code to Some folder in directory
   -COPY .(all Code in local Directory) .(Directory in Image)

7. setting up env Variables
   -ENV ......
   --copy .env file but put ENV in front of all variable

8. Expose your desired port to image
   -Expose 3000

Note-- Image port then can be reflected to another port

9. RUN the Image
   -CMD ["npm", "start"]

# Set Dockerignore

1. Make a file name .dockerignore

2. copy all the file from .gitignore to .dockerignore
