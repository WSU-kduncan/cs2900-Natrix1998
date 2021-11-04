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

Conclusion: Use volumes, don't ever touch bind mounts. I read somewhere that bind mounts have better performance but it honestly does not seem worth it, regardless of the performance boost. 
## Podman
1. Volumes: Podman also has options for both volumes and bind mounts, but there are some slight differences in naming conventions. Everything else is the exact same as docker from what I can tell. 
2. Bind Mounts: Podman Bind Mounts work the exact same as Docker Bind mounts (I think). The syntax is extremely weird for their `-v` option. "-v[=[[SOURCE-VOLUME|HOST-DIR:]CONTAINER-DIR[:OPTIONS]]]".
    * As they so helpfully document, "Create a bind mount. If you specify /HOST-DIR:/CONTAINER-DIR, Podman bind mounts host-dir in the host to CONTAINER-DIR in the Podman container. Similarly, SOURCE-VOLUME:/CONTAINER-DIR will mount the volume in the host to the container. If no such named volume exists, Podman will create one. (Note when using the remote client, the volumes will be mounted from the remote server, not necessarily the client machine.)"
# Investigate Building Images for the Container Engine
## Docker
1. Docker has image building tools provided. Docker uses a configuration file called "Dockerfile" to build images.
    * To setup a Dockerfile, you need a minimum of ____________________
    * The command for building images via the CLI is `$ docker build [OPTIONS] PATH | URL`, which builds an image based on the Dockerfile and the "context". 
        * Context: Refers to a set of files at the specified PATH or URL. 
## Podman
1. Podman has image building tools provided. Podman uses a configuration file called Containerfile or Dockerfile to build images. 
