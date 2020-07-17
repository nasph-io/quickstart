
## Required Software

nasph 0.8 runtime relies 100% on Docker initially, and you must have docker installed into your machine. We tested on Linux, Mac and Windows. 

* **Windows Users**: We found some issues depending on windows's versions, if you get issues installing Docker in your windows, please, use a linux virtual machine properly configured. We recommend you use Vagrant for setup your VM boxes in your windows. 

## Docker installed 

Let's test if Docker is instaled into your machine, please open the terminal an type the following comand:

    $ <sudo> docker ps 
sudo: might be necessary just if you have no root permissions.

The result shall be something like:

    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

If your Docker environement was recently intalled, you might have no containers instances running into your machine (the host). 

## Running Portainer (Container Management tool)

In this version, we are using yet Portainer to manage our containers services and clusters, please, execute the following commands in order to pull nasph's portainer image and execute it :

    $ docker volume create portainer_data
It will allocate the volume (storage) for portainer. 

The next step, we will execute our Containers management tool:

    docker run -d -p 9000:9000 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data nasph/swarmconsole

The above docker command will execute as a service the portainer into your Docker host. In order to test, please, open up your browser and point to: `http://HOST-IPADDRESS:9000`, if you are using your machine, it might be http://localhost:9000 . Other interesting point, the lastest docker command will let our portainer (docker containers management tool) running, even after your reboot your machine, like a service. The result, should be something similar to the the following console output:

    Unable to find image 'nasph/swarmconsole:latest' locally
    latest: Pulling from nasph/swarmconsole
    d1e017099d17: Pull complete
    b8084bf83dcf: Pull complete
    72b2bbaae47d: Pull complete
    Digest: sha256:4002388a7230c50f6ddde14bc492be861babba39420a771c1f0b8772fff107d3
    Status: Downloaded newer image for nasph/swarmconsole:latest
    ee3391493eb17f8eae71a4c55ff9636b0dd8ad31919b9d3f69a5ce911e5b334a

Deeper details about Docker are beyond the scope of that Quickstart, but we can notice that in the case, Docker had not this image locally, had pulled from Docker Hub and executed into our Host.

![nasph swarm console / portainer ](https://github.com/nasph-io/quickstart/raw/master/nasph_swarm_home.jpg)

# nasph API Orchestrator

Nasph combines a super reliable load-balancer/reverse proxy and a blazing fast API Gateway, running in a very smooth and lightweight architecture. The technologies that nasph is using are 100% opensource and built using Go Language, which promotes an incredible performance, at the same time, extremely easy to extend.

We will run this entire stack in a total auto-scalable way using just regular docker (Swarm). We are planning to use Kubernetes in the future, but there are a lot of other solutions in that path, we do belive that nasph can be as powerful as some of those solutions, but running a much easier way, using the tools that you love. 

