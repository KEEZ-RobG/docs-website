---
layout: default
title: LAN
parent: 2 Remote Access
grand_parent: The Guide
nav_order: 1

---
# SSH LAN - Ubuntu

## Terminology
We will be switching between two devices in this guide, your LUKSO node (the server) and a personal device (the client). They will be referred to as:
* **node machine** (server)
* **personal device** (client)


## Configure Node Machine

Perform these step on your **node machine**.

### Install SSH

```
sudo apt install --assume-yes openssh-server
```

### Confiugre SSH



Open the SSH config file

```
sudo nano /etc/ssh/sshd_config
```
**Enable SSH Port**

Find the line that says `#Port 22` and remove the `#` sign.

**Change SSH Port Number**

The default SSH port (22) should be changed to a random number for security reasons.

Choose a number between 1024 thru 49141. 
***MAKE NOTE:*** This number will replace `<ssh-port>` in this guide.

Change `Port 22` to `Port <ssh-port>`

Close the editor by pressing `ctrl` + `X`, then `y` , then `Enter`

### Configure Firewall

Open the SSH port in your firewall. Remember to replace `<your-port>` with the number you chose.

```
sudo ufw allow <your-port>/tcp
```
    
Enable SSH

```
sudo systemctl start ssh
sudo systemctl enable ssh
```

Determine Node IP Address

```
hostname -I
```
***MAKE NOTE:*** this address will replace ``<node-ip>`` in this guide.


## Configure Personal Device (Ubuntu)
Perform these step on your **personal device**.



### Update SSH Config
The SSH command requires the username, IP address, and port number of the node machine. 

Simplify the SSH command by updating the SSH config file with your node credentials.
```
nano ~/.ssh/config 
```

Copy/Paste the following into the config file.
Replace `<node-user>`, `<node-ip>`, and `<ssh-port>`
IP address and port number have been determined in previous steps. 
`<node-user>` is your node machine's user name.

```
Host lukso
  User <node-user>
  HostName <node-ip>
  Port <ssh-port>
```

Attempt to connect to verify the configuration:

```
ssh lukso
```
Enter the password of your node machine. 

You should now see the command line of you node machine.

Type `exit` to close the SSH connection. You are not back on the command line of your personal device.

### Generate SSH Keys
SSH is more secure when using public/pricate keys instead of a password. In this step we well generate keys on your personal device and send the public key to the node machine.

On your **personal device**, create a new key pair for ssh authentication.

```
ssh-keygen -t rsa -b 4096
```

Copy the public key **keyname.pub** to a node machine. Replace **keyname.pub** with a key in home directory.

```
ssh-copy-id -i ~/.ssh/keyname.pub lukso
```

### Disable Non-Key Remote Access

On a personal computer, try to ssh again. This time it should not prompt for a password.

```
ssh lukso
```

Open the SSH configuration file.

```
sudo nano /etc/ssh/sshd_config
```

Find and modify the following options:


`ChallengeResponseAuthentication no`
`PasswordAuthentication no`
`PermitRootLogin prohibit-password`
`PermitEmptyPasswords no`

Save changes and close editor. (`ctrl` + `X`, `y` , `Enter`)

Validate SSH configuration and restart ssh service.

```
sudo sshd -t
sudo systemctl restart sshd
```

Close the ssh connection

```
exit
```

### Verify Remote Access
Reconnect to your node machine
```
ssh lukso
```
Stay connected to a remote node machine to perform next steps.

### Keep System Up to Date
    
Update your system manually:
    
```
sudo apt-get update -y
sudo apt dist-upgrade -y
sudo apt-get autoremove
sudo apt-get autoclean
```

Configure your system to automatically update:

```
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

### Disable Root Access
    
A root access should not be used. Instead, a user should be using the `sudo` command to perform privilged operations on a system.

```
sudo passwd -l root
```

### Block Unathorised Access

Install Fail2ban to block IP addresses that are attempting to access our node. Fail2ban blocks addresses after a certain number of failed attempts.

```
sudo apt-get install fail2ban -y
```

Edit a config to monitor ssh logins

```
sudo nano /etc/fail2ban/jail.local
```

Replace *replace-port* to match the ssh port number.

```
[sshd]
enabled=true
port=replace-port
filter=sshd
logpath=/var/log/auth.log
maxretry=3
ignoreip=
```

Save changes and close the editor

Restart the service

```
sudo systemctl restart fail2ban
```


### Improve SSH Connection

WiFi power managment may slow down SSH connections. Modifying a config file will disable it.

```
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```

Find the wifi.power setting and change to to match the following:
```
[connection]
wifi.powersave = 2
```

Save changes and close the editor.

Restart `NetworkManager` service:

```
sudo systemctl restart NetworkManager
```


---

Proceed to the next step: L16 Testnet Node & Validator


---
Credits
[Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)
[Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)
