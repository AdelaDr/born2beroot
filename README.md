*This project has been created as part of the 42 curriculum by adrahoto.*

[![C](https://img.shields.io/badge/language-C-555?style=flat&logo=c)](https://en.wikipedia.org/wiki/C_(programming_language))
[![42 Project](https://img.shields.io/badge/42-Prague-blue)](https://www.42prague.com/)

# Table of Contents
- [Description](#description)
    - [Installing VirtualBox](#installing-virtualbox)
    - [Choosing the Operating System](#choosing-the-operating-system)
    - [Creating a New Virtual Machine](#creating-a-new-virtual-machine)
    - [Attaching the Debian ISO to the Virtual Machine](#attaching-the-debian-iso-to-the-virtual-machine)
    - [Installing Debian](#installing-debian)
    - [Virtual Machine Setup](#virtual-machine-setup)
  

- [Instructions](#instructions)
- [Resources](#resources)
- [Project description](#project-description)

# Description
Born2beRoot is a system administration project focused on setting up and securing a Linux server from scratch inside a virtual machine. The goal is to build a minimal, secure, and well-configured server environment using either Debian or Rocky Linux, following strict security and configuration rules.

### This project introduces fundamental concepts of system administration, including:

- Virtualization and virtual machine setup

- Disk partitioning and Logical Volume Management (LVM)

- User and group management

- Sudo configuration and privilege control

- SSH service configuration and security

- Firewall configuration

- Password policy enforcement

- System monitoring via custom Bash scripting

The server must be configured without a graphical interface, emphasizing command-line proficiency and a deep understanding of how Linux systems operate at a low level.

Born2beRoot is designed to develop practical skills in system hardening, security best practices, and shell scripting, forming a foundation for more advanced DevOps and infrastructure work.

## Installing VirtualBox

Before setting up the server, a virtualization tool is required. If VirtualBox is not already installed on your system, it must be downloaded and installed.

VirtualBox is a virtualization software that allows you to create and run virtual machines (VMs). A virtual machine simulates a physical computer, enabling you to install and configure an operating system (such as Debian) without affecting your main system.

### To install VirtualBox:

1. Visit the official website: https://www.virtualbox.org

2. Download the version compatible with your operating system (Windows, macOS, or Linux).

3. Run the installer and follow the installation instructions.

After installation, verify that VirtualBox launches correctly and is ready to create a new virtual machine.

## Choosing the Operating System

For this project, `Debian` was selected as the operating system instead of Rocky Linux.

### Debian is known for its:

- Stability and reliability

- Large and well-maintained package repositories

- Strong security updates

- Extensive documentation

- Minimal default installation

Debian is widely used in server environments because it prioritizes stability over cutting-edge features. This makes it an excellent choice for a secure and controlled server setup like Born2beRoot.

It also has a large community, which makes troubleshooting and learning easier.

### Debian vs Rocky Linux

Rocky Linux is a community-driven enterprise Linux distribution designed to be binary-compatible with Red Hat Enterprise Linux (RHEL). It focuses on enterprise-level stability and long-term support.

| Debian | Rocky Linux |
|--------|--------------|
| Community-driven project | RHEL-compatible enterprise distribution |
| Uses `apt` package manager | Uses `dnf` package manager |
| More common in general server and hosting environments | More common in enterprise and corporate environments |
| Very large package ecosystem | Enterprise-focused ecosystem |

<br>

Both systems are stable and secure. However, Debian was chosen for its simplicity, documentation quality, and widespread use in general server environments, making it well-suited for this project.

## Creating a New Virtual Machine

Once VirtualBox is installed, the next step is to create a new virtual machine (VM) to host your server.

1. Open VirtualBox and click `New`.

2. Enter a **name** for the VM and select the folder where it will be stored (**sgoinfre** folder on campus).

3. Select the type as `Linux` and the version as `Oracle (64-bit)`.

4. Allocate **memory (RAM)**. For this project, 1–2 GB is sufficient.

5. Create a **virtual hard disk**. Use **VDI** format and **dynamically allocated** storage.

6. Set the disk size (12 GB). This will provide enough space for installation, configuration, and exercises.

7. Review settings and click `Create`.

The VM is now ready to install the operating system and begin the Born2beRoot exercises.

## Attaching the Debian ISO to the Virtual Machine

Before installing the operating system, the Debian installation image (ISO file) must be attached to the virtual machine. This allows the VM to boot from the installer.

### Steps

1. Open **VirtualBox**.

2. Select the created virtual machine.

3. Click **Settings**.

4. Navigate to **Storage**.

5. Under **Controller: IDE**, select the Empty optical drive.

6. Click the **disk icon** on the right side.

7. Select **Choose a disk file**.

8. Locate and select the downloaded **Debian ISO** file.

After attaching the ISO file, the virtual machine will be able to boot into the Debian installer when it starts.

## Installing Debian

After attaching the Debian ISO file, the virtual machine can be started to begin the operating system installation.

1. Start the virtual machine in **VirtualBox**.

2. The system will boot from the attached **Debian ISO**.

3. Select **Install** from the boot menu.

4. Follow the installation prompts:

    - Choose your **language** (English)

    - Select your **location** (other - Europe - Czechia)

    - Configure your **locales** (United states)

    - Configure your **keyboard layout** (American English)

5. Configure the **network settings**:

    - Set a **hostname** for the server (adrahoto42)

    - Leave the **domain name** empty since it is not required

6. Create **users and passwords**:

    - Set the **root password**

    - Create a **new user account** (adrahoto) and set its **password**

7. Select your time zone  

8. **Disk Partitioning**:

    - Select `Guided – use entire disk and set up encrypted LVM`

    - Choose the virtual disk

    - Select `Separate /home partition`.

    - Select `Yes` so the changes will be writen in the disk and so we can set the logical volume manager (LVM).

    - Click on `Cancel`, the erasing of the data is not required.

    - Set an **encryption passphrase**

    - Enter the Amount of volume group for guided parttioning (max -12.4 GB)
    
    - Click on `Finish partitioning and write changes to disk`.

    - Click `Yes` to confirm that we do not want more changes.

9. Package Manager Configuration

    - When asked to **scan additional installation media**, select `No` since no extra packages are required.

    - Choose your **country** for the Debian mirror.

    - Select `deb.debian.org`, which is the recommended official Debian mirror.

    - Leave the **HTTP proxy** field blank and `Continue`.

    - When asked about **participating in the package usage survey**, select `No` if you prefer not to send statistics.

    - Leave blank all software choises and click on `Continue`.

10. Installing the GRUB Boot Loader
    
    GRUB (Grand Unified Bootloader) is the standard bootloader used by most Linux systems. It allows the computer to load the operating system kernel and also provides the ability to choose between multiple operating systems or kernel versions if they are installed.
    
    - When asked **“Install the GRUB boot loader to the hard disk?”**, select `Yes`.

    - Choose the device where GRUB should be installed. Select the virtual disk: `/dev/sda (ata-VBOX_HARDDISK)`
    
    - `Continue` the installation process.
    
    *Why do we install GRUB?*

    *Because the computer does not magically know which OS to start.*
    *GRUB is the tiny program that runs before the operating system and loads the Linux kernel into memory.*

    **No GRUB → no boot → very sad server :(**

11. Finish installation

    - To finish the installation we click on `Continue`.

## Virtual Machine Setup

1. First Connection

    **Booting into the Virtual Machine**

    - Select the boot option. When the GRUB menu appears, select: `Debian GNU/Linux`

    - Enter the encryption password

        Because the disk was configured with encrypted LVM, the system will prompt for the encryption password created during installation.

    - User login - Enter the **username** created during installation with the corresponding **password**

    After successfully logging in, you will gain access to the Debian system through the command line interface.

    *Because the disk uses LUKS encryption, the system cannot access any data until the passphrase is entered during boot.*

    *Meaning if someone steals the VM disk file, they just get encrypted noise instead of THE system.*

2. Installing sudo and Configuring Users

    sudo is a program used in Unix-like operating systems that allows a permitted user to execute commands with the privileges of another user, typically the superuser (root).

    The name originally stood for “superuser do”, since it allowed users to execute administrative commands without logging in directly as the root user.

    Using sudo improves security because administrative privileges can be granted selectively instead of giving full root access to every user.

    **Installing sudo**

    - Switch to the root user: `su`

    - Enter the **root password** created during the Debian installation.

    - Install the sudo package: `apt install sudo`
    *The package manager will download and install the required files.*
    - Press `y` when prompted to confirm the installation.

    - Reboot the system: `sudo reboot`

    - Verify the installation. Once the system restarts: Enter the disk encryption password. Log in with your user account.

    - Switch to the root user again: `su`

    - Then check that sudo was installed correctly: `sudo -V`

    If the installation was successful, this command will display the sudo version and configuration information.

    *Why do we use sudo instead of logging in as root all the time?*
        - *it limits privilege escalation*
        - *it logs administrative actions*
        - *it reduces the risk of accidental system damage*

3. Creating a user

    - if a user was not added during th installation of the system, we add one `sudo adduser <login>`

4. Creating a group 

    - We create a new group called **user42**: `sudo addgroup user42`

    - It will display **GID** of the group == **Group ID**
   
    - We can check th group by `getent group user42` (get entries)

        - It queries the system’s **Name Service Switch (NSS)** databases. They include things like: users, groups, hosts, services.

        - Linux checks wherever group information is stored(and prints the result): 
            - `/etc/group` (This is the local file on your machine that stores group informatio. Example : sudo:x:27:user1,user2 == group name:password placeholder:GID:users)
            - **LDAP (Lightweight Directory Access Protocol)** - Centralized user database for many machines. Instead of creating users on every machine, they store them in one central LDAP server.
            - **NIS (Network Information Service)** - An older system for sharing user and group data across multiple machines. Older and less secure than LDAP.
            - other configured sources (Active Directory (very common in companies), SSSD, Kerberos, custom authentication systems)
5. **Installing & configuring SSH**

    *SSH (Secure Shell) is a network protocol that allows secure remote access to a system. It was designed as a secure replacement for older remote shell protocols.*

    *SSH uses a client–server model, where a client connects to a remote server through an encrypted communication channel, ensuring that commands and data cannot be intercepted.*

    *This allows administrators to manage servers remotely through the command line.*
    
    - Before installing new packages, it is good practice to update the package lists: `sudo apt update`
    - To enable SSH access on the system, install the OpenSSH server package: `sudo apt install openssh-server`
    - When prompted, type: `y` and press Enter to confirm the installation.
    - After installation, verify that the SSH service is running: `sudo service ssh status`
    - If the installation was successful, the output should indicate: `Active: active (running)`
    
    *What is the difference between SSH client and SSH server?*
        
    *Server → waits for incoming connections (Debian VM)*
        
    *Client → initiates the connection (host machine)*
