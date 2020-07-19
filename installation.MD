## nasph Cluster

Nasph combines in one solution a popular reverse proxy / load balancer and a blazing fast API Gateway. Both technologies are opensource and built with Go Lang, for those and many other reasons, we have a very interesting performance. Besides, we are using tools that developers love, such as Docker in order to setup the whole clusters and environment to expose Microservices and APIs in a very high performance way, also taking in consideration aspects of discovery, security, BFF (Backend for Frontend) etc. 

![nasph basic deployment schema](https://github.com/nasph-io/quickstart/raw/master/nasph_cluster_model.png)

Let's describe the image above in a very quick way, during the documentation, we will be able to cover in depth, each os those:

1 - Containers : You will have just to design your microservice as containers, we suggest you use Docker. If your Microservice has dependencies of other microservices, such as databases, other services and etc, you have to add this into your docker-compose.yml. 

2 - Each Container will be considered a "Service", as such, it will have a kind of "name"/"alias", that is act as key in order to let the Load Balancer layer to discover how and where to invoke this service.

3 - The Central API Gateway is the lonely part that knows internally how to invoke the services.

4 - Any consumer of API Gateway will use just this component as the entry-point to execute any microservice/API inside your architecture/cluster.


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

![docker compose](https://github.com/nasph-io/quickstart/raw/master/docker-compose.png)

We will run this entire stack in a total auto-scalable way using just regular docker (Swarm). We are planning to use Kubernetes in the future, but there are a lot of other solutions in that path, we do belive that nasph can be as powerful as some of those solutions, but running a much easier way, using the tools that you love. 

## Creating networks
We might be able to have different networks and communications between our microservices(APIs/Backends), and our API Gateway and external world.

For this basic scenario, let's create a simple bridge network called nasph_network:

```
docker network create nasph-network
```

## Creating the API Gateway Cluster Environment

We will have the following microservices:
* springboot - https://hub.docker.com/r/nasph/springboot-microservice
* dotnetcore - https://hub.docker.com/r/nasph/dotnetcore-microservice
* micronaut - https://hub.docker.com/r/nasph/micronaut-microservice

Each of those backends above, expose functions through the HTTP methods, in other words, they are APIs. 

## Using Docker-Compose

One of our goals is to make nasph extremelly easy to be executed, so, the composition will use just Docker and Docker-Compose. For 90%(or more) of Microservices's and APIs use cases, we are sure that this environment works pretty well. Docker supports a way to isolate different networks, and expose just what we want to. In addition, our API Gateway will be the responsible for expose what shall be consumed by the clients.

Let's to create the docker compose file, follow the instructions bellow:

* Go to the terminal / console 
* Create a new folder called nasph-quickstart 
* inside that created folder, create a sub-folder called: krakend , and inside this subfolder create a file called: krakend.json witht the following content: https://raw.githubusercontent.com/nasph-io/quickstart/master/krakend.json
* Inside the folder nasph-quickstart, create a file called docker-compose.yml , use the text editor of your preference. 
* Go to this address: https://raw.githubusercontent.com/nasph-io/quickstart/master/docker-compose.yml , save it. 
* Your folder will be like this:

```
.
├── docker-compose.yml
└── krakend
    └── krakend.json

1 directory, 2 files

```

* In the terminal window, please execute the following command:

```
docker-compose up 
```
On that moment, your machine will connect to the Docker Hub and you will download and execute our official Docker Images that will boot the nasph for you. According to your internet speed, it can take a few minutes, don't worry, the Docker has a very responsive command line, and you will be able to see how the things are progressing. 

## Digging Deeper 

As said before, nasph relies 100% in docker and into opensource projects, so let's take a look in each of the files you had created:

* docker-compose.yml - It defines the whole stack, as you can see we have the load balancer layer defined, the api-gateway and the microservices. We could create this file dinamically as a result of a CI/CD pipeline, in that regards, we can make our devops practices much simpler and focused, not requiring too much complexity for companies and teams that are jumping in this new world.
* krakend.json - This file defines the APIs's resources and how they will be invoking the existing APIs. In order to edit the API's routes, you can use text editor, or you can use our API editor that is available on the web. 

## Cluster is Up and Running

When you see the initial console's logs like this: 

```
Starting nasph-quickstart_reverse-proxy_1 ... done
Recreating nasph-quickstart_api-gateway_1 ... done
Recreating nasph-quickstart_dotnet-micro_1 ... done
Recreating nasph-quickstart_whoami_1       ... done
Attaching to nasph-quickstart_reverse-proxy_1, nasph-quickstart_api-gateway_1, nasph-quickstart_dotnet-micro_1, nasph-quickstart_whoami_1
api-gateway_1    | 2020/07/19 04:34:48  ERROR: unable to create the gologging logger: getting the extra config for the krakend-gologging module
api-gateway_1    | 2020/07/19 04:34:48  ERROR: unable to create the GELF writer: getting the extra config for the krakend-gelf module
api-gateway_1    | 2020/07/19 04:34:48  INFO: Listening on port: 8080
api-gateway_1    | 2020/07/19 04:34:48  DEBUG: creating a new influxdb client
api-gateway_1    | 2020/07/19 04:34:48  DEBUG: no config for the influxdb client. Aborting
api-gateway_1    | 2020/07/19 04:34:48  WARNING: influxdb: unable to load custom config
api-gateway_1    | 2020/07/19 04:34:48  WARNING: opencensus: no extra config defined for the opencensus module
api-gateway_1    | 2020/07/19 04:34:48  WARNING: building the etcd client: unable to create the etcd client: no config
reverse-proxy_1  | time="2020-07-19T04:34:17Z" level=info msg="Configuration loaded from flags."
api-gateway_1    | Parsing configuration file: /etc/krakend/krakend.json
whoami_1         | Starting up on port 80
api-gateway_1    | 2020/07/19 04:34:48  INFO: registering usage stats for cluster ID '1IK2SSWMoR7RjJwHGlK0NUG+wbfFna/19LVlMc9eQcw='
reverse-proxy_1  | 192.168.16.1 - - [19/Jul/2020:04:34:19 +0000] "GET /api/overview HTTP/1.1" 200 435 "-" "-" 1 "api@internal" "-" 0ms
api-gateway_1    | 2020/07/19 04:34:48  DEBUG: no config for the bloomfilter
reverse-proxy_1  | 192.168.16.1 - - [19/Jul/2020:04:34:22 +0000] "GET /api/overview HTTP/1.1" 200 435 "-" "-" 2 "api@internal" "-" 0ms
```

The cluster is ready to receive API's requests.

## Time for Testing 

OK, now we will invoke the first microservice: whoami : 

```
curl --location --request GET 'http://localhost/api/whoami' \
--header 'Host: nasph-apigateway'
```
the result will be the following:

The result will be the following:

    Hostname: 5eab7f3320be
    IP: 127.0.0.1
    IP: 192.168.32.5
    RemoteAddr: 192.168.32.2:42736
    GET / HTTP/1.1
    Host: whoami.docker.localhost
    User-Agent: KrakenD Version 1.1.1
    Accept-Encoding: gzip
    X-Forwarded-For: 192.168.32.3
    X-Forwarded-Host: whoami.docker.localhost
    X-Forwarded-Port: 80
    X-Forwarded-Proto: http
    X-Forwarded-Server: 233b716325c8
    X-Real-Ip: 192.168.32.3

Let's understand the "Whys": 

1 - Into our docker-compose, we have a service defined like this: 

    whoami:
    image: containous/whoami
    depends_on:
        - "api-gateway" (depending on API Gateway starting first)
        -labels:
         -"traefik.http.routers.whoami.rule=Host(`whoami.docker.localhost`)" 

The last configuration is the kind of very important: This the **"service name / key "** that will act in order to invoke this service (API), in that case, this value shall be passed in a HTTP Header with the value *"whoami.docker.localhost"* 

2 - Our API Gateway configuration, inside the file krakend.json, is exposing this Docker/Microservice as the following: 

    {
    
    "endpoint": "/api/whoami",
    
    "extra_config": {},
    
    "output_encoding": "no-op",
    
    "querystring_params": ["*"],
    
    "concurrent_calls": 1,
    
    "backend": [
    
    {
    
    "url_pattern": "/",
    
    "encoding": "no-op",
    
    "sd": "static",
    
    "extra_config": {
    
    "github.com/devopsfaith/krakend-martian": {
    
    "header.Modifier": {
    
    "scope": ["request", "response"],
    
    "name": "Host",
    
    "value": "whoami.docker.localhost"
    
    }
    
    }
    
    },
    
    "method": "GET",
    
    "host": ["http://192.168.32.2"],
    
    "disable_host_sanitize": false
    
    }
    
    ]
    
    }

The JSON above defines the following :
* The `endpoint` URI / API:  "`/api/whoami`" .
* This endpoint will call a `backend`:  `HTTP GET http://192.168.32.2` with the `url_pattern` "/" passing a `HTTP Header Host` with the value `whoami.docker.localhost` . In that case, the API Gateway is the one that knows the Backends and Microservices, and not the API requester (client consumers). 

### Executing the calls on the API Gateway 

The `API Gateway` is defined pretty-much similar to the `whoami` service. Check the definition bellow extracted from docker-compose.yml:  

    api-gateway:
    
    image: nasph/krakend
    
    depends_on:
    
    - "reverse-proxy"
    
    networks:
    
    - nasph-network
    
    labels:
    
    - traefik.http.routers.api-gateway.rule=Host(`nasph-apigateway`)
    
    - traefik.http.services.api-gateway-service.loadbalancer.server.port=8080
    
    volumes:
    
    - ./krakend:/etc/krakend/

The most important configuration for this moment is the traefik.http.routers.api-gateway.rule=Host(`nasph-apigateway`) , that is the value to header Host when you want to execute the API Gateway. 


## Executing other Services

Inside the docker-compose.yml, you will find also a dotnet-core microservice, that you can invoke accordingly :

Post a new Task :

    curl --location --request POST 'http://localhost/api/dotnetcore' \
    
    --header 'Host: nasph-apigateway' \
    
    --header 'Content-Type: application/json' \
    
    --data-raw '{
    
    "name":"review the doc",
    
    "isComplete":false
    
    }'

The following POST request, will return the created task. 

In order to see the all created tasks, you can call the same api, but using HTTP GET as the method:

    curl --location --request GET 'http://localhost/api/dotnetcore' \
    
    --header 'Host: nasph-apigateway'

Result: 

    [{"id":1,"name":"review the doc","isComplete":false}]%

# Troubleshooting
If you get a 502 HTTP Error - Bad Gateway, please, make sure that the backends in the krakend.json are pointing to the right IP address inside docker. 

The easiest way to see that, is to check the Traefik's log:

    reverse-proxy_1  | 192.168.32.1 - - [19/Jul/2020:05:56:50 +0000] "GET /api/overview HTTP/1.1" 200 435 "-" "-" 464 "api@internal" "-" 0ms
    reverse-proxy_1  | 192.168.32.1 - - [19/Jul/2020:05:56:55 +0000] "GET /api/overview HTTP/1.1" 200 435 "-" "-" 465 "api@internal" "-" 0ms

In the scenario above, the network attached to this cluster and its containers, is working on the range: 192.168.32.X, in that case, the load-balancer front-end that will hold the whole containers services shall be: `http://192.168.32.2.`

The following example in krakend.json file of a the dotnetcore service definition:

    {
    
    "endpoint": "/api/dotnetcore",
    
    "extra_config": {},
    
    "output_encoding": "no-op",
    
    "querystring_params": ["*"],
    
    "concurrent_calls": 1,
    
    "backend": [
    
    {
    
    "url_pattern": "/api/TodoItems",
    
    "encoding": "no-op",
    
    "sd": "static",
    
    "extra_config": {
    
    "github.com/devopsfaith/krakend-martian": {
    
    "header.Modifier": {
    
    "scope": ["request", "response"],
    
    "name": "Host",
    
    "value": "dotnet.localhost"
    
    }
    
    }
    
    },
    
    "method": "GET",
    
    "host": ["http://192.168.32.2"],
    
    "disable_host_sanitize": false
    
    }
    
    ]
    
    }

# Conclusion

Please, keep posted about our news, and how we will be evolving our documentations, samples, quickstarts, webinars etc.



