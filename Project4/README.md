# Project 4
## Container Netorking
1. Docker
- Docker supports **host mode, bridged mode,** and **no mode (offline or different/custom driver)** for networking. They also have drivers for overlay, ipvlan, and macvlan networks, but these seem to be for advanced/niche situations so I will not be covering them. 
- To my extreme surprise, the documentation is actually quite helpful and clear in regards to Docker networking. The full page can be found here https://docs.docker.com/network/.
- Docker uses **bridged networking** by default.
- Example run command: `$Docker run -dit --rm -p 4141:8080 thethingtorun:9.1` will run the app version 9.1 and bind the host port 4141 to the container port 8080. 
This link provides a description of when you might want to use each mode. https://docs.docker.com/network/#network-driver-summary.
2. Podman
- Podman takes a different approach (surprisingly) to networking modes/drivers. 
    - If a container is rootfull and being run as a root or equivalent user, then podman relies on the **containernetworkingplugins** project, which appears to be an opensource project for container network drivers. They support the same modes Docker does (host, bridged, ipvlan, macvlan)
- If a container is rootless and defined as being run by a regular user, podman uses the **slirp4netns** project. 
- Podman's documentation about networking is also very useful. https://podman.io/getting-started/network. From that page, "When using Podman as a rootless user, the network is setup automatically. The container itself does not have an IP Address, **because without root privileges, network association is not allowed**. You will also see some other limitations."
    - Docker's main page for networking does not mention rootless vs root containers or any other security concerns. Probably because, in my opinion, "securing" a docker container fully is a process I would call tangled. 
- Podman also uses **bridged networking** by default. 
## Vulnerability Scanners/Linters
1. One Dockerfile linter is https://hadolint.github.io/hadolint/ and it searches for best practices that are defined by docker. These best practices are a list of guidelines and recommendations for how to format certain common instructions/situations within the Dockerfile. With the hadolint linter, you just need to paste a dockerfile and click “lint”. It will then insert suggestions above “problematic” code. 
- Example: When selecting an image with FROM, hadolint will suggest that you always tag the image explicitly. It is not always a good idea to just pull the latest image for your container. 
- The command line version of hadolint provides a full list of “Rules” on its dockerhub page found here https://hub.docker.com/r/hadolint/hadolint. 
2. One vulnerability scanner is Trivy. Here is their official website https://aquasecurity.github.io/trivy/v0.21.0/. <br>Trivy defines itself as a vulnerability/misconfiguration scanner for containers and other artifacts. They define a vulnerability as a glitch, flaw, or weakness present in the software or Operating System [being scanned]. It scans OS packages, language specific packages, and Infrastructure as Code files such as Terraform, CloudFormation and Dockerfile. The output of Trivy is very clean in my opinion, and it will list things like Common Vulnerability and Exposures (CVE) IDs, as well as the severity and library name associated with each vulnerability. A Trivy scan also outputs which version has fixed the issue (if one has), and a short description of the issue (if it can).
- Trivy appears to be in active development, so while there are some limits on what you can scan currently, these limits may change in the future.
- Trivy can currently only scan **public** git repositories.
- Trivy can scan container images or tar files. It is a little unclear (to me) as to whether or not container image scanning functionality varies depending on which program you used to create the container image. I think the scanner's functionality support is mostly universal for container images (as of 12/4/2021) since the industry is leaning heavily towards Docker and Docker-like practices. 
### Appendix
https://github.com/rootless-containers/slirp4netns
https://github.com/containernetworking/plugins

