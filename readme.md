# Installation

## Install ZED Link Driver
The driver can be downloaded from the [ZED X Camera Drivers] page of the Stereolabs website. Select the driver corresponding to your configuration and run:

```
sudo dpkg -i stereolabs-zedx_X.X.X-ZED-LINK-YYYY-L4TZZ.Z_arm64.deb
```

Where:

 - X.X.X is the driver version
 - YYYY is the ZED Link GMSL2 capture card model (i.e. MONO, DUO, QUAD)
 - L4TZZ.Z is the Jetson™ Linux (L4T) version

 You might need to install `libqt5core5a` if not already installed using the following command:

 ```
 sudo apt install libqt5core5a
 ```

Now, reboot the NVIDIA® Jetson™ device.

```
sudo reboot
```

## How to use ZED X with Docker

In order to use the ZED X and the ZED X Mini in a Docker container, a certain number of options and volumes are required when running the container.

To run an interactive container using docker run:

```
docker run --runtime nvidia -it --privileged -e DISPLAY \
  --network host \
  -v /dev/:/dev/ \
  -v /tmp/:/tmp/ \
  -v /var/nvidia/nvcam/settings/:/var/nvidia/nvcam/settings/ \
  -v /etc/systemd/system/zed_x_daemon.service:/etc/systemd/system/zed_x_daemon.service \
  -v ${HOME}/zed_docker_ai/:/usr/local/zed/resources/ \
  <docker_image> sh
```

NOTE: ensure that the L4T (Linux for Tegra) version of your host system matches the L4T version of the container you are using.

NOTE: the ZED GMSL2 driver must be only installed on the host machine, and not in the Docker container.