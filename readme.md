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