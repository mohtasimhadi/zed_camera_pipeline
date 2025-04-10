# How to Install ZED SDK with Docker on NVIDIA® Jetson
## Setting Up Docker

On NVIDIA® Jetson™, the Container Runtime for Docker lets you build and run GPU-accelerated containers with Docker. It is included as part of NVIDIA® JetPack.

Check that it has been correctly installed by the NVIDIA® SDK Manager using the following command:

```
sudo dpkg --get-selections | grep nvidia
#libnvidia-container-tools       install
#libnvidia-container0:arm64      install
#nvidia-container-runtime        install
#nvidia-container-runtime-hook   install
#nvidia-docker2                  install

sudo docker info | grep nvidia
#+ Runtimes: nvidia runc
```

# Download a ZED SDK Docker Image

To build and run an application using the ZED SDK, you need to pull a ZED SDK Docker image first.

The official ZED SDK Docker images for Jetson™ are located in Stereolabs DockerHub repository. The releases are tagged using ZED SDK and JetPack versions. These images are based on the NVIDIA® l4t-base container, adding the ZED SDK library and dependencies.

An example:

```
docker pull stereolabs/zed:3.0-devel-jetson-jp4.2   # devel release 3.0 for JetPack 4.2
Please ensure that the L4T (Linux for Tegra) version of your host system matches the L4T version of the container you are using.
```

# Start a Docker Container
To run a Docker container using a ZED SDK image, use the following command:

```
docker run --gpus all -it --privileged stereolabs/zed:<container_tag>
The --gpus all command adds all available GPUs to the container, and --privileged grants permission to the container to access the camera connected on USB.
```

For example:

```
docker run --gpus all -it --privileged stereolabs/zed:3.0-devel-jetson-jp4.2
Congratulations, the ZED SDK is now available in your container!
```

# Run ZED Explorer Tool
To verify our installation, we are going to run the ZED Explorer tool. By default, a Docker container can only be used to run command-line applications. On NVIDIA® Jetson™, however, OpenGL, CUDA and TensorRT are ready to use within the l4t-base container.

To enable display output, we simply need to grant access to the X server when running the container:

```
xhost +si:localuser:root  # allow containers to communicate with X server
docker run -it --runtime nvidia --privileged -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix stereolabs/zed:<container_tag>
--runtime nvidia will use the NVIDIA® container runtime.
-v /tmp/.X11-unix:/tmp/.X11-unix is required for the container to access the display.
-e DISPLAY makes your DISPLAY environment variable available in the container.
<container_tag> must be replaced with the tag of the container to be used.
```

Now you can run any GUI application in the container. Let’s run ZED Explorer:

```
/usr/local/zed/tools/ZED_Explorer
```

You should now be looking at the Explorer GUI application running on your display from a Docker container!

# Run a Sample Application
In this example, we will run a simple Depth Sensing sample application available in the ZED SDK sample folder.

```
apt update && apt install cmake -y
cp -r /usr/local/zed/samples/depth\ sensing/ /tmp/depth-sensing
cd /tmp/depth-sensing ; mkdir build ; cd build
cmake .. && make
./ZED_Depth_Sensing
```