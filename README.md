# TRMM-RustDesk-Integration
### Vegetable8

Getting Rustdesk integrated with [Tactical RMM](https://github.com/amidaware/tacticalrmm) is extremely simple. Thanks to the amazing work from [dinger1986](https://github.com/dinger1986) creating all scripts used here, and a simple tutorial to walk you through it all.

## Installing RustDesk Proxy Server
For this, you can install the Rustdesk(RD) Proxy server on the same host you use to install TacticalRMM, or a seperate host. In this example, I will be installing it on the same host TRMM is installed on (Ubuntu 20.04 LTS).

```
sudo apt-get update
```
Create a TRMM Backup in case something does not go right.
```
wget -N https://raw.githubusercontent.com/amidaware/tacticalrmm/master/backup.sh
chmod +x backup.sh
./backup.sh
```
### Install RustDesk Server
Set UFW Rules (If using UFW, if not `sudo ufw disable`)
```
ufw allow 21115:21119/tcp
ufw allow 8000/tcp
ufw allow 21116/udp
sudo ufw enable
```
Installing Rustdesk Server
```
wget https://raw.githubusercontent.com/dinger1986/rustdeskinstall/master/install.sh
chmod +x install.sh
./install.sh
```
Run through the sections of the script

![image](https://user-images.githubusercontent.com/50916823/213038323-56314f50-34dd-49b1-a217-41aaa68318f8.png)

Select Option 1- or Yes

![image](https://user-images.githubusercontent.com/50916823/213042414-170185eb-2ac3-4b72-b7e9-a0dc7784e42b.png)


This will make a small HTTP server, that you can go to to grab a pre-made PS1/SH script. These scripts automatically install the Rustdesk client configured to your Rustdesk proxy that you just installed

- These are important scripts to download to save for a later step

### Post Rustdesk Installation
If the RustDesk server is behind NAT or a firewall, ensure to forward these ports
- 21115-21119 **TCP**
- 21116 UDP

*Personally, I would not forward port 8000 (URL To grab scripts from), as it does not seem relevant past this point to have available*

## TacticalRMM Integration

#### Importing Scripts

Open the .PS1 script in a text/code editor and copy all of the contents from the PS1

Open TacticalRMM > Settings > Script Manager > New (Top Left) > New Script

Paste the contents into the code editor, name the Script (Rustdesk- Install), ensure the "Shell Type" is Powershell and set the "Supported Platforms" to Windows. Click Save once done.

Follow the same process for linuxclientinstall.sh.

#### Setting up Collector Task

Inside of TacticalRMM > Settings > Global Settings > Custom Fields > Add Custom Field

Target: Agent <br/>
Name: rustdeskid <br/>
Field Type: Text
Default Value: N/A

Inside of TacticalRMM > Settings > Automation Manager > Click on an existing Automation Policy (Or create one now) > Tasks (Middle Region) > Add Task

Name: RustDesk- Get ID<br/>
Select "Collector Task"<br/>
Select "Save all output"<br/>
Set Alert Severity "Informational
