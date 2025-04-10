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

## Create Python Virtual Environmnet

If venv is not installed install it with:

```
sudo apt-get install python3.12-venv
```

Create a new virtual environment and activate it:

```
python3.12 -m venv env
source env/bin/activate
```

### Download and Install the ZED SDK

Download the [ZED SDK for NVIDIA® Jetson](https://www.stereolabs.com/developers/release). Multiple versions of JetPack are supported, make sure to select the one that matches your system.

Go to the folder where the installer has been downloaded.

```
cd path/to/download/folder
```

Add execution permission to the installer using the chmod +x command. Make sure to replace the installer name with the version you downloaded.

```
chmod +x ZED_SDK_Tegra_L4T35.3_v4.0.0.zstd.run
```

Run the ZED SDK installer.

```
./ZED_SDK_Tegra_L4T35.3_v4.0.0.zstd.run
```

At the beginning of the installation, the Software License will be displayed, hit q after reading it.

During the installation, you might have to answer some questions on the installation of dependencies, tools and samples. Type y for yes and n for no and hit Enter. Hit Enter to pick the default option.

On Jetson™ boards, CUDA is automatically installed with the JetPack so you’re now ready to use the ZED SDK.

## Install Dependencies

Make sure virtual environment is activated.

```
pip install -r requirements.txt
```

# Usage
