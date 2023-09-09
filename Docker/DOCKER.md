# DOCKER IN PILLS
Develop by googgle in 2013

The OS has 2 different layes 
1. OS kernel ( comunicate with hardware )
2. OS application ( your appllication )
```
 ┌────────────────────┐
 │OS APPLICATION LAYER│
 └─────────┬──────────┘
           │
           ▼
 ┌────────────────────┐
 │     OS KERNEL      │
 └─────────┬──────────┘
           │
           ▼
 ┌────────────────────┐
 │      HARDWARE      │
 └────────────────────┘
```

## Differance between application virtualization and OS virtualizzation
1. APPLICATION VIRTUALIZATION
    This virtualization talk directly with the OS Kernel only the application is isoleted

2. OS VIRTUALIZATION 
    This virtualization contains the OS and the application

COMPARATION: 
- SIZE: in the first case we have an image smaller than the second case becouse it do not required the OS space and license so in the first case we talk in order of  MegaBytes in the second ths order is  GigaBytes
- STARTUP SPEED:, starting up a container is faster than starting up a VM  (millisecond vs minute)
- COMPATIBILITY: a container can run in windows and linux system

PRONS
- rapid and isolated installaztion
- replicas of the same enviroment on each machine
- centralized command and manage point
- standardize and increasing semplicity of setup process
- diffenert version of same software in the same enviroment

CONS
- Shared OS Kernel

# GO IN DEEP OF RELEASE SOFTWARE

COMMONLLY PROCESS

- Developer relase create a new package of software
- Operation teams pick the new package and manually installed this software 
- write documentation of which process follow for insatlling etc.

Common Issue:
Installed manually comsume time and maybe ensure to problem with compatibility that may not be done with the current version
so developer must solve and gurantee that then new installation is backward comaptible withe new version .
Another point to considere is the human errors.

This mean:
- more time for each customization and installaztion
- solid memory on each customer state or situation
- be in late with other relase

NEW PROCESS

Create new application package e segregate package that means no configuration,
the enviroment and version is the same off the developer enviroment that is tested.
from the dev toi the server there is no excuses, all the process is automatically , this semplify the time and error that be ensure in this process
The processe can be automatically without any intercation os support

># In short
>NO MORE RUN ON MY MACHINE


## Docker image and container
### Image
The image is a set of file such as the artifacts and eviroment configuration such as web server the frameworks require,files etc.
```
 ┌─────────────────────────┐
 │         IMAGE           │
 ├─────────────────────────┤
 │ - APPLICATION           │
 │                         │
 │ - FRAMEWORK             │
 │                         │
 │ - ENVIROMENT VARIABLES  │
 │                         │
 │ - CONFIGURATION FILE    │
 └─────────────────────────┘

OR

┌─────────────────────────┐        ┌─────────────────────────┐
│         IMAGE           │        │         IMAGE           │
├─────────────────────────┤        ├─────────────────────────┤
│ - APPLICATION           │        │                         │
│                         │        │                         │
│ - FRAMEWORK OR RUNTIME  │        │ - DATABASE              │
│                         │        │                         │
│ - ENVIROMENT VARIABLES  │        │                         │
│                         │        │                         │
│ - CONFIGURATION FILE    │        │                         │
└─────────────────────────┘        └─────────────────────────┘

```
The images is pushed on a docker hub that can be (private or public) and the downloaded by the script called in DockerFile
The imgae is versioned, the last tag is called latest.
When you pull an image and do not specify the version the latest will be downloaded.

### Container
- Container is the instance of an image runing in a server, from 1 image you can run multiple container

```
            ┌─────────────────────────┐
            │         SERVER          │
┌───────────┴─────────────────────────┴───────────────┐
│                                                     │
│  ┌──────────────┐┌──────────────┐┌─────────────┐    │
│  │ WEB APP 1    ││ WEB APP 2    ││  DATABASE   │    │
│  │ IMAGE   1    ││ IMAGE   2    ││ IMAGE   3   │    │
│  └──────────────┘└──────────────┘└─────────────┘    │
└─────────────────────────────────────────────────────┘
```



## Image and contiainer command

- Pull an image from docker hub, the syntax is  dokcer pull <Image:version>
```sh
dokcer pull {Image}:{version}
```
- List of images downloaded 
```sh
dokcer image ls
```
- Running a container 
    After download an image with pull command you can now running , alternativly you can call the run command directly, docker chek in images list locally if it do not found it the image will be pulled autoimatically 
```sh
dokcer run {Image}:{version}
```
- Running in detacched mode with soecific name
```sh
dokcer run -name MyContainer -d {Image}:{version} 
```
- List of running container, the flag -a menas all , if is not specified docker show only running container
```sh
dokcer ps -a
```
- Show log inside container
```sh
dokcer logs {containerID or container name}
```
- Stop a container
```sh
dokcer stop {containerID or container name}
```
- Start a container
```sh
dokcer startp {containerID or container name}
```

### Port binding
Each container has her own ports, for bindig bort to the host you must specify the mapping.
With falg -p you can specify the {host port} mapped to container port
```sh
dokcer run -d -p 90:80 myAPP:1.0
```

```
┌───────────────────────────────────────────────────────────────┐
│                   HOST ENVIROMENT / SERVER                    │
├───────────────────────────────────────────────────────────────┤
│                                                               │
│                                                               │
│        ┌──────────────┐                                       │
│        │ CONTAINER    │                                       │
│        ├──────────────┤                                       │
│        │              │                                       │
│        │ PORT 80      ├─────────────────────────────┐         │
│        │              │                             │         │
│        └──────────────┘                             │         │
│                                                     │         │
│                                            ┌────────▼─────────┤
│                                            │                  │
│                                            │  HOST PORT 90    │
│                                            │                  │
└────────────────────────────────────────────┴──────────────────┘
```

## Dockerfile for create image
A Dockerfile is a script that that contains the spcecification from building an image.
I want to create a container that run a nodejs api
so create this structure :
```
Source
└──src
    └───package.json
    |
    └───server.js
```
Create a foledr src and inside create the 2 files.
in the package.json copy this:
```sh
{
    "name":"myapp",
    "version":"1.0",
    "dependencies":{
        "express":"4.18.2"
    }
}
```
```sh
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});

```
For running this as continer we hae to replicate the same steps like our enviroment so we need:
1. Node installed on machine
2. The application source code
3. The package file for the denendencies
4. Running a command for execute the application


For example:
```sh
# 1. installing node
FROM node:19-alpine

# 2. copy from src local folder to app folder on  image
COPY . /app/
# change direcory to app
WORKDIR /app 

# 3. installing package -> npm install inside folder
RUN npm install
# 4. running application -> node server.js
CMD ["node","server.js"]
```

for build and image now we have to run this command , moving in Dockerfile folder (Source folder)
the flag -t  specify the name of image and the version the .(dot) rappresents the path of Dockerfile 
```sh
docker build -t myapp:1.0 .
```
After build we should have the image named myapp and we can run it
```sh
docker image ls
docker run --name app-test-container -d -p 3000:3000 myapp:1.0 
```
now check running continer
```sh
docker ps 
```
After that navigate on http://localhost:3000/ an the Hello World! sould appear!
We can now ispect the container
```sh
docker logs  
```
docker log


# FLOW 
```
            ┌─────────────────┐
            │   DOCKER HUB    │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │    MONGODB      │
            │   (pull image)  │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │ DEVELOPERS TEAM │
            ├─────────────────┤
            │ APPLICATION     │
            │                 │
            │ (in development)│
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │      COMMIT     │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │     SOURCE      │
            │     CONTROL     │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │   TRIGGER CI    │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │     BUILD       │
            │     DOCKER      │
            │     IMAGE       │
            │      APP        │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │   PUSH  IMAGE   │
            └───────┬─────────┘
                    │
                    ▼
            ┌─────────────────┐
            │   DOCKER HUB    │
            └─────┬──────┬────┘
                  │      │
                  ▼      ▼
  ┌───────────────────────────────────────┐
  │           PRODUCTION SERVER           │
  ├───────────────────────────────────────┤
  │                                       │
  │ ┌───────────────┐┌─────────────────┐  │
  │ │ APPLICATION   ││    MONGO DB     │  │
  │ │ (pull image)  ││   (pull image)  │  │
  │ └───────────────┘└─────────────────┘  │  
  └───────────────────────────────────────┘

```
