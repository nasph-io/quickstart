
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
Creating nasph_external-LB_1 ... done
Creating nasph_api-gateway_1 ... done
Recreating nasph_micronaut-sp_1 ... done
Recreating nasph_dotnet-micro_1 ... done
Creating nasph_springboot-ms_1  ... done
Attaching to nasph_external-LB_1, nasph_api-gateway_1, nasph_springboot-ms_1, nasph_micronaut-sp_1, nasph_dotnet-micro_1
api-gateway_1    | Parsing configuration file: /etc/krakend/krakend.json
api-gateway_1    | 2020/07/17 22:05:43  ERROR: unable to create the gologging logger: getting the extra config for the krakend-gologging module
api-gateway_1    | 2020/07/17 22:05:43  ERROR: unable to create the GELF writer: getting the extra config for the krakend-gelf module
api-gateway_1    | 2020/07/17 22:05:43  INFO: Listening on port: 8080
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: creating a new influxdb client
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: no config for the influxdb client. Aborting
api-gateway_1    | 2020/07/17 22:05:43  WARNING: influxdb: unable to load custom config
api-gateway_1    | 2020/07/17 22:05:43  WARNING: opencensus: no extra config defined for the opencensus module
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: pubsub: subscriber (http://172.20.0.2): github.com/devopsfaith/krakend-pubsub/subscriber not found in the extra config
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: pubsub: publisher (http://172.20.0.2): github.com/devopsfaith/krakend-pubsub/publisher not found in the extra config
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: http-request-executor: no extra config for backend /api/TodoItems
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: CEL: no extra config detected for backend /api/TodoItems
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: lua: no extra config
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: CEL: no extra config detected for pipe /api/dotnetcore
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: lua: no extra config
api-gateway_1    | 2020/07/17 22:05:43  INFO: JOSE: singer disabled for the endpoint /api/dotnetcore
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: lua: no extra config
api-gateway_1    | 2020/07/17 22:05:43  INFO: JOSE: validator disabled for the endpoint /api/dotnetcore
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: botdetector:  no config defined for the module
api-gateway_1    | 2020/07/17 22:05:43  DEBUG: http-server-handler: no extra config
api-gateway_1    | 2020/07/17 22:05:43  INFO: registering usage stats for cluster ID 'Zu07Tx5wWffmg+PBDhCcJbWTfpkFUa59076Z+5o2xn0='
external-LB_1    | time="2020-07-17T22:05:42Z" level=info msg="Configuration loaded from flags."
dotnet-micro_1   | info: Microsoft.Hosting.Lifetime[0]
dotnet-micro_1   |       Now listening on: http://0.0.0.0:5000
dotnet-micro_1   | info: Microsoft.Hosting.Lifetime[0]
dotnet-micro_1   |       Application started. Press Ctrl+C to shut down.
dotnet-micro_1   | info: Microsoft.Hosting.Lifetime[0]
dotnet-micro_1   |       Hosting environment: Production
dotnet-micro_1   | info: Microsoft.Hosting.Lifetime[0]
dotnet-micro_1   |       Content root path: /app
springboot-ms_1  | 
springboot-ms_1  |   .   ____          _            __ _ _
springboot-ms_1  |  /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
springboot-ms_1  | ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
springboot-ms_1  |  \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
springboot-ms_1  |   '  |____| .__|_| |_|_| |_\__, | / / / /
springboot-ms_1  |  =========|_|==============|___/=/_/_/_/
springboot-ms_1  |  :: Spring Boot ::        (v2.3.0.RELEASE)
springboot-ms_1  | 
springboot-ms_1  | 2020-07-17 22:05:48.990  INFO 1 --- [           main] c.n.springboot.SpperformerApplication    : Starting SpperformerApplication v0.0.1-SNAPSHOT on c0991e5473e0 with PID 1 (/app.jar started by spring in /)
springboot-ms_1  | 2020-07-17 22:05:49.002  INFO 1 --- [           main] c.n.springboot.SpperformerApplication    : No active profile set, falling back to default profiles: default
springboot-ms_1  | 2020-07-17 22:05:52.445  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 7780 (http)
springboot-ms_1  | 2020-07-17 22:05:52.514  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
springboot-ms_1  | 2020-07-17 22:05:52.515  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.35]
springboot-ms_1  | 2020-07-17 22:05:52.836  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
springboot-ms_1  | 2020-07-17 22:05:52.837  INFO 1 --- [           main] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 3650 ms
springboot-ms_1  | 2020-07-17 22:05:53.737  INFO 1 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
springboot-ms_1  | 2020-07-17 22:05:54.407  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 7780 (http) with context path ''
springboot-ms_1  | 2020-07-17 22:05:54.433  INFO 1 --- [           main] c.n.springboot.SpperformerApplication    : Started SpperformerApplication in 7.196 seconds (JVM running for 10.152)
```

The cluster is ready to receive API's requests.







