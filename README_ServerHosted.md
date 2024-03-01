> Alternatively if you want to run a Starbound server on a virtual machine or Linux host here is a setup guide
# Manual Install using VM as server

Setup and install example was done on Ubuntu 16.04 server
Some variables may change for networiking if you use a newer version of Ubuntu

```
sudo apt-get update && sudo apt-get install lib32gcc1 libvorbisfile3
```
*add service user
```
sudo adduser steam
```
*switch to that user
```
su - steam
```

*make directory and cd to it
```
mkdir steamcmd
```

```
cd steamcmd
```

*download Steam CMD
```
wget [https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz](https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz)
```
*verify files are there
```
ls -l
```

*extract the files
```
tar -zxvf steamcmd_linux.tar.gz
```

*verify files are there
```
ls -l
```

# Install Server

```
./steamcmd.sh
```

*login to steam
```
steam>login STEAMUSERNAME

steam>force_install_dir ./starbound_server

steam>app_update 211820

steam>quit
```
```
vim update_starbound_server.sh
```
> ```nano``` can also be used if preffered over ```vim```
Press I to enter insert mode
```
#!/bin/bash
```

```
./steamcmd.sh +login STEAMUSERNAME +force_install_dir ./starbound_server +app_update 211820 validate +quit
```

> If using ```vim``` hit ESC then type :wq
> if using ```nano``` use Ctrl+X Press Y and hit Enter to save

*set permissions for the .sh file to update server
```
chmod 700 update_starbound_server.sh
```
*run the script
```
./update_starbound_server.sh
```


# Running the Server

*run as steam user

*from a reboot login as steam user

*open the server directory
```
 cd ./starbound_server/linux
```

*run the server
```
./starbound_server
```

*press Ctrl+C to stop the server and open terminal again

*Edit the server config file

/[server directory]/storage
```
nano starbound_server.config
```

Ref: [https://starbounder.org/Guide:LinuxServerSetup#Server_Configuration](https://starbounder.org/Guide:LinuxServerSetup#Server_Configuration)

Change to match values below:

"allowAnonymousConnections" : true,

"ClientIPJoinable" : true,

"ClientP2PJoinable" : false,

"AllowAssetsMismatch" : true,

"TutorialMessages" : false,

# Advanced setup

*create the service file to have Starbound server service
```
sudo touch /etc/systemd/system/starbound-server.service
```

*open to edit this file
```
sudo -e /etc/systemd/system/starbound-server.service
```

*Add this to the file and save

[Unit]  
Description=StarboundServer  
After=network.target  
[Service]  
WorkingDirectory=/[server directory]/linux  
User=steam  
Group=steam  
Type=simple  
ExecStart=/[server directory]/linux/starbound_server  
RestartSec=15  
Restart=always  
KillSignal=SIGINT  
[Install]  
WantedBy=multi-user.target

*reload the system control
```
sudo systemctl daemon-reload
```

*start the new server service
```
sudo systemctl start starbound-server
```

*verify service status
```
sudo systemctl status starbound-server
```

*stop starbound server service
```
sudo systemctl stop starbound-server
```

*enable the service to start on boot
```
sudo systemctl enable starbound-server
```

*If your status showed active and running then you can reboot the VM to test

# Firewall rules

*install network tools
```
sudo apt install net-tools
```

*make note of the IP address for local connection on home network
```
ifconfig
```

## Setup firewall rules

*install 
```
sudo apt-get install firewalld
```

```
sudo firewall-cmd --zone=public --add-port=21025/tcp --permanent
```

```
sudo firewall-cmd --reload  
```

```
sudo systemctl enable firewalld
```

```
sudo systemctl start firewalld
```


# OPTIONAL
```
sudo apt-get install screen
```
 screen
```
cd /home/steam/steamcmd/starbound_server/linux
```

```
sudo ./starbound_server
```

*use ctrl + A D to detach session

To access terminal session again use
```
screen -r
```
```
byobu-enable
```

```
cd ~/steam/starbound/linux
```

```
./starbound_server
```

```
cd ~/steam
```


# Connecting

Launch Starbound

Join Game

IP: [VM IP]

You can setup usernames and password here. This would be suggested for any in game admin console stuff you may want to perform
