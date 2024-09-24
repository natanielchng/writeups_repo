# How to Set Up a Clean Development Environment with Docker and Alpine Linux

### Introduction
In this tutorial, you will set up a lightweight development environment using Docker and Alpine Linux. Docker works on containers, providing a lightweight and isolated space for running different programming languages and tools without affecting your main computer. Alpine Linux, known for its small footprint, is ideal for this setup.

By following this guide, you'll create a Docker-based environment where you can experiment with Python, Node.js, and other tools without installing them directly on your machine. You'll also set up a shared folder, allowing you to transfer files between your host machine and the Docker container.

By the end of this tutorial, you will have a fully functioning, isolated development environment. You'll be able to install and test different programming languages and tools quickly, keeping your main system clean and organized, which is perfect for coders who like to experiment.

## Prerequisites
To follow this tutorial, you'll need: 
- Docker installed on your machine. Follow the instructions for your system on the [Docker Official Website](https://docs.docker.com/engine/install/).
- Familiarity with Linux commands like `cd` (change directory), `mkdir` (create a directory), `ls` (list directory contents), and `nano` (text editor) will be helpful for navigating and editing files in the terminal.

## Step 1 -- Create a Project Directory
We'll start by setting up a project folder to hold our files.

Create the directory:
```sh
mkdir docker-dev-env
```

Navigate to the directory:
```sh
cd docker-dev-env
```

This will serve as the workspace for your Docker setup.

## Step 2 -- Create a Dockerfile
The Dockerfile is a text file where we define the instructions or commands for building our development environment within a container. This file is also conventionally named `Dockerfile`.

Create the Dockerfile:
```
nano Dockerfile
```

Add the following content:
```dockerfile
FROM alpine:latest
RUN apk add bash
CMD ["bash"]
```

- `FROM alpine:latest`: Specifies Alpine Linux as the base image. Alpine is lightweight, making it ideal for development.
- `RUN apk add bash`: Installs Bash in the container.
- `CMD ["bash"]`: Launches Bash when the container starts.

## Step 3 -- Build the Docker Image
Now we'll build the Docker image from the Dockerfile. The Docker image acts as a snapshot of everything you want in your container. It will be used to create and run your container.

Run the following command:
```sh
docker build -t docker-dev-env .
```

- `docker build -t docker-dev-env`: Builds the image and tags (names) it as `docker-dev-env`. 
- `.`: Indicates the current directory, where the Dockerfile is located.

## Step 4 -- Set Up a Shared Folder
To exchange files between your host machine and the container, we'll create a shared folder and use a feature called bind mount

First, create a folder:
```sh
mkdir shared-folder
```

This folder will be accessible inside the container, allowing easy file transfer between the host and container.

## Step 5 -- Run the Container
Now, we'll run the container using the image we built.

Run the container:
```sh
docker run --name dev-env-01 -it -v "$(pwd)/shared-folder:/app" docker-dev-env
```

- `--name dev-env-01`: Assigns a name to the container.
- `-it`: Runs the container in _interactive_ mode with a terminal. Interactive mode allows you to use the terminal inside the container just like you would on a regular computer.
- `-v "$(pwd)/shared-folder:/app"`: Mounts the `shared-folder` directory on the host to the `app` directory inside the container.

Once the container is running, the terminal will switch to the container's environment (the container ID will differ):
```
902f3cefc126:/#
```

## Step 6 -- Create a File in the Container
Let's create a file inside the container and check if it appears in the shared folder on the host.

List the contents of the current directory:
```sh
ls
```

You'll see the app directory (declared from the bind mount):
```sh
app    bin    dev    etc    home   ...
```

Navigate to the app directory:
```sh
cd app
```

Create a file:
```sh
touch someFile.txt
```

This file will now appear in the `shared-folder` on your host machine, showing that the bind mount works.

## Step 7 -- Install Programming Languages
Next, we'll install Python and Node.js in the container.

Run the following command:
```sh
apk add python3 nodejs
```

After the installation, confirm Python is installed by running:
```sh
python3
```

You should see:
```
Python 3.12.6 (main, Sep 24 2024, ...) on linux
```

Exit Python by typing `quit()`. Then, try running the Node.js interpreter:
```sh
node
```

You'll see the Node.js prompt:
```
Welcome to Node.js v20.15.1.
Type ".help" for more information.
>
```

Press `Ctrl+C` to exit. 

Using the container like this helps you experiment with different languages without cluttering your host machine.

## Step 8 -- Stopping and Restarting the Container
Once you're done working in the container, you can stop it without losing any data or configurations.

To stop the container, run:
```sh
docker stop dev-env-01
```

This command gracefully stops the container named `dev-env-01`.

To restart the container and pick up where you left off, use:
```sh
docker start -i dev-env-01
```

The `-i` flag opens the container in interactive mode again, allowing you to continue working inside the environment.

## Conclusion
You've successfully set up a lightweight development environment using Docker, created a shared folder between the host and container, and installed Python and Node.js. This isolated development setup ensures that your main system remains clean and uncluttered, making Docker a great tool for testing and experimenting.