## Apcupsd NUT Wrapper Script (Apcupsd and NUT on the same machine)

Hi,

i had a problem. I have a Debian based Linux machine (works for Ubuntu or a Raspberry too) with an APC ups connected via USB cable (works also via ethernet client) running the lovely,
small and simple **apcupsd**. Also another PCs (running Windows) are connected to the same ups, so i installed **apcupsd** on Windows too and connected
it to the apcupsd on the Linux machine, fine. Then i needed an additional **NUT server** so i can hook up my **Synology NAS** also to the same ups.
But, trying to install the complicated NUT-Server alongside **apcupsd** on the same linux machine was a pain. 

So, i wrote this little script that emulates a **NUT Server** and gets the values from `apcupsd/apcaccess`.

It is working great for me here and also in my company and for my friends, should work with other NUT Clients too not only Synology. Maybe someone finds it useful... 😄

Ciao Martin

&nbsp;<br>

### Description:

This little quiet non-optimized script emulates a **NUT-Server** together with the tiny "tcpserver"
from the ucspi-tcp package. It **needs** an installed and working **apcupsd** running on the machine
or on a remote machine. It is working fine with Synology NAS for example (my usecase).

The script is simple and small and solves some problems having **apcupsd and a NUT-Server** on the
same machine. Use it if you like, but don't scream at me if it's doing something wrong.
Please feel free to make this script better, but send me a copy (email above) if you're done. :-)

&nbsp;<br>

### Install / Running:

  1. You need an installed and running apcupsd on the same machine or on a remote machine.
     You also need apcaccess on this machine. Should be all fine if you installed apcupsd like:
     ``` console
     apt-get update && apt-get install apcupsd
     ```
     If you're not sure, please run the command
     ``` console
     apcaccess
     ```
     in the shell and see if you get your ups data. Or run
     ```console
     apcaccess -h <remoteip>
     ```
     to get the data of apcupsd running on a remotemachine with the IP `<remoteip>` (f.e. 192.168.1.10).

     &nbsp;<br>
  
  1. Copy the script `upsnutwrapper.sh` into the directory `/usr/local/bin/` and make it executable.
     You can simply run these commands to copy the script to the right location:
     ``` console
     wget https://github.com/gitmachtl/various/raw/main/upsnutwrapper/upsnutwrapper.sh -O /usr/local/bin/upsnutwrapper.sh
     chmod +x /usr/local/bin/nutwrapper.sh
     ```
     &nbsp;<br>
  
  1. Install the ucspi-tcp package via
     ``` console
     apt-get update && apt-get install ucspi-tcp
     ```

  1. Start the NUT-Server-Wrapper by executing the following command via shell or a script:
     ``` console   
     tcpserver -q -c 10 -HR 0.0.0.0 3493 /usr/local/bin/upsnutwrapper.sh &
     ```

     This starts a listening tcp server on port 3493(nut) with no binding (0.0.0.0), max. 10 simultanious connections.
     
     Another method is to simply put it into the crontab file so it starts with the system, just add an entry like:
     ```
     @reboot  <user>  tcpserver -q -c 10 -HR 0.0.0.0 3493 /usr/local/bin/upsnutwrapper.sh &
     ```
     Replace `<user>` with root or the user you want to run the script in the background.

&nbsp;<br>

❤️ **Enjoy your apcupsd and "Nut Server" side by side on the same machine 😄**

&nbsp;<br>

_________________

### Details:
```
Script:       upsnutwrapper.sh
Author:       Martin (Machtl) Lang
E-Mail:       martin@martinlang.at
Version:      1.2 (15.02.2019)
```
  
### History:
```
  1.0:    First working version "yeah"
  1.1:    Changed the "apcaccess" call and added the option "-u"
  1.2:    Added many parameters
```