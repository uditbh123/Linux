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