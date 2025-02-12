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

# Assignment 3 APT

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