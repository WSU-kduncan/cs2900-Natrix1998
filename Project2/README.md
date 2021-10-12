# Container Technologies
1. Docker Engine (Not Docker Desktop, and not older versions which were named docker, docker.io, and docker-engine). I will still refer to Docker Engine with all-encompassing "Docker" because they do as well. 
2. Podman. (Docker in disguise).
**NOTE** This guide assumes you are running Linux Mint 20.1 Ulyssa for Docker. Linux Mint is somewhat known for being as close to a Windows OS as possible while still being linux, and that definitely shows when it comes to getting container platforms up and running. I recommend you DON'T use Mint, and instead use Ubuntu or even a Mac. I like Mint/I'm used to it and I am reluctant to change, so, here we are.
- **I am using Ubuntu 20.10 for podman. Mint does not work easily with containers, and 20.10 is recommended heavily by podman to make everything easier.** 
# How to Install
## Docker
1. `$ sudo apt-get update`.
1a. (Optional) `$ sudo apt-get upgrade`.
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
7. Verify the installation by running `$ sudo docker run hello-world`<br>
7a. This will pull the image down and run it, testing both at once. 
## Podman
1. `$ sudo apt-get update`
2. `$ sudo apt-get install podman`
3. Test by using `$ podman run -dt -p 8080:80/tcp docker.io/library/httpd` to run an httpd server. (Connecting is optional because of potentially annoying port-forwarding and additional linux configurations. If it didn't throw errors during install and run it should be fine.)
4.  Much better than using docker with mint. 
# Pulling a Container Image
## Docker
`$ sudo docker pull IMAGE:VERSION` will pull the requested image. If no version is specified, the latest version will be pulled.<br>
View all images with `$ sudo docker image ls -a`
## Podman
Podman works similarly to docker, use `$ sudo podman pull IMAGE` to pull a requested image. It can also pull docker archive images with `$ podman pull docker-archive:/tmp/myimage`. <br>
View all images with `$ sudo podman image ls -a`. This is the same as docker and the output is in a very similar format as well. 
# Running a Container 
## Docker
`$ sudo docker run CONTAINERNAME` will run the specified container (and initialize it first, more on that later). If it cannot be found locally, docker will first try to pull it, then run it.<br>
Docker automatically initializes containers when you run them. You can specify the executable you want to handle the init process, by adding the `--init PATH` flag, but this is somewhat new. "The default init process used is the first docker-init executable found in the system path of the Docker daemon process" Source: https://docs.docker.com/engine/reference/run/ <br>
- Add the `-d` option to the above command and docker will run in "detached" mode.<br>
- This means that the container will run in the background and it will not receive input or display output. Typically how you might want to run a webserver or a large data analysis job.
- Alternatively, running the command with `-it` options will run the container and keep STDIN open along with providing a terminal. 
- `-i` can be thought of as input/interactive, and `-t` can be thought of as terminal.
  - Sidenote: docker says `-t` "allocates a pseudo-tty" and since tty is an abbreviation for TeleTYpewriter (somehow) it's definitely just an "open a terminal" command.
  - If you use `-t` without `-i` you can see what the container is doing but you can't input any commands via the terminal.
`$ sudo docker exec CONTAINERNAME COMMAND` will execute the given command in the selected **running** container. This could be useful for configuring settings, weekly maintenance jobs, etc. 
## Podman
Podman is also very similar to docker in when it comes to running containers. <br>Simply use `$ sudo podman run CONTAINERNAME` to run a container.
You can use `$ sudo podman container init CONTAINERNAME` to initialize a container. Initializing a container performs all tasks necessary for starting the container (mounting filesystems, creating an OCI spec, initializing the container network) but does not start the container. <br> Source: https://docs.podman.io/en/latest/markdown/podman-init.1.html <br>
All of the previously mentioned docker run options will work with podman (if you swap out `docker` for `podman`). 
# Logs and Status
## Docker
1. To check the status of all containers, simply run `$ sudo docker container ls -a`. The status column displays the uptime or time since exit for each container.
1a. Additionally, running `$ sudo docker container stats CONTAINER` will display the resource usage of the specified container. Things such as CPU usage, mem usage, number of processes, etc. 
2. `$ sudo docker logs CONTAINERNAME` displays logs for a specified container.<br>
To view the logs for a running container, open a separate terminal window and use the `-f` option with the logs command.<br>
`$ sudo docker logs -f CONTAINERNAME`<br>
If I typed "echo 'hello'" in an ubuntu container, the logs terminal would immediately show that command being entered and the output it produces. 
## Podman
1. To check the status of all containers, run `$ sudo podman ps -a`. Note that podman uses **ps** NOT **ls** (although I think ls still works for now 10/12/2021).
1a. The previously mentioned docker commands work in podman as well. `$ sudo podman container stats CONTAINERNAME` will show a specific containers resource usage. Without a name, the command will display the resource usage for all running containers. Further options like `-a` can be added to include all containers. 
2. Logs works exactly the same as described above, but with `podman` instead of docker. 
# Stopping a Container
## Docker
First, I suggest getting all container ID's with `$ sudo docker container ls -a`. You'll need to enter these CONTAINER IDs or NAMES when running commands.<br>
Pause: `$ sudo docker container pause CONTAINERNAME` pauses the processes running inside the container. Locks further input if it's something like Ubuntu.
Unpause: Same command as before but with `unpause` this time. Resumes the processes.
Start: Starts a stopped container. `$ sudo docker container start -i CONTAINERNAME` <br>
Don't forget `-i` to start it in an interactive terminal. 
Stop: `$ sudo docker stop CONTAINERNAME` stops the container "gracefully" by asking it nicely to shut down.<br>
Kill: `$ sudo docker kill CONTAINERNAME` stops the container "hard".<br>
The difference can be (kind of) thought of as clicking "shut down" vs holding the power button on a computer. 
To remove all stopped containers: `$ sudo docker container prune -y`
## Podman
Podman is miraculously the same in every regard when it comes to stopping and restarting a container. It has some extra features to be aware of, **do not** try to start a container with `play`. That is a command for pods.<br>
Otherwise, the above commands listed for `docker` will work when running `podman`. 
