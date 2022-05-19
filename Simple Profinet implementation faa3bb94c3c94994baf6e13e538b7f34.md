# Simple Profinet implementation

# **Introduction**

In order to test the communication through Profinet we used a sample application created by rt-labs to send logic signals of 1 or 0 to a PLC (IO controller) from a raspberry pi model B+ (IO device).

### Topics

- rt-labs repository download
- Raspberry pi set up
- Download and compile p-net
- Run the sample application
- Set up the PLC
- Simulation files

**[This tutorial was tested with a raspberry pi model B+, a S71500(CPU 1516-3 PN/DP) and TIA Portal v16.]**

## RT-LABS repository download

Further on in this repository the existing files in the rt-labs repository will be needed where the GSDML files and the files attached to the IO device in the TIA Portal are present. You can download it with the link below:

```html
[https://github.com/rtlabs-com/p-net](https://github.com/rtlabs-com/p-net)
```

Click on the button “Code”.

![Figure 1](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924.png)

Figure 1

Click on the button “Download ZIP”.

![Figure 2](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%201.png)

Figure 2

Now extract the .zip files to your desired location, when you need these files it will be explained in the tutorial.

## Raspberry pi set up

### Imager download

First of all we will need to install Raspberry Pi OS image to SD card using Raspberry Pi Imager.

You can install it with the link below:

```html
[https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/) 
```

Click on the Download for Windows button.

![Figure 3](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Sem_ttulo.png)

Figure 3

Open the downloaded file.

![Figure 4](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Sem_ttulo%201.png)

Figure 4

Click on the CHOOSE OS option then choose the recommended option which is the full version.

![Figure 5](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Sem_ttulo%202.png)

Figure 5

Click on the CHOOSE STORAGE option and select your SD card (8 GB minimum storage). Then click the WRITE button to finish the installation on the SD card.

## Raspberry pi set up

For an easier use of the raspberry pi it is advisable to use a mouse, keyboard and monitor. If you have already connected the raspberry pi to the network and defined its ip if you enable the VNC option by going to: Menu > Preferences > Raspberry Pi Configuration > Interfaces. You will be able to use your PC to access the raspberry pi by installing the respective VNC viewer on your PC (link below). With this you won't need to use a mouse, keyboard and monitor for the raspberry pi.

```html
[https://www.realvnc.com/pt/connect/download/viewer/](https://www.realvnc.com/pt/connect/download/viewer/) 
```

First we insert the SD card and do the login (choose any name and password you like), then we'll update the raspberry pi and install the latest version of **cmake.** To do this open the terminal and enter the following commands in command line:

```python
sudo apt update
sudo apt install snapd
sudo reboot 
sudo snap install cmake --classic
```

Verify the installed version (at the time of the tutorial, we used version 3.19, but you should have version 3.14 or newer):

```python
cmake --version
```

You also need **Git** to download the github repository of p-net from rt-labs. Install it inserting the following commands to the command line:

```python
sudo apt install git
```

## Download and compile p-net

First we need to create a directory and to do it, use the following commands:

```python
mkdir /home/(user name)/profinet/
cd /home/(user name)/profinet/
```

Clone the GitHub repository:

```python
git clone --recurse-submodules https://github.com/rtlabs-com/p-net.git
```

This command will clone the repository with submodules.

after that, create and configure the build:

```python
cmake -B build -S p-net
```

Build the code:

```python
cmake --build build --target install
```

## Run the sample application

Run the sample app in the build directory:

```python
cd /home/(user name)/profinet/
cd build
```

Enable the Ethernet interface and set the initial IP address:

```python
sudo ifconfig eth0 192.168.228.146 netmask 255.255.255.0 up
```

Run the sample application:

```python
sudo ./pn_dev -v
```

The IP settings are stored to a file. If you accidentally have run the application when IP settings were wrong, use this command to remove the stored settings:

```python
sudo ./pn_dev -r
```

Now you have installed the sample app on the Raspberry Pi. In order to see it in action, you need to connect it to a PLC.

# Set up the PLC

For this tutorial was used a S71500(CPU 1516-3 PN/DP) PLC connected to a switch where the raspberry pi and the PC are also connected. It is recommended that the three devices are connected on the same network to avoid connection errors, such as failures in the delivery/return of the signal, between the PLC and the raspberry pi.

The next step will be using TIA Portal v16 to configure everything that is needed. To start click on the “Create new project” button, choose a name and select version v16.

![Figure 6](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%202.png)

Figure 6

Then click on “Configure a device”.

![Figure 7](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%203.png)

Figure 7

In the “Configure a device” menu choose the PLC that you will use. For the sake of this tutorial we used a S71500(CPU 1516-3 PN/DP) 6ES7 516-3AN01-0AB0.

![Figure 8](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%204.png)

Figure 8

Now head to “Network view”.

![Figure 9](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%205.png)

Figure 9

On the right side of the screen under “Hardware catalog”, in the search bar search for rt-labs and then choose “P-Net Sample App”.

![Figure 10](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%206.png)

Figure 10

Now in the centre of the screen should appear the device as demonstrated in the image below.

![Figure 11](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%207.png)

Figure 11

The next step will be to connect the two devices as shown in the picture. To do that, just click and drag over the green square in the middle of the IO controller and the green square of the IO device.

![Figure 12](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%208.png)

Figure 12

To set up both devices IP addresses, start with the PLC and then the IO device.  

![Figure 13](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%209.png)

Figure 13

![Figure 14](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%2010.png)

Figure 14

Double click on rt-labs-dev and the following window should appear. Inside that window, on the right hand side where it says “Hardware catalog”, you must drag the DIO 8xLogiclevel to the first slot below the X1 P1.

![Figure 15](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%2011.png)

Figure 15

Now by clicking on the IO device in IO cycle > update time, the option “calculate update time automatically” will be enabled by default, so you will need to change to “Set update time manually” and select the update time to 16 ms.

![Figure 16](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%2012.png)

Figure 16

In order to choose the GSDML file, you must go to: Options > Manage general station description files (GSD).

![Figure 17](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%2013.png)

Figure 17

Now select the place where you extracted the repository and choose the sample app folder. After selecting, the GSDML files will be displayed in this menu. Select the necessary file and press install.

![Figure 18](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%2014.png)

Figure 18

Now you should download the configuration to the PLC on the following button.

![Figure 19](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Captura_de_ecr_2022-05-09_163924%2015.png)

Figure 19

Then press the “Go online” button and select your settings.

![Figure 20](Simple%20Profinet%20implementation%20faa3bb94c3c94994baf6e13e538b7f34/Sem_ttulo%203.png)

Figure 20

At this point it will already be possible to see the communication between the devices by profinet.

## Simulation files

To see the value being sent by the IO device every 0.1 s, enter the following command into the raspberry pi:

```python
cd /home/(user name)/profinet/
cd build
watch -n 0.1 cat /home/(user name)/profinet/build/pnet_led_1.txt
```

Now to configure plain files instead of real buttons, use these commands:

```python
touch /home/(user name)/profinet/build/button1.txt
touch /home/(user name)/profinet/build/button2.txt
sudo ./pn_dev -v -b /home/(user nema)/profinet/build/button1.txt -d /home/(user nema)/profinet/build/button2.txt 
```

To manually write a value of 1 or 0 to the plain file in order to simulate press and release, you should use the following commands:

```python
echo 1 > /home/(user name)/profinet/build/button1.txt
echo 0 > /home/(user name)/profinet/build/button1.txt
echo 1 > /home/(user name)/profinet/build/button2.txt
echo 0 > /home/(user name)/profinet/build/button2.txt
```