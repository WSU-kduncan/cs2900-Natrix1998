# Container Technologies
1. Docker Engine (Not Docker Desktop, and not older versions which were named docker, docker.io, and docker-engine)
2. Podman

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
  focal stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`<br>
  **IMPORTANT** If using linux mint, make sure you select focal instead of `$(lsb_release -cs)` as Docker's site suggests. 
6. Run `$ sudo apt-get update` and then `$ sudo apt-get install docker-ce docker-ce-cli containerd.io` to install the latest versions of Docker Engine and containerd.
7. Verify the installation by running `$ sudo docker run hello-world`

# Pulling a Container Image
## Docker
`$ sudo docker pull IMAGE:VERSION` will pull the requested image. If no version is specified, the latest version will be pulled.
View all images with `$ sudo docker image ls`
# Running a Container 
## Docker
`$ sudo docker run CONTAINER` will run the specified container. If it cannot be found locally, docker will first try to pull it, then run it. 
# Logs and Status
## Docker
# Stopping a Container
## Docker
First, I suggest getting all container ID's with `$ sudo docker container ls -a`. You'll 'need' to enter these IDs when running commands.<br>
Stop: `$ sudo docker stop CONTAINER_ID` stops the container "gracefully" by asking it nicely to shut down.<br>
Kill: `$ sudo docker kill CONTAINER_ID` stops the container "hard".<br>
The difference can be thought of as clicking "shut down" vs holding the power button on a computer. 
To remove all stopped containers: `$ sudo docker container prune -y`
