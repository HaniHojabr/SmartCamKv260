# SmartCamKV260

This tutorial shows how to run Smart Camera Project on Kria Kv260 board.
Details can be found [here](https://www.xilinx.com/products/som/kria/kv260-vision-starter-kit/kv260-getting-started/getting-started.html)

1) Download the [Kria Ubuntu Desktop 22.04 LTS Image](https://drive.google.com/file/d/1l3sUqZDmzPQo-70NrrBYVKosOTTpTyFr/view?usp=share_link) and save it on your computer.
2) Download the [Balena Etcher](https://www.balena.io/etcher/) (recommended; available for Window, Linux, and macOS). Other simiral tools to write the image to a SD card can be utilized.
3) Follow the instructions in the tool and select the downloaded image to flash onto your microSD card.
4) Insert the microSD card containing the boot image in the microSD card slot on the Starter Kit.
5) Get your USB-A to micro-B cable (a.k.a. micro-USB cable), which supports data transfer. Connect the micro-B end to J4 on the Starter Kit
6) Connect the USB Camera to U44 or U46.
7) Connect to a monitor with the help of a HDMI cable.
8) Connect the Ethernet cable for required internet access
8) Grab the Power Supply and connect it to the DC jack (J12) on the Starter Kit.

9) Based on your OS you can follow the instructions:

A) #For Windows, use the Device Manager to observe which COM ports appear when you plug the USB cable attached to the KV260 Starter Kit into your computer.

The Starter Kit uses an FTDI USB to COM port device that requires the FTDI virtual COM port driver to be installed on your machine. If the driver is not already installed on your host machine or Windows has not automatically installed it, go to the following link:

https://ftdichip.com/drivers/vcp-drivers/

Four COM ports are enumerated where the second numbered COM port corresponds to the UART.

Configure your terminal program (e.g., TeraTerm, PuTTy) with the settings shown below.

Baud rate = 115200
Data bits = 8
Stop bits = 1
Flow control = None
Parity = None

B) #For Linux, open a terminal by right clicking on the Desktop and selecting “Open In Terminal”, and enter the following command to observe which COM ports appear when you plug in the USB cable attached to the KV260 into your computer.

$ dmesg | grep tty

Four COM ports should be enumerated. The second numbered COM port corresponds to the UART.​

Configure your UART within the same terminal. You can also configure your terminal program with the same settings below. These settings are the same as defined in the Windows set-up.

$ sudo putty /dev/ttyUSB2 -serial -sercfg 115200,8,n,1,N

The Starter Kit uses an FTDI USB to COM port device that requires the FTDI virtual COM port driver to be installed on your machine. If the driver is not already installed on your host machine or Linux does not automatically detect it and load the appropriate driver, go to the following link for additional guidance: 

https://ftdichip.com/drivers/vcp-drivers/

10) Power ON the Starter Kit by connecting the power supply to the AC plug.
11) The Starter Kit QSPI boots the board using SD boot mode and loads the SD contents to boot into Linux. At initial log-in, the platform requires you to set a new password. When the system boots to Linux and asks for a login username, enter default username as “petalinux” and set a new user password.

To enable the root user, use the command below because “petalinux” has limited privileges. Enter the same password that you created for “petalinux” in the above step.

sudo su -l root

Verify Internet connectivity via “ping” or “DNS lookup.”

ping 8.8.8.8

If you can observe that packet transmit/receive worked and there is no packet loss with the above ping command, this means your Internet connectivity is working and active.

12) Run the below command to get the list of available application package groups.
 
sudo xmutil getpkgs

13) Run the below command to install Smart Camera accelerated application package group from the above listing. Press “y” when prompted and wait for approximately two minutes for 204 packages to install.

sudo dnf install packagegroup-kv260-smartcam.noarch

14) Run the below command to list the existing application firmware available on the Vision AI Starter Kit.

sudo xmutil listapps

15) Run the below command to unload default “kv260-dp” application firmware.

sudo xmutil unloadapp

16) Run the below command to load Smart Camera accelerated application firmware.

sudo xmutil loadapp kv260-smartcam

17) place the recommended USB webcam pointing to the users face and run the following command:

sudo smartcam --usb 0 -W 1920 -H 1080 -r 30 --target dp

Note: The argument 0 for “--usb” depends on which media node the USB webcam was detected by running Linux on Vision AI Starter Kit. In this case, it was /dev/media0, so we used “--usb 0.
