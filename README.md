# Splunk-Lab

## Objective

The Splunk & Sysmon Logging Lab is designed to enhance log collection, monitoring, and analysis capabilities within an enterprise environment. This project involves setting up a local Splunk server on Ubuntu and deploying Sysmon on an existing Windows Active Directory VM and Windows 10 client VM. The primary goal of this lab is to centralize and analyze security logs, gain insights into system activity, detect anomalies, and simulate real-world security monitoring use cases.

### Skills Learned

- Installation and configuration of Splunk on an Ubuntu server.
- Deployment and fine-tuning of Sysmon for advanced Windows logging.
- Integration of Windows event logs and Sysmon logs into Splunk for centralized analysis.
- Understanding of log collection, parsing, and indexing for security monitoring.
- Hands-on experience in detecting security events and investigating suspicious activity.
- Improved knowledge of SIEM (Security Information and Event Management) concepts.

### Tools Used

- Ubuntu Server (VM) – Hosting the Splunk server for log ingestion and analysis.
- Splunk – Collecting, indexing, and visualizing security logs.
- Windows Server 2022 (AD DC) – Simulated enterprise environment for logging security events.
- Windows 10 Client VM – Generating user and system activity logs.
- Sysmon (System Monitor) – Providing enhanced event logging on Windows systems.
- Group Policy Management – Deploying and managing Sysmon configurations.
- PowerShell & Command Prompt – Configuring Sysmon, checking logs, and testing Splunk queries.

## Steps

This lab will utilize an existing Windows Server 2022 AD DC and Windows 10 Client PC.

This is the link to the instructions for <a href="https://github.com/christianclark123/Active-Directory/tree/main">Active Directory Home Lab </a> in case you need to spin up these virtual machines. 

Head to <a href="https://ubuntu.com/server">Ubuntu Server Download </a> to download the Ubuntu Server that will host our Splunk. As of this lab, the current version of the Ubuntu server is 24.04.01 LTS.

![image](https://github.com/user-attachments/assets/dcef6435-84fa-4620-a892-961ef9915587)
![image](https://github.com/user-attachments/assets/644900a3-b50a-478d-a955-bae0f8a89664)

Once the Ubuntu server is downloaded create a new VM on VirtualBox.

On this VM name it "Splunk Server", head to the directory where your Ubuntu server ISO image is downloaded and check the box for "Skip Unattended Installation"

![image](https://github.com/user-attachments/assets/e84c27c7-165b-4d2d-9006-665d41040ee2)

For the Hardware set your base memory to at least 8GB (8192MB) and set your processors to 2 CPUs. This Splunk server will be ingesting data from our AD so it requires more specs. 

![image](https://github.com/user-attachments/assets/e0e65e7c-20f3-4c83-91ab-0e1e3cdaa47b)

For a Hard Disk, make sure you have at least 100GB of storage on your saved destination drive.

![image](https://github.com/user-attachments/assets/e3d39411-66eb-4d40-b11c-c64d1bc45704)

Spin up the Splunk Server and proceed through the installation.

On Profile Configuration name the profile and server name as you like and set a password.

![image](https://github.com/user-attachments/assets/31d78559-c968-444a-ba8a-d3ba930aceb7)

Continue with installation. Skip Ubuntu Pro, Install OpenSSH server is optional (For this lab I will not check that box), featured server snaps are optional as well. 

Reboot after installation and log on once the server is rebooted.

**NOTE** If a [FAILED] Failed unmounting cdrom.mount - /cdrom. appears hit enter.

![image](https://github.com/user-attachments/assets/c36557d5-2c0b-456b-8eb9-f4982a4cd8af)

**NOTE** Your password will be invisible when typing it on the Ubuntu login screen. This is by design and a security measure. 

Once logged on run the command "sudo apt-get update && sudo apt-get upgrade -y". This command updates and upgrades repositories for Ubuntu server. This is a sudo command so you will have to enter your password to give it permission to update and upgrade. 

![image](https://github.com/user-attachments/assets/dc7cde05-a22b-4469-bceb-d9ed4ee03ef2)

Change your network on your VirtualBox to the previously configured Nat Network used for the AD lab. (Settings -> Network -> Attached to -> NAT Network -> AD-Lab)

![image](https://github.com/user-attachments/assets/f0de6943-b3a6-46f7-980d-d5553667eb9f)

Run the command "ip a" on your splunk server and the connection point enp0s3 under inet give you the IP address for your Splunk server.

![image](https://github.com/user-attachments/assets/b297c389-31d0-490b-af1d-1d94c8b02eff)

To test connectivity type the command "ping google.com". In order to cancel pings use CTRL+C

![image](https://github.com/user-attachments/assets/e07d7616-02f8-4ec6-9003-9c75af3e2140)

Go to <a href="https://www.splunk.com/?locale=en_us">Splunk</a> on your Host machine sign up for an account and head to Free Trials & Downloads. 

![image](https://github.com/user-attachments/assets/3329d737-0a86-4c4f-bd5b-a3b5ecb22e09)

Download a free trial of the Linux 64-bit .deb Splunk Enterprise and save it to the directory of your choice.

![image](https://github.com/user-attachments/assets/fb000eb4-d9a2-42af-922f-6ed1eac27185)

On your Splunk VM install the guest addons for Virtual box by using the command "sudo apt-get install virtualbox-guest-additions-iso". Enter your password for sudo permissions and when prompted type "Y"

![image](https://github.com/user-attachments/assets/275666d9-721a-4920-b030-aef169120e64)

![image](https://github.com/user-attachments/assets/4ec2a899-aa57-47cf-a9fa-acfb70e4f973)

In order to import your Splunk Enterprise .deb download on your Splunk VM go to Devices -> Shared Folders -> Shared Folder Settings

![image](https://github.com/user-attachments/assets/d7b7eac1-2210-4338-964d-121fa680f2d3)

Add a folder by clicking the +Folder Icon 

![image](https://github.com/user-attachments/assets/481afc5a-f5f5-46e1-82ec-da35773a93ab)

Add the Folder Path of your Splunk server download, check the Read-only, Auto-mount, and Make Permanent checkboxes. Hit Ok twice to get back to your Splunk VM.

![image](https://github.com/user-attachments/assets/b1a1d012-65f8-473b-a990-49ebd04f6ea5)

Type "sudo reboot" and log back into your account.

Type "sudo apt-get install virtualbox-guest-utils"

![image](https://github.com/user-attachments/assets/c894ec9e-ec5d-47be-9b87-36ecc4575bca)

Type "sudo reboot" and log back into your account.

Add your user to the vboxsf group by typing "sudo adduser (user) vboxsf"

![image](https://github.com/user-attachments/assets/cc89fd1b-52e2-44cd-b7af-f11a78b2ff22)

Make a new directory named share by typing in the command "mkdir share"

![image](https://github.com/user-attachments/assets/e1bf7210-c8dc-4641-8eb5-4a7aedd7accc)

Run the following command to mount the shared folder to the newly created directory share "sudo mount -t vboxsf -o uid=1000,gid=1000 (Host Folder Name) share/"

![image](https://github.com/user-attachments/assets/c5666a22-db71-40da-892f-3fc99811a53c)

Your share directory should be highlighted when typing in "ls -la"

![image](https://github.com/user-attachments/assets/8c638c43-c111-49e6-9619-a4b90dd2b03b)

Change to the share directory by typing in "cd share/"
Type in "ls -la" to see the files in the directory, you should see your splunk .deb installer file. 

![image](https://github.com/user-attachments/assets/5dfc8ac5-f953-4dd0-a882-8bcde72825ee)

Type "sudo dpkg -i splunk(tab to complete)" to install your Splunk Enterprise

![image](https://github.com/user-attachments/assets/5c781fea-7ed3-4617-8973-4e192a8cad75)

When the installer is complete change to the splunk directory by typing "cd /opt/splunk"
Change to the splunk user by typing "sudo -u splunk bash" 

![image](https://github.com/user-attachments/assets/04dc4d7e-569c-487c-a8bc-943d3e1de6df)

Change to the splunk user binary directory by typing "cd bin/"
Type in "./splunk start" to start the installer. Space through the installer and when prompted type "Y" to agree with the license. Create an admin username and password and let it continue with the installation.
Once the installation is done type "exit" to exit the splunk user
Change to the binary directory "cd bin" and type "sudo ./splunk enable boot-start -user splunk" This makes it so that anytime the VM reboots it will run with Splunk as the user.

![image](https://github.com/user-attachments/assets/e3452f53-14c9-4778-8473-c7f656ce44c9)

On your Windows 10 Client PC log onto your Splunk Server by going to the web browser and typing in (Splunk ip address):8000 Splunk listens on port 8000

![image](https://github.com/user-attachments/assets/ad90bd22-4d01-49b8-b559-6469f3877085)

Go to <a href="https://www.splunk.com/?locale=en_us">Splunk</a> on your client machine (Make sure your AD VM is spun up or the internet won't work) 

When you reach <a href="https://www.splunk.com/?locale=en_us">Splunk</a> go to Products -> Free Trials and Downloads -> Universal Forwarder and select the appropriate system (My client machine is Windows 10 64 bit)

![image](https://github.com/user-attachments/assets/25b740b5-80bd-4049-a225-f8bec40fdc13)

When the download is complete go to the downloads directory in your client machine and double-click the Splunkforwarder download. 

Make sure the "An on-premises Splunk Enterprise instance" is checked.

![image](https://github.com/user-attachments/assets/dc1378dd-5f10-4ff2-8e42-28e5969c44e6)

Type a username and leave the "Generate random password" box checked

![image](https://github.com/user-attachments/assets/7e8fd979-020a-4392-9ff5-b62a47d1a8bf)

Click next on the Deployment Server 

For the Receiving Indexer put your Splunk VM IP address and the port is going to stay default (9997)

![image](https://github.com/user-attachments/assets/c4f58021-4586-405a-ab1d-9babab2167cb)

Install the Splunk universal forwarder.

Install <a href="https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon">Sysmon</a>. We will use Sysmon to collect data from our client machine and ingest data into Splunk. 

![image](https://github.com/user-attachments/assets/c9300638-d050-4d5a-bb90-6937b191470b)

Download the sysmonconfig.xml <a href="https://github.com/olafhartong/sysmon-modular">Sysmon Olaf Config</a>. 

![image](https://github.com/user-attachments/assets/0aabe178-9ee5-4cde-bb46-b8ac15faadc9)

Click "Raw" Right-click -> Save As to the downloads directory.

![image](https://github.com/user-attachments/assets/2c075a55-c6cb-4094-bd71-a406a06f415e)

Go to your downloads directory and extract the Sysmon contents. Once extracted copy the file location and open an admin PowerShell window.

On your PowerShell change to the directory by typing "cd and pasting the location"

![image](https://github.com/user-attachments/assets/a6a8bd80-a549-4dbc-9887-6e26cbc3e0d7)

Type ".\Sysmon64.exe -i ..\sysmonconfig.xml" Press enter. (-i flag states that you want to specify the config file ..\ is used to backtrack directories to where the sysmonfig.xml is located)

![image](https://github.com/user-attachments/assets/76807a97-b150-4ee3-b21b-33fa27983814)

*IMPORTANT* In order to instruct your Splunk forwarder on what is being sent to the Splunk server. Go to your client PC and go to Local Disk (C:) -> Program Files -> SplunkUniversalForwarder -> etc -> system -> local

![image](https://github.com/user-attachments/assets/df7fb514-a293-4b38-9a83-f72afd197c9a)

Run a Notepad as an administrator 

![image](https://github.com/user-attachments/assets/a104b9ea-c36d-4762-a53c-68f17f2e13d6)

Copy <a href="https://github.com/christianclark123/Splunk-Inputs.conf/blob/main/README.md">Splunk inputs.conf</a> into the notepad. This inputs.conf file instructs the forwarder to push events to the server relating to Application, Security, System, and Sysmon events. The index we will be using is endpoint. 

Save it to the directory up above (Local Disk (C:) -> Program Files -> SplunkUniversalForwarder -> etc -> system -> local). Change the file name to inputs.conf and change the Save as type: All Files

![image](https://github.com/user-attachments/assets/e074f79b-1469-460e-b35c-686941ef51f4)

In order for the changes to be made to the forwarder you have to look up the SplunkForwarder service by typing services (Run as Admin) in the windows search and restarting it. 

Once you find the service double-click it and go to the "Log On" tab and change to the Local System Account and hit apply.

![image](https://github.com/user-attachments/assets/86382c6f-7db8-4660-9b96-4bb056058fd1)

On the left underneath the name restart the SplunkForwarder service. If an error occurs hit ok and start the service again. 

![image](https://github.com/user-attachments/assets/ab37c4de-35a1-4324-97b3-5fc2d36a7713)

Log onto your Splunk server in the browser (Splunk IP address:8000)

![image](https://github.com/user-attachments/assets/0da0cec1-328f-4d83-81c1-72de102c7208)

Create the index=endpoint to receive data by going to Settings -> Indexes -> New Index at the top

![image](https://github.com/user-attachments/assets/08afef8a-22a6-440c-8489-909a2a85a922)
![image](https://github.com/user-attachments/assets/a0a4fa40-6d4f-4a39-9412-c7b09242ac9b)

Name the index endpoint. You should see it appear under your indexes

![image](https://github.com/user-attachments/assets/3ea62c07-37b6-48bd-8275-bab3de9a7da4)

Enable the Splunk server to receive data by going to Settings -> Forwarding and receiving -> Under receive data -> Configure receiving -> New receiving port -> 9997

![image](https://github.com/user-attachments/assets/594d07a0-ca7f-4ee5-86b8-2e504c7f856c)
![image](https://github.com/user-attachments/assets/e5186a71-8f39-497b-87ca-9a6ed90512c5)
![image](https://github.com/user-attachments/assets/ada9c6dc-45b6-4f65-9e3b-0557adba394f)

At the top of your Splunk server go to Apps -> Search & Reporting -> search index=endpoint and you should see your client PC under the Selected Fields Host

![image](https://github.com/user-attachments/assets/805c49d4-2e13-49c1-9452-1c4dbe940aaa)
![image](https://github.com/user-attachments/assets/6c6982a4-122e-46d6-add3-a462d166bd82)

If you click on the source and sourcetype you will see that Splunk is collecting data we set in the inputs.conf file (Security, Application, System, Sysmon)

![image](https://github.com/user-attachments/assets/d1f60832-2737-46ba-8ac9-91a855ca31d1)
