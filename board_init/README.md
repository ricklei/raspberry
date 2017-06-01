#Guide to setup Raspberry Pi3
This document describes the steps to setup a fresh Raspberry Pi3.

## VNC
###Choosing TightVNC
There are several VNC software. Here TightVNC is chosed because it's free open source software and provide compression to allow it to work over a slow network.
###Install the TightVNC server
Refresh the repository informaiton
> sudo apt-get update

Install the server software
> sudo apt-get install tightvncserver

###Start server
> /usr/bin/tightvncserver

Then the server is running. We can connect to this using :1 at the end of the IP address in the client.

###Adding Tightvnc to systemd startup
To have Tightvnc startup automatically we need to create a new startup file, which needs to be stored in the /etc/sysemd/system/ directory and end with the suffix ".service". I called this tightvncserver.service. This needs to be created as the root user.

> sudo nano /etc/systemd/system/tightvncserver.service
> 
1. [Unit]
1. Description=TightVNC remote desktop server
1. After=sshd.service
1. 
1. [Service]
1. Type=dbus
1. ExecStart=/usr/bin/tightvncserver :1
1. User=pi
1. Type=forking
1.  
1. [Install]
1. WantedBy=multi-user.target

You may need to change the user name on line 8.
Change the file so it is owned by root
> sudo chown root:root /etc/systemd/system/tightvncserver.service

Make the file executable
> sudo chmod 755 /etc/systemd/system/tightvncserver.service

Enable startup at boot
> sudo systemctl enable tightvncserver.service