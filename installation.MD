
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

The above docker command will execute as a service the portainer into your Docker host. In order to test, please, open up your browser and point to: `http://HOST-IPADDRESS:9000`, if you are using your machine, it might be http://localhost:9000 .
