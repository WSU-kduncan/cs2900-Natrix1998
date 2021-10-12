# Container Technologies
1. Docker Engine (Not Docker Desktop, and not older versions which were named docker, docker.io, and docker-engine). I will still refer to Docker Engine with all-encompassing "Docker" because they do as well. 
2. LXC Linux Containers
<br>
**NOTE** This guide assumes you are running Linux Mint 20.1 Ulyssa. Linux Mint is somewhat known for being as close to a Windows OS as possible while still being linux, and that definitely shows when it comes to getting container platforms up and running. I recommend you DON'T use Mint, and instead use Ubuntu or even a Mac. I like Mint/I'm used to it and I am reluctant to change, so, here we are. 

# How to Install
## Docker
1. `$ sudo apt-get update`<br>
1a. (Optional) `$ sudo apt-get upgrade`<br>
2. Verify there is no old version of docker on your system. `$ sudo apt-get remove docker docker-engine docker.io containerd runc`<br>
**Note** If the previous packages report as being not installed, that is okay. Some versions of Linux (e.g. mint) reports that docker-engine cannot be located. This seems to be less okay but I was still able to ignore it and get docker running.
3. We are going to be installing docker from the repository. To add the repository, first run <br>
`$sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release`<br>
This will allow `apt` to use a repository over HTTPS.
4. Add Docker's official GPG key. `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
5. Add the repo for the stable version. `echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  focal stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`<br><br>
  **IMPORTANT** If using linux mint, make sure you select focal instead of `$(lsb_release -cs)` as Docker's site suggests.<br><br>
6. Run `$ sudo apt-get update` and then `$ sudo apt-get install docker-ce docker-ce-cli containerd.io` to install the latest versions of Docker Engine and containerd.
7. Verify the installation by running `$ sudo docker run hello-world`
7a. This will pull the image down and run it, testing both at once. 
## LXC
1. `$ sudo apt-get update`
2. `$ sudo apt-get install lxc`
# Pulling a Container Image
## Docker
`$ sudo docker pull IMAGE:VERSION` will pull the requested image. If no version is specified, the latest version will be pulled.
View all images with `$ sudo docker image ls`
## LXC

# Running a Container 
## Docker
`$ sudo docker run CONTAINERNAME` will run the specified container. If it cannot be found locally, docker will first try to pull it, then run it.<br>
Add the `-d` option to the above command and docker will run in "detached" mode.<br>
This means that the container will run in the background and it will not receive input or display output. <br>
Alternatively, running the command `$ sudo docker run -it CONTAINERNAME` will run the container and keep STDIN open along with providing a terminal. `-i` can be thought of as input/interactive, and `-t` can be thought of as terminal.<br>
Sidenote: docker says `-t` "allocates a pseudo-tty" and since tty is an abbreviation for TeleTYpewriter (somehow) it's definitely just an "open a terminal" command. If you use `-t` without `-i` you can see what the container is doing but you can't input any commands via the terminal.<br>
`$ sudo docker exec CONTAINERNAME COMMAND` will execute the given command in the selected **running** container.

## LXC

# Logs and Status

## Docker
1. To check the status of all containers, simply run `$ sudo docker container ls -a`. The status column displays the uptime or time since exit for each container.
1a. Additionally, running `$ sudo docker container stats CONTAINER` will display the resource usage of the specified container. Things such as CPU usage, mem usage, number of processes, etc. 
2. `$ sudo docker logs CONTAINERNAME` displays logs for a specified container.<br>
To view the logs for a running container, open a separate terminal window and use the `-f` option with the logs command.<br>
`$ sudo docker logs -f CONTAINERNAME`<br>
If I typed "echo 'hello'" in an ubuntu container, the logs terminal would immediately show that command being entered and the output it produces. 

## LXC

# Stopping a Container

## Docker
First, I suggest getting all container ID's with `$ sudo docker container ls -a`. You'll need to enter these CONTAINER IDs or NAMES when running commands.<br>
Pause: `$ sudo docker container pause CONTAINERNAME` pauses the processes running inside the container. Locks further input if it's something like Ubuntu.
Unpause: Same command as before but with `unpause` this time. Resumes the processes.
Start: Starts a stopped container. `$ sudo docker container start -i CONTAINERNAME` <br>
Don't forget `-i` to start it in an interactive terminal. 
Stop: `$ sudo docker stop CONTAINERNAME` stops the container "gracefully" by asking it nicely to shut down.<br>
Kill: `$ sudo docker kill CONTAINER_ID` stops the container "hard".<br>
The difference can be thought of as clicking "shut down" vs holding the power button on a computer. 
To remove all stopped containers: `$ sudo docker container prune -y`

## LXC
