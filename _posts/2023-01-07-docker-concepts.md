title: Docker and some common concepts
categories:
- Docker
- Containerisation
- Introduction
exerpt: "Docker and some common concepts"
---

Docker is a containerization platform that allows you to package an application with all of its dependencies into a standardized unit for software development. This makes it easier to develop, test, and deploy applications, as you can be sure that the application will run consistently regardless of the environment it is being run in.

There are several key concepts in Docker that you should be familiar with:

**Images**: A Docker image is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the application code, system tools, libraries, and runtime. Images are created using a series of commands called a Dockerfile, which is a text file that contains all the commands needed to build the image.

**Tags**: Docker images can be tagged with a specific version number, which allows you to refer to a specific image when running or deploying an application. This is useful for maintaining different versions of an application, as you can easily switch between them by specifying a different tag.

**Containers**: A Docker container is a running instance of a Docker image. When you run a container, you are creating a new instance of the image with a specific set of configurations and options. Containers are isolated from one another and from the host operating system, which makes it easy to run multiple containers on a single host without interference.

**Volumes**: A Docker volume is a persistent storage location that can be mounted into a container. This allows you to store data in a location outside of the container, which can be useful for storing application data or for sharing data between containers.

**Dockerfile**: A Dockerfile is a text file that contains all the commands needed to build a Docker image. The Dockerfile is used to automate the process of building a Docker image, as you can specify all of the commands needed to build the image in a single file.

There are several commands that you can use in a Dockerfile to specify how the image should be built:

**RUN**: The RUN command is used to execute a command during the build process. This is typically used to install dependencies or to build the application.

**CMD**: The CMD command is used to specify the default command that should be run when a container is started from the image. This is typically used to start the application.

**ENTRYPOINT**: The ENTRYPOINT command is similar to the CMD command, but it specifies the command that should always be run when a container is started from the image. This is useful for setting up an image to be run as an executable.

**COPY**: The COPY command is used to copy files from the host file system into the image. This is typically used to include application code or configuration files in the image.

I hope this helps to give you a better understanding of Docker and some of the key concepts and commands involved in using it. If you have any questions, feel free to ask.