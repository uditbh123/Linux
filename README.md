# Linux
# Account Setup
In the first step, I have created a Microsoft Azsure account using our university email address. 
Then, my microsoft azsure account is successfully created and I have recevied $100 worth of credit which helps me to purchase different types of useful learning environment. 

# Create Virtual Machine 
1. I have created a virtual machine where I can learn and do some projects for this course. 
2. Then, I named the machine "lab-robotics" and then I have select the no infrastructure redundancy required option and Trusted launch virtual machines security type.
3. I choosed Ubuntu Server 24.04 LTS - x64 Gen2 server and B series version 2 which is Standard_B21s_v2. 
4. After completing all the steps, I got a key and I have selected the connect option and choose the native ssh option.
5. There is a copy and execute SSH command button, I have pasted the key which I have got earlier.
6. Then open the windows powershell and paste the link generated link there and I reached in the Linux virtual machine.
7. It help us to use Linux in windows laptop without installing it. 

# Linking my GitHub account to my HAMK email
1. I created a new repository named "Linux" for the assignment.
2. Then I linked my HAMK email as a secondary mail for version control. 

# Screenshot 
![alt text](<Screenshot 2025-01-23 005738.png>)

# Assignment 3 User Management and File System access

1. Creating Users 
Tupu (Using)
To create the user tupu, use the following command:

        sudo adduser tupu
This will:
- Create a new user tupu.
- Automatically create the home directory /home/tupu.
- Set the default shell to /bin/bash.
- Prompt for password creation and additional user details.

Tupu (Using)
To create the user lupu, use the following command:

        sudo useradd -m -d /home/lupu -s /bin/bash -G lupu lupu
This will:
- Create the user lupu
- Set the home directory to /home/lupu
- Assign the default shell /bin/bash
- Add lupu to the lupu group

Set a password for lupu:

        sudo passwd lupu

Hupu (System User)
To create a system user hupu with restricted login, use:

        sudo useradd --system --shell /bin/false hupu
This ensures that hupu cannot log in interactively.

2. Granting Sudo Privilege 
Method 1: Using (Recommended)
Edit the sudoers file safely:

        sudo visudo 
Add the following lines at the end:

        tupu ALL=(ALL:ALL) ALL
        lupu ALL=(ALL:ALL) ALL
Save and exit.

Method 2: Adding Users to the Group
Alternatively, run:

        sudo usermod -aG sudo tupu
        sudo usermod -aG sudo lupu
Verify with:

        groups tupu
        groups lupu

3. Setting Up Shared directory
Step 1: Create the directory

        sudo mkdir /opt/projekti
Step 2: Create and Assign a Group

        sudo groupadd projekti
        sudo usermod -aG projekti tupu
        sudo usermod -aG projekti lupu
Step 3: Set Directory Ownership 

        sudo chown :projekti /opt/projekti
Step 4: Set Permissions 

        sudo chmod 770 /opt/projekti
This allows only tupu and lupu to access and modify files. 

Step 5: Ensure New Files Inherit Group Ownership

        sudo chmod g+s /opt/projekti
This ensures that new files in /opt/projekti inherit the projekti group.

4. Verification 
Check user groups:

        groups tupu
        groups lupu
Check directory permissions:

        ls -ld /opt/projekti
# Output

        drwxrws--- 2 root projekti 4096 Jan 30 18:37 /opt/projekti

# Screenshot 
![alt text](<Screenshot 2025-01-30 203724.png>)
![alt text](<Screenshot 2025-01-30 204041.png>)
![alt text](<Screenshot 2025-01-30 212143.png>)

# Assignment 6 APT

# APT Package Management Assignment 
# Part 1: Understanding APT and System Updates 

1. Check APT Version 
```
apt --version
```
## Output
apt 2.7.14 (amd64)

2. Update Package List 
```
sudo apt update 
```

## Why is this step important?
This command refreshes the local package index with the most recent updates from the repositories. It guarantees that your system is aware of the most recent packages and their versions available.

3. Upgrade Installed Packages
```
sudo apt upgrade -y
```

## Difference between update and upgrade:

- The update refreshes the available packages and their versions, but it neither installs nor upgrades any packages.
- upgrade updates to the latest versions of the packages you possess.

4. View Pending Updates 
```
apt list --upgradable
```
- [Listing... Done] This was the output for the code and there were no pending updates. 

# Part 2: Installing & Managing Packages

1. Search for a Package
```
apt search image editor
```
- So, I have selected the gimp package. 

2. View Package Details 
```
apt show gimp 
```
## Output 
- The output lists all dependencies required by the package.

## Dependencies Required 
```
libgimp2.0t64 (>= 2.10.36), libgimp2.0t64 (<= 2.10.36-z), gimp-data (>= 2.10.36), gimp-data (<= 2.10.36-z), graphviz, xdg-utils, libaa1 (>= 1.4p5), libbabl-0.1-0 (>= 1:0.1.78), libbz2-1.0, libc6 (>= 2.38), libcairo2 (>= 1.12.2), libfontconfig1 (>= 2.12.6), libfreetype6 (>= 2.2.1), libgcc-s1 (>= 3.3.1), libgdk-pixbuf-2.0-0 (>= 2.30.8), libgegl-0.4-0t64 (>= 1:0.4.38), libgexiv2-2 (>= 0.10.6), 
```

3. Install the Package
```
sudo apt install gimp -y
```
- Confirmation 
The package was installed successfully.

4. Check Installed Package Version 
```
apt list --installed | grep gimp 
```
## Output
gimp-data/noble-updates,now 2.10.36-3ubuntu0.24.04.1 all [installed,automatic]
gimp/noble-updates,now 2.10.36-3ubuntu0.24.04.1 amd64 [installed]
libgimp2.0t64/noble-updates,now 2.10.36-3ubuntu0.24.04.1 amd64 [installed,automatic]

# Part 3: Removing & Cleaning Packages 

1. Uninstall the Package 
```
sudo apt remove gimp -y
```
-Is the package fully removed?
- No, the configuration files are still left behind. 

2. Remove Configuration Files 
```
sudo apt purge gimp -y
```
-Difference between remove and purge:
- remove - removes the package but leaves configuration files.
- purge -  removes the package along with its configuration files.

3. Clear Unnecessary Dependencies
```
sudo apt autoremove -y
```
-Why is this step important?
- This command deletes packages that were automatically installed to fulfill dependencies for other packages and are currently unnecessary.

4. Clean Up Download Package Files
```
sudo apt clean
```
-What does this command do?
- This command removes the local repository of downloaded package files. It deletes all files located in the /var/cache/apt/archives/directory.

# Part 4: Managing Repositories & Troubleshooting

1. List All APT Repositories 
```
cat /etc/apt/sources.list 
```
-What do you notice in this file?
- This document includes the compilation of repositories from which your system retrieves packages.

2. Add a New Repository (Universe)
```
sudo add-apt-repository universe
sudo apt update
```
-What types of packages are found in the universe repository?
- The universe repository holds community-managed free and open-source software. 

3. Simulate an Installation Failure
```
sudo apt install fakepackage 
```
# Error Message:
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package fakepackage

- How would you troubleshout this issue?
- Ensure the package name is correct.
- Check if the repository containing the package is enabled.
- Update the package list using sudo apt update.

# Bonus Challenges

1. Hold a Package
```
sudo apt-mark hold gimp
```

2. Unhold a Package
```
sudo apt-mark unhold gimp
```
-Why would you want to hold a package?
- Keeping a package from being automatically upgraded is done by holding it. This is helpful if you wish to maintain a particular version of a package that you are aware functions well with your system.

# Screenshot 

## Figure 1: Updating Package List
![alt text](<Screenshot 2025-02-12 234459.png>)

## Figure 2: Installing the Package 
![alt text](<Screenshot 2025-02-12 234610.png>)

## Figure 3: Removing the package 
![alt text](<Screenshot 2025-02-12 234655.png>)

## Figure 4: Simulating Installation Failure 
![alt text](<Screenshot 2025-02-12 234813.png>)

# Assignment 7 - Virtualization

## Part 1: Introduction to Virtualization concepts



### Virtualization: 

Virtualization involves generating virtual representations of computing resources, including servers, storage, networks, and even complete operating systems. It enables several virtual environments to operate on one physical device, enhancing resource usage and adaptability. 

### Hypervisor: 

A hypervisor, known as a Virtual Machine Monitor (VMM), is a software that establishes and oversees virtual machines (VMs). It enables several VMs to utilize the hardware of one physical server while ensuring their isolation from one another. There exist two categories of hypervisors: 

Bare-Metal - Operates straight on the hardware (e.g., VMware ESXi, Microsoft Hyper-V, KVM). 

Hosted - Operates on an established OS, functioning as an application (e.g., VirtualBox, VMware Workstation). 

### Virtual Machines (VMs): 

A Virtual Machine (VM) is a software-driven simulation of a physical computer. Every VM operates its own operating system (guest OS) and applications, separate from other VMs on the same host. Main Characteristics of VMs: 

1. Contains a complete operating system with virtualized components. 

2. Operates above a hypervisor. 

3. Offers robust separation among various VMs. 

### Containers: 

A container is a portable, lightweight environment that encapsulates an application alongside its dependencies. In contrast to VMs, containers utilize the host operating system's kernel while maintaining application isolation at the process level. Main Characteristics of Containers: 

1. Quicker to initiate and terminate than virtual machines. 

2. Perfect for cloud-native applications and microservices. 

3. Examples: Docker, Kubernetes, LXC. 

### Difference Between Virtual Machines (VMs) and Containers: 

1. Architecture: 

- Virtual machines operate on a hypervisor and consist of a complete operating system, as well as virtualized hardware. 

- Containers utilize the host OS kernel and provide application isolation at the process level. 

2. Resource Usage: 

- VMs demand additional resources since each VM operates with its own operating system. 

- Containers are efficient since they utilize the host OS, which reduces resource consumption. 

3. Levels of Isolation: 

- VMs provide robust isolation since each operates independently. 

- Containers provide process-level isolation but share the host OS kernel, making them less isolated than VMs. 

## Part 2: Working with Multipass 

### Step 1: Install Multipass
Code

```
  sudo snap install multipass
```
![alt text](<Screenshot (9).png>)

```
  multipass version
```

## Step 2: Start Using Multipass

1. Booting an Ubuntu Virtual Machine 
Code 
```
  multipass launch
```
2. Listing All Running Instances 
Code 
``` 
multipass list 
```
3. View Instance Information
Code 
```
multipass info proper-boatbill
```
4. Accessing the Virtual Machine Shell 
```
multipass shell proper-boatbill
```
## Screenshot of multipass verison and 1 to 4
![alt text](<Screenshot 2025-02-21 133833.png>)

5. Running Commands on the Virtual Machine 
Code 
```
  multipass exec proper-boatbill -- ls -l /home
```
![alt text](<Screenshot 2025-02-21 133846.png>)
6. Stopping the Instance 
Code
```
  multipass stop proper-boatbill
```
7. Delete the Instance 
```
multipass delete proper-boatbill 
```
![alt text](<Screenshot 2025-02-21 133846-1.png>)
```
multipass purge
```
![alt text](<Screenshot 2025-02-20 235051-1.png>)

## Step 3: Using Cloud-init For Customization
- Cloud-init allows users to preconfigure the instance. 
1. Creating a file name cloud-init.yaml
```
nano cloud-init.yaml
```
2. Adding the following content
```
  #cloud-config
  users:
  - name: myuser
    groups: sudo
    shell: /bin/bash
  packages:
  - htop
  - curl
  ```
3. launching a new instance using this code
```
  multipass launch --cloud-init cloud-init.yaml
```
![alt text](<Screenshot 2025-02-21 134501.png>)

## Step 4: File Sharing
- To share files between host and the instance
1. Create a shared folder in your host machine:
```
  mkdir ~/shared-folder
```
2. Mount the folder inside Multipass
```
  multipass mount ~/shared-folder <instance-name>:/mnt/shared
```
3. Inside the instance, we can access /mnt/shared
```
  multipass shell <instance-name> ls /mnt/shared
```
![alt text](<Screenshot 2025-02-21 135033.png>)

## Part 3: Exploring LXD
- LXD is a tool that helps in managing containers and virtual machines on your linux platform. Consider it a "manager" for lightweight, isolated environments (containers) and complete virtual machines (VMs) operating on your computer. Main Characteristics of LXD:
1. Handles both containers and virtual machines.
2. Provide a unified API for handling instances.
3. Provides advanced capabilities such as snapshots, live migration, and resource limits.
4. Developed using LXC (Linux Containers) for enhanced performance and security.

1. Updating your system:
```
  sudo apt update
  sudo apt upgrade -y
```
2. Installing LXD:
```
  sudo snap install lxd
```
3. Verifying the Installation:
```
  lxd --version
```
Screenshot of 1 to 3
![alt text](<Screenshot 2025-02-21 135222.png>)
4. Creating a New Container and List it
```
  lxc launch ubuntu:24.04 <container-name>
  lxc list
```
5. Start and Stop the Container
```
  lxc stop <container-name>
  lxc list
```
6. Delete the container
```
  lxc delete <container-name>
  lxc list
```
Screenshot of 4 to 6
![alt text](<Screenshot 2025-02-21 135541.png>)

## Part 4: How to Stick Apps with Docker 
1. Install Docker Engine
```
  sudo apt update
  sudo apt install docker-ce docker-ce-cli containerd.io -y
```
2. Verifying Installation, Checking Docker Version
```
  docker --version
```
![alt text](<Screenshot 2025-02-22 165949.png>)

3. Making directory and Creating nano file in docker 
```
  mkdir myapp
  cd myapp
  echo "hello world" > index.html
```
![alt text](<Screenshot 2025-02-21 141202.png>)

4. Building Docker 
```
  sudo docker build -t my-nginx
  sudo docker ps
```
![alt text](<Screenshot 2025-02-21 141353.png>)
![alt text](<Screenshot 2025-02-21 141516.png>)

## Part 5: Snaps for Self-Contained Applications
1.  Installing Snapd
```
  sudo apt install snapd
```
2.  Installing Snapcraft
```
  sudo snap install snapcraft --classic
```
3. Verifying Installation
```
  snapcraft --version
```
![alt text](<Screenshot 2025-02-21 141706.png>)

## Simple Snapcraft Application
- Now we will create a simple snap for a basic application. For example, we will use a "Hellp Snap" Python script.

1. Setting Up the Project Directory

```
mkdir my-snapcraft  
cd my-snapcraft  
```
2. Creating the Application Script 
```
mkdir bin  
nano bin/hello-snap  
```
3. Paste the following content into hello-snap
```
#!/bin/bash
echo "Hello, Snap!"
```
4. Making the Script Executable
```
chmod +x bin/hello-snap  
```
![alt text](<Screenshot 2025-02-21 141920.png>)

5. Defining the Snapcraft Configuration
```
nano snapcraft.yaml
```
6. Write the following code for configuration
```
name: my-snapcraft
base: core22
version: "1.0"
summary: "A simple Snapcraft app"
description: "This is a simple Snap application that prints Hello, Snap!"

grade: stable
confinement: strict

apps:
hello:
command: bin/hello-snap

parts:
hello:
plugin: dump
source: .
```
![alt text](<Screenshot 2025-02-21 141834.png>)

7. Building the snap package
```
snapcraft
```
8. Installing and running my snapcraft
```
sudo snap install --devmode my-snap-app_0.1_amd64.snap
```
9. Now, running my snap:
```
snap run my-snap-app.myscript
```
Now, we can see the output:
```
Hello from myscript.py!
```
![alt text](<Screenshot 2025-02-23 194954.png>)

# Firewall Configuration
1. Introduction 
This report details the firewall setup for protecting a server by permitting essential services, preventing unauthorized access, and reducing typical network threats. The firewall regulations are intended to: 
- Allow essential services (OpenSSH, HTTP, and HTTPS)
- Log blocked and allowed connections for monitoring
- Mitigate SYN Flood and other common network attacks

2. Firewall Rules and Explanation
2.1 Allow OpenSSH (Port 22)
```
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
- Purpose: Allows SSH connections for remote administration
- Why? SSH is required for secure remote access. The rule ensure only new and established connections are allowed.

2.2 Allow HTTP and HTTPS (Ports 80 & 443)
```
sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 80 -m conntrack --ctstate ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 443 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```
- Purpose: Allows web traffic over HTTP and HTTPS
- Why? Necessary for hosting web services while ensuring communication over HTTPS

2.3 Log Blocked connections
```
sudo iptables -A INPUT -j LOG --log-prefix "Blocked Connection: "
sudo iptables -A INPUT -j DROP
```
- Purpose: Logs and drops all unauthorized access attempts
- Why? Helps monitor and analyze potential attack attempts

2.4 SYN Flood Protection
```
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
```
- Purpose: Prevents SYN flood attacks by limiting the rate of new TCP connections.
- Why? Prevents attackers from overwhelming the server with excessive connection requests

2.5 Brute Force Proctection for SSH
```
sudo iptables -A INPUT -p tcp --dport 22 -m recent --set
sudo iptables -A INPUT -p tcp --dport 22 -m recent --update --seconds 60 --hitcount 3 -j REJECT --reject-with tcp-reset
```
- Reasoning: Prevent brute force SSH attacks by limiting the number of login attempts.

2.6 Prevent Ping Flood (ICMP Attack)
```
sudo iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s -j ACCEPT
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```
- Purpose: Limits the reate of ICMP echo requests (ping) to prevent Denial-of-Service (Dos) attacks.
- Why? Prevents attackers from overloading the network with excessive ping requests

2.7 Block Invalid Packets 
```
sudo iptables -A INPUT -m state --state INVALID -j DROP
```
- Reasoning: Block packets that are considered invalid, reducing the attack surface.

3.8 Log All Blocked Connections
```
sudo iptables -A INPUT -j LOG --log-prefix "BLOCKED_TRAFFIC: " --log-level 4
```

3. Conclusion 
This firewall setup guarantees that only essential services are reachable while preventing unauthorized access. It also offers safeguards against prevalent attacks such as SYN flood and ICMP flood. The logging system aids in efficiently tracking security incidents. 



