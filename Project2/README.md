# Container Technologies
1. Docker Engine (Not Docker Desktop, and not older versions which were named docker, docker.io, and docker-engine)
2. Podman

# How to Install
## Docker
Run the following commands in the order they are listed. 
'$ sudo apt-get update' 
(Optional) '$ sudo apt-get upgrade'
Verify there is no old version of docker on your system.
'$ sudo apt-get remove docker docker-engine docker.io containerd runc'
If the previous packages report as being not installed, that is okay.
Some versions of Linux (e.g. mint) reports that docker-engine cannot be located. 

We are going to be installing docker from the repository. 
To add the repository, first run '$sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release'
This will allow 'apt' to use a repository over HTTPS. 
'$ sudo systemctl enable docker && sudo systemctl start docker'
Check the status to make sure the installation worked and everything was enabled correctly. 
'$ sudo systemctl status docker'
Docker is now installed and "running" - meaning you can use 'docker' commands - but it will turn itself off after a reboot. 

# Pulling a Container Image

# Running a Container 

# Logs and Status

# Stopping a Container
