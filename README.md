# nRF52 Blinky Demo

Example project setup, flash, and debug firmware using Visual Studio Code.

Toolchain is cross-platform, however the instructions below are specifically for Linux.

## Prerequisites

### Download Tools

1. Download [nRF52 SDK](https://www.nordicsemi.com/Software-and-Tools/Software/nRF5-SDK)
2. Download [nRF52 Command Line Tools](https://www.nordicsemi.com/Software-and-Tools/Development-Tools/nRF5-Command-Line-Tools)
3. Download [Segger J-Link Software Tools](https://www.segger.com/downloads/jlink)
4. Download [GNU-RM Embedded Toolchain for ARM](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)
  - It's recommended to install the GCC version that matches the Nordic SDK version. Check the GCC version in `<sdk>/components/toolchain/gcc/Makefile.posix` and download the appropriate version.
  - For nRF5 SDK 15.3.0, the gcc version is `gcc-arm-none-eabi-7-2018-q2-update`

### Setup Tools

```bash
# Unpack SDK to home directory
unzip nRF5_SDK_15.3.0_59ac345.zip -d $(HOME)

# Unpack gcc toolchain to /usr/local
sudo tar -xjvf gcc-arm-none-eabi-8-2018-q4-major-linux.tar.bz2 --directory /usr/local

# Unpack nRF command line tools and make accessible in terminal
sudo tar -xvf nRF-Command-Line-Tools_9_8_1_Linux-x86_64.tar --directory /opt/
sudo ln -s /opt/nrfjprog/nrfjprog /usr/local/bin/nrfjprog
```

Check `nrfjproj --version` that it's been installed correctly. Output should be something like:

```
nrfjprog version: 9.8.1
JLinkARM.dll version: 6.44f
```

### Setup SDK

In the nRF52 SDK folder, update the values in `components/toolchain/gcc/Makefile.posix`:

- `GNU_INSTALL_ROOT`
- `GNU_VERSION`

This is only required if using a different gcc version than specified. It's recommended to use the same one as the SDK.

## Build the Project

### Setup

In `blinky/` directory, do a global search and replace to update the SDK root to wherever your nRF5_SDK directory is:

- From: `SDK_ROOT := ../../../../../..`
- To: `SDK_ROOT := $(HOME)/nRF5_SDK_15.3.0_59ac345`

### Build and Flash

```bash
cd blinky/pca10056/mbr/armgcc

# To just build
make

# To build and flash
make flash
```

Example output:

```
DONE nrf52840_xxaa
Flashing: _build/nrf52840_xxaa.hex
nrfjprog -f nrf52 --program _build/nrf52840_xxaa.hex --sectorerase
Parsing hex file.
Erasing page at address 0x1000.
Applying system reset.
Checking that the area to write is not protected.
Programming device.
nrfjprog -f nrf52 --reset
Applying system reset.
Run.
```

### To Debug

In Visual Studio Code, install the Cortex-Debug extension.

Open the debug pane (`CTRL+SHIFT+D`) and select **Cortex-Debug**.

To create a new configuration, select **Add Configuration** and choose **Cortex-Debug**.

In `.vscode/launch.json`, update the `executable` and/or `armToolchainPath` if required.

Hit `F5` to start debugging.
