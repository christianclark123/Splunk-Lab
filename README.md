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

