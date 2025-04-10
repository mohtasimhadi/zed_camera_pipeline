# Installation

## Setting up Docker
Install Docker on you local host machine

```
# Automatic setup script
curl -sSL https://get.docker.com/ | sh

# Run docker to make sure installation worked
sudo docker run hello-world
```

If Docker is correctly installed you should get a message like this:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

📌 Note: To run docker commands without sudo, create a Unix group called docker and add users to it. Note that the docker group grants privileges equivalent to the root user. For more information, see Docker docs.

NVIDIA® Docker

📌 Note: For NVIDIA® Jetson™ boards, skip the step below and jump to the Docker install guide for NVIDIA® Jetson.

Install NVIDIA Container Toolkit support using the commands below. This lets you build and run GPU accelerated Docker containers.

📌 Note: Make sure you have installed the NVIDIA® driver for your graphics card before installing nvidia-container-toolkit.

```{bash}
# Add the package repositories
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## Download a ZED SDK Docker Image
To build and run an application using the ZED SDK, you need to pull a ZED SDK Docker image first. The official ZED SDK Docker images are located in Stereolabs DockerHub repository. The releases are tagged using ZED SDK, CUDA and Ubuntu versions, along with optional support for OpenGL or ROS.

For example, the following downloads these ZED SDK images to your machine:

```
docker pull stereolabs/zed:4.2-runtime-cuda12.1-ubuntu22.04   # Ubuntu 22.04 runtime release 4.2 with CUDA 12.1 
docker pull stereolabs/zed:4.2-gl-devel-cuda11.4-ubuntu20.04  # Ubuntu 20.04 development release 4.2 with OpenGL support and CUDA 11.4
docker pull stereolabs/zed:4.0-devel-cuda11.8-ubuntu20.04     # Ubuntu 20.04 development release 4.0 with CUDA 11.8
```

## Start a Docker Container
To run a Docker container using a ZED SDK image, use the following command:

```
docker run --gpus all -it --privileged stereolabs/zed:<container_tag>
```

The --gpus all command adds all available GPUs to the container, and --privileged grants permission to the container to access the camera connected on USB.

For example:

```
docker run --gpus all -it --privileged stereolabs/zed:3.0-runtime-cuda9.0-ubuntu18.04
```

Congratulations, the ZED SDK is now available in your container!

## Test the Docker container with ZED SDK capabilities

To verify our installation, we are going to run the ZED Explorer tool. By default, a Docker container can only be used to run command-line applications. If we want to add a display window, we need to use a container with OpenGL support.

```
docker pull stereolabs/zed:3.7-gl-devel-cuda11.4-ubuntu20.04  # pull ZED SDK v3.7.x devel release with OpenGL support under Ubuntu 20.04 
xhost +si:localuser:root  # allow containers to communicate with X server
docker run -it --runtime nvidia --privileged -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix stereolabs/zed:3.7-gl-devel-cuda11.4-ubuntu20.04
--runtime nvidia will use the NVIDIA® container runtime.
-v /tmp/.X11-unix:/tmp/.X11-unix is required for the container to access the display.
-e DISPLAY makes your DISPLAY environment variable available in the container.
```

Now you can run any OpenGL application in the container. Let’s run ZED Explorer:

```
/usr/local/zed/tools/ZED_Explorer
```

You should now be looking at the Explorer GUI application running on your display from a Docker container!

## Run a Sample Application

In this example, we will run a simple Depth Sensing sample application available in the ZED SDK sample folder.

```
apt update && apt install cmake -y
cp -r /usr/local/zed/samples/depth\ sensing/ /tmp/depth-sensing
cd /tmp/depth-sensing/cpp ; mkdir build ; cd build
cmake .. && make
./ZED_Depth_Sensing
```

## Next Steps

Read the next section to learn how to create your own Docker image for your application.