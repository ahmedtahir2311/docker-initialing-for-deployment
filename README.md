# Docker Installation in Ubuntu

## Step 1 Installing Docker

- First, update your existing list of packages:
  ` sudo apt update`
- Next, install a few prerequisite packages which let apt use packages over HTTPS:
  ` sudo apt install apt-transport-https ca-certificates curl software-properties-common`
- Then add the GPG key for the official Docker repository to your system:
  ` curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
- Add the Docker repository to APT sources: `$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`
- Next, update the package database with the Docker packages from the newly added repo:
  ` sudo apt update`
- Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
  ` apt-cache policy docker-ce`
  You’ll see output like this, although the version number for Docker may be different:
  Output of apt-cache policy docker-ce
  `docker-ce: Installed: (none) Candidate: 18.03.1~ce~3-0~ubuntu Version table: 18.03.1~ce~3-0~ubuntu 500 500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 18.04 (bionic).`

---

### Finally, install Docker:

` sudo apt install docker-ce`

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:

`sudo systemctl status docker`

The output should be similar to the following, showing that the service is active and running:

`● docker.service - Docker Application Container Engine Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled) Active: active (running) since Mon 2021-08-09 19:42:32 UTC; 33s ago Docs: https://docs.docker.com Main PID: 5231 (dockerd) Tasks: 7 CGroup: /system.slice/docker.service └─5231 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. We’ll explore how to use the docker command later in this tutorial.`

---

## Step 2 Executing the Docker Command Without Sudo (Optional)

By default, the docker command can only be run the root user or by a user in the docker group, which is automatically created during Docker’s installation process. If you attempt to run the docker command without prefixing it with sudo or without being in the docker group, you’ll get an output like this:

`docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied. See 'docker run --help'.`

If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:

`sudo usermod -aG docker ${USER}`

To apply the new group membership, log out of the server and back in, or type the following:

`su - ${USER}`

You will be prompted to enter your user’s password to continue.

Confirm that your user is now added to the docker group by typing:

`id -nG`
`Output : sammy sudo docker`

If you need to add a user to the docker group that you’re not logged in as, declare that username explicitly using:

`sudo usermod -aG docker username`

The rest of this article assumes you are running the docker command as a user in the docker group. If you choose not to, please prepend the commands with sudo.

Let’s explore the docker command next.

---

---

# Setting DockerFile in a project

## to check all the available images in the project

`$ docker image ls`

---

## Setup a Docker File in Project

1. Make a DockerFile.

2. Set up a Base Image

   - The base image would be a layer of another imge which in our case is a node image
   - Node image is collection of node modules running on a apline server
   - To check for your desire node image visit https://hub.docker.com/_/node

3. Set up the Base Directory
   `WORKDIR /app`

   - it could be any name

4. To avoid copy all the code First copy Package.Json
   ` Copy Package. Json .`

5. Installing NPM (will only insatll packages that are new else it will use Cache ) not the first time
   `RUN npm install`

6. Copy all of your Local Code to Some folder in directory
   `COPY .(all Code in local Directory) .(Directory in Image)`

7. setting up env Variables

   - copy .env file but put ENV in front of all variable
     `ENV ......`

8. Expose your desired port to image
   `Expose 3000`

&hearts;Note-- Image port then can be reflected to another port

9. RUN the Image
   `CMD ["npm", "start"]`

---

## Set Dockerignore

1. Make a file name .dockerignore

2. Copy all the file from .gitignore to .dockerignore

---

# Docker Commands For images

To check docker version

`docker -v `

To check images

`sudo docker image ls `

To make a local image build

`sudo docker build -t (anyname you want to give to your docker image)`

To check all the images in docker

`sudo docker image -a`

To get ids of docker images

`sudo docker image -aq`

To remove a image

`sudo docker image rm -f (image id )`

To removing dangling images

`sudo docker image prune`

To removing all unused images
`sudo docker image prune -a`

---

# Docker Commands For Container

To check docker version

`docker -v `

To check images

`sudo docker container ls `

To run a docker container in interactive mode (if docker stop container will also stop).

`sudo docker run -it (image name)`

To run a docker container in detached mode (to run in background)

`sudo docker run -d (image name)`

To run a docker container in detached mode along wth changed ports (to run in background)

`sudo docker run -d -p 5000 (Local machine port) : 5000 (Image port) (image name)`

To get ids of docker containers details

`sudo docker ps -a`

To get ids of docker containers

`sudo docker ps -aq`

To remove a docker containers

`sudo docker container rm (container ids)`

To removing all stopped containers

`sudo docker container prune`

To get a list of all non-running (stopped) containers

`sudo docker container ls -a --filter status=exited --filter status=created`

To remove all container including running

`sudo docker rm -f $(sudo docker ps -aq)`

---

# Execute a Docker Container

- Use docker ps to get the name of the existing container.

- Use the command `sudo docker exec -it (container name) ` /bin/bash to get a bash shell in the container.

- Generically, use `sudo docker exec -it <container name> <command>` to execute whatever command you specify in the container.

- use sh for shellscript `sudo docker exec -it <container name> sh`

---

# Stop a Docker Container

`docker stop <container name>`

---

# Docker Compose for Multi-Container

Docker Compose is a tool that was developed to help define and share multi-container applications.
With Compose, we can create a YAML file to define the services and with a single command, can spin everything up or tear it all down.

for docker compose installation use Clcik [Here][docer-compose installtion]
[docer-compose installtion]: https://docs.docker.com/compose/install/compose-desktop/

# Installing Compose on Linux systems

In this section, you can find various methods for installing Compose on Linux.

### Installation methods

Installing Docker Desktop for Linux is the easiest and recommended installation route. Check the Desktop for Linux supported platforms page to verify the supported Linux distributions and architectures.

The following other methods are possible:

Using the automated convenience scripts (for testing and development environments). These scripts install Docker Engine and Docker CLI with the Compose plugin. For this route, go to the Docker Engine install page and follow the provided instructions. After installing Desktop for Linux, this is the recommended route.

Setting up Docker’s repository and using it to install Docker CLI Compose plugin. See the Install using the repository section on this page. This is the second best route.

Installing the Docker CLI Compose plugin manually. See the Install the plugin manually section on this page. Note that this option requires you to manage upgrades manually as well.

## Install using the repository

If you have already set up the Docker repository jump to step 2.

Set up the repository. Go to the “Set up the repository” section of the chosen Linux distribution. found on the Docker Engine installation pages to check the instructions.

Update the apt package index, and install the latest version of Docker Compose:

Or, if using a different distro, use the equivalent package manager instructions.

` sudo apt-get update`

` sudo apt-get install docker-compose-plugin`

Alternatively, to install a specific version of Compose CLI plugin:

a. List the versions available in your repo:

`apt-cache madison docker-compose-plugin`
b. From the list obtained use the version string you can in the second column to specify the version you wish to install.

c. Install the selected version:

` sudo apt-get install docker-compose-plugin=<VERSION_STRING>`

where `<VERSION_STRING>` is, for example,2.3.3~ubuntu-focal.

Verify that Docker Compose is installed correctly by checking the version.

` docker compose version`

## Install the plugin manually

To run Compose as a non-root user, see Manage Docker as a non-root user.

To download and install the Compose CLI plugin, run:

` DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}`

` mkdir -p $DOCKER_CONFIG/cli-plugins`

`curl -SL https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose`

This command downloads the latest release of Docker Compose (from the Compose releases repository) and installs Compose for the active user under $HOME directory.

#### To install Docker Compose for all users on your system, replace ~/.docker/cli-plugins with /usr/local/lib/docker/cli-plugins.

A different version of Compose, substitute v2.6.0 with the version of Compose you want to use.
Apply executable permissions to the binary:

` chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose`

or, if you chose to install Compose for all users:

`sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose`

Test the installation.

`docker compose version`

To download and install Compose standalone, run:

` curl -SL https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose`

Apply executable permissions to the standalone binary in the target path for the installation.
Test and execute compose commands using` docker-compose`.

If the command docker-compose fails after installation, check your path. You can also create a symbolic link to /usr/bin or any other directory in your path. For example:

` sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`

# Uninstalling the Docker Compose CLI plugin

To remove the Compose CLI plugin, run:

` sudo apt-get remove docker-compose-plugin`

Or, if using a different distro, use the equivalent package manager instruction to remove docker-compose-plugin.

Manually installed

If you used curl to install Compose CLI plugin, to uninstall it run:

` rm $DOCKER_CONFIG/cli-plugins/docker-compose`

or, if you have installed Compose for all users, run:

` rm /usr/local/lib/docker/cli-plugins/docker-compose`

You can also use:

` docker info --format '{{range .ClientInfo.Plugins}}{{if eq .Name "compose"}}{{.Path}}{{end}}{{end}}'`

to inspect the location of the Compose CLI plugin.

## Creating a yaml file

1. Create a `dockercompose.yaml` in the main folder with contains multiple repos
