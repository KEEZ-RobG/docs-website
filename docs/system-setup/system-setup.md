---
layout: default
title: 1 System Setup
nav_order: 1
parent: The Guide
---

## Step 1: System Setup

In this section we will cover the hardware and operating system needed for running a node.

Requirments:
* USB Drive for Ubuntu installation
* Personal computer to prepare the USB drive


## Purchase Hardware
### Minimum Requirments
These are the minimum hardware requirements for a node machine. Always check [LUKSO's documentation](https://docs.lukso.tech/networks/l16-testnet/run-node#system-requirements) for updates.

* **OS:** Linux or Mac 
* **CPU:** 2 cores
* **RAM:** 16GB
* **Storage:** 100GB SSD (solid state is required)

For a future-proof setup, you may wish to choose a system with 4 cores and 1TB of SSD storage, and 32GB of RAM.

### Example Hardware
Mini PCs are designed for 24/7 use and low power consumption, making them ideal for running a LUKSO node. The Intel NUC line of mini PCs is a common choice within the community. Below are the specs and pricing of three different NUC models.




|           | Intel NUC 10<br> (Minimum)         | Intel NUC 10 |Intel NUC 10 <br>Performance Kit <br> |
| --------  | --------                           | -------- |-----
| Processor | i3 2-cores                         | i5 4-core| i7 6-core
| RAM       | 16GB                               | 16GB     | 32GB
| Storage   | 256GB                         | 512GB |1TB
| Price     | $489                               | $629     |$949
| Link      | [Amazon](https://a.co/d/3g1vg6G)   | [Amazon](https://a.co/d/1UdrolU)     |[Amazon](https://a.co/d/iE7niEu)

A backup system (or parts) is also something to consider. If your node is offline, you will incur slashing penalties roughly equal to the rewards you would have gained while being online. For example, if you average 1 LYX per day in rewards, you will lose 1 LYX per day if offline.




## Install OS
For this guide, we will use Ubuntu 22.04 LTS Desktop operating system, which is most commonly used by the community.

### 1-1 Install Ubuntu
To install Ubuntu, following the <a href="https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview" target="_blank" rel="noopener">official Ubuntu's official guide.</a> Be sure to use a strong password when creating your user accont.

### 1-2 Configure Auto Start

It is important to set up your node to power back automatcially on after a power outage. The setting is usually found in the BIOS, but some systems use a jumper on the motherboard. Refer to your hardware manual for instructions.

For Intel NUC, follow these steps:
1. Press F2 during boot to enter BIOS setup
2. Go to `Power` -> `Secondary Power Settings` menu
3. Set the option for `After Power Failure` to `Power One`
4. Press F10 to save changes and exit BIOS

Test the setting by unplugging the power cord while the system is running. It should turn on and boot when you plug it back in.

### 1-3 Update System Software
>NOTE: All text in the grey code boxes are to be copy/pasted into the Ubuntu Terminal

Open the Terminal application and run the following commands. Multiple commands can be copy/pasted at the same time. To paste into the terminal window, right click on the screen and select paste.
>HINT: Use the shortcut `crtl`+`alt`+`T` to quickly open the Terminal
```
sudo apt update
sudo apt upgrade -y
```

### 1-4 Install Dependencies
Nano is the command-line text editor used throughout this guide.
```
sudo apt install -y nano wget make git
```
### 1-5 Reserve Node IP Address

>NOTE: These steps will require access to your router's internal settings.

Address reservation ensures your router always assignesthe same IP address to your node. You will need the following information for these steps:
* The IP address of your router
* The IP address of your node machine
* The username and password of your router

#### Router Login

Find the IP address of your router:
```
netstat -nr | awk '$1 == "0.0.0.0"{print$2}'
```
Open a web browser and enter your router's IP address.

A username and password prompt will appear. If you do not know your credential, you will need to reset your router to its default setting. Refer to your routers manual for instructions.

#### Configure Router


Find the setting for reserving IP addresses. The names of this setting can vary from router to router. It is often found under “DHCP Settings” or “DHCP Reservation.” Refer to your router’s manual for specific instructions.

Find the IP address of your node:
```
hostname -I
````
Configure the router to reserve this address



---
Credits
[Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)
[Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)


---

Proceed to the next step: Remote Access
