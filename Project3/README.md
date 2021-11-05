    # Investigate Available Mounts
    ## Docker
    1. Volumes: According to the docker documentation, "volumes are the preferred mechanism [over bind mounts] for persisting data generated and used by Docker containers". Volumes are more flexible overall than bind mounts. Volumes are completely managed by docker.
        * A hilarious piece of documentation: "-v or --volume: Consists of three fields, separated by colon characters. The fields must be in the correct order, and the meaning of each field is not immediately obvious."
            * The first field is the name of the volume and is unique on a given host machine. 
            * The second field is the path to where the file or directory is mounted in the container. 
            * The third field is optional and is a comma-separated list of options.
            * All together, this looks like `$ docker run -dit -v NAMEDVOLUME:/CONTAINER/FOLDER/PATH:OPTIONS IMAGE:TAG` I recommend creating a volume when running the container, as it seems to be the simplest way to do it (in my mind). 

    A full rundown of volumes by Docker can be found here https://docs.docker.com/storage/volumes/.  
    Using `--mount` allows you to be more specific than using `-v` does. `-v` can be thought of as the quick way for when you're really familiar with it, and `--mount` is for spelling everything out.
    2. Bind Mounts: These are dependent on the directory structure and the OS of the host machine. Also I would like to mention again that docker really, REALLY doesn't seem to want you using these. Bind mounts "have been around since the early days of Docker" and have "limited functionality compared to volumes". 
        * Any process can modify a bind mount since it can exist anywhere on the host filesystem. 
        * As for how to use a bind mount, you can use it almost the exact same way you would use a volume. `$ docker run -dit -v "$(pwd)"/target:/app IMAGE:TAG`
        * Instead of `"$(pwd)"` you can specify the path to the file or directory. The previous command will bind the "/target" directory into the container at "/app". 

    Conclusion: Use volumes, don't ever touch bind mounts. I read somewhere that bind mounts have better performance but it honestly does not seem worth it, regardless of the performance boost. Bind mounts also have more risks for "easy" vulnerabilities. 
    ## Podman
    1. Volumes: Podman also has options for both volumes and bind mounts, but there are some slight differences in naming conventions. Everything else is the exact same as docker from what I can tell. 
    2. Bind Mounts: Podman Bind Mounts work the exact same as Docker Bind mounts (I think). Although the syntax is extremely weird for their `-v` option. "-v[=[[SOURCE-VOLUME|HOST-DIR:]CONTAINER-DIR[:OPTIONS]]]".
        * As they so helpfully document, "Create a bind mount. If you specify /HOST-DIR:/CONTAINER-DIR, Podman bind mounts host-dir in the host to CONTAINER-DIR in the Podman container. Similarly, SOURCE-VOLUME:/CONTAINER-DIR will mount the volume in the host to the container. If no such named volume exists, Podman will create one. (Note when using the remote client, the volumes will be mounted from the remote server, not necessarily the client machine.)"
    # Investigate Building Images for the Container Engine
    ## Docker
    1. Docker has image building tools provided. Docker uses a configuration file called "Dockerfile" to build images.
        * To setup a Dockerfile, you need a minimum of ____________________
        * The command for building images via the CLI is `$ docker build [OPTIONS] PATH | URL`, which builds an image based on the Dockerfile and the "context". 
            * Context: Refers to a set of files at the specified PATH or URL. 
    An example Dockerfile is as follows.
    >FROM   ubuntu
    >
    >VOLUME ["/var/cache/apt-cacher-ng"]
    >RUN    apt-get update && apt-get install -y apt-cacher-ng
    >
    >EXPOSE 3142
    >CMD    chmod 777 /var/cache/apt-cacher-ng && /etc/init.d/apt-cacher-ng start && tail -f /var/log/apt-cacher-ng/*
    This is a Dockerfile used to build a apt-cacher-ng service.
    The slides and the documentation describe the following terms. 
    * FROM specifies the parent image from which you are building.
    * VOLUME creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers. 
    * RUN executes any commands on top of the current image and creates a new layer for comitting the results. 
    * CMD is the command the container executes by default when you launch the built image.
    * EXPOSE is an instruction that informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP. The default is TCP if the protocol is not specified.
    `$ docker build -f Dockerfile.debug .` will build a container running an image specified in the dockerfile "Dockerfile.debug" in the current directory. 
    ## Podman
    1. Podman has image building tools provided. Podman uses a configuration file called Containerfile or Dockerfile to build images.
    2. A Containerfile uses the same syntax as a dockerfile. Replace docker with podman where appropriate, and be wary of any other small but rare syntax differences. 
    `$ podman build -f Containerfile .` builds a container running an image specified in the container file, as designated by the -f option. It is very nice that this option is the same in both Docker and podman. 

    ## Sources
    https://stackoverflow.com/questions/37461868/difference-between-run-and-cmd-in-a-dockerfile 
    https://docs.docker.com/samples/apt-cacher-ng/
    https://docs.docker.com/engine/reference/builder/
    https://docs.podman.io/en/latest/markdown/podman-build.1.html
    https://docs.docker.com/engine/reference/commandline/build/
