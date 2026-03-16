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
    - [monitoring.sh](#monitoringsh)
    - [Cron Configuration](#cron-configuration)
    - [Signature.txt](#signaturetxt)
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
    - Create a **new user account** (adrahoto) and set its **password*
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
5. **Installing SSH**

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

6. Configuring SSH

    After installing the SSH server, the next step is to configure it according to the project requirements.

    - The SSH server configuration is stored in: `/etc/ssh/sshd_config`
    - Since this is a system file, it requires root privileges to modify.First switch to the root user: `su`
    - Then open the configuration file using a text editor: `nano /etc/ssh/sshd_config`
    - Inside the configuration file, lines beginning with `#` are commented out. These lines must be uncommented and modified. By default, SSH runs on port 22. For this project it must be changed.
    - Find the line: `#Port 22`
    - Modify it to: `Port 4242`
    - Allowing direct root login is a security risk and must be disabled.
    - Find the line: `#PermitRootLogin prohibit-password`
    - Modify it to: `PermitRootLogin no`
    - Save the Changes
    - After modifying the configuration file, restart the SSH service so the changes take effect: `sudo service ssh restart`
    - Check that the SSH service is running correctly: `sudo service ssh status`
    - The output should show: `Active: active (running)`
    
    *Changing the port is basically moving your front door so the average burglar walks past it. Not perfect security, but it cuts down noise.*

7. Connecting via SSH

    To connect to the virtual machine from the host system using SSH, port forwarding must be configured in **VirtualBox**.

    **Configuring Port Forwarding**

    - Shut down the virtual machine.
    - Open **VirtualBox** and select the virtual machine.
    - Click **Settings**.
    - Navigate to **Network**.
    - Click **Advanced**.
    - Select Port **Forwarding**.

    **Add a new rule by clicking the add (+) icon.**

    - Add Host Port `4241`
    - Add Guest Port `4242`
    - The IP fields can be left empty.
    - After adding the rule, click `Accept` to save the changes.

    *This configuration forwards connections from the host machine (port 4241) to the virtual machine (port 4242).*

    *Sometimes connection conflicts occur if the host and guest ports are identical, so using different ports (4241 → 4242) can prevent this issue.*

    **Once the virtual machine is running, connect from the host machine using:**

    - `ssh <adrahoto>@localhost -p 4241`
    - You will be prompted to enter the user password. After entering it correctly, the SSH session will start and you will be connected to the Debian virtual machine.

    *localhost simply means your own computer.*

    *connect from my computer → back to my computer → forwarded into the VM.*

8. Installing and Configuring UFW 🔥🧱

    **UFW (Uncomplicated Firewall)** is a firewall management tool for Linux that provides a simple interface for configuring **iptables**, the underlying packet filtering system.

    It allows administrators to manage firewall rules using clear and straightforward commands.

    **Installing UFW**

    - First, install the UFW package using the Debian package manager: `sudo apt install ufw`
    - When prompted for confirmation, type: `y`
    - Press `Enter` to continue the installation.
    - After installation, activate the firewall with: `sudo ufw enable`

    Once enabled, the system should display a confirmation message indicating that the firewall is active.

    From this point forward, **UFW will control incoming and outgoing network traffic according to its configured rules.**

    *Right after enabling UFW, SSH can get blocked if you haven't allowed it first.*

    *Typical safe order on real servers is:*
    ```bash
    sudo ufw allow 4242
    sudo ufw enable
    ```
    *Otherwise people lock themselves out of their own machine.*

9. Allowing a Port Through the Firewall

    Since SSH has been configured to run on port 4242, the firewall must allow incoming connections on that port. Otherwise, any attempt to connect via SSH will be blocked.

    - To allow traffic on port 4242, run the following command: `sudo ufw allow 4242`

    *This command creates a firewall rule that permits incoming connections through port 4242.*

    - To check whether the rule has been applied correctly and to view the current firewall status, run: `sudo ufw status`
    - If the configuration was successful, the output should show a rule similar to:
    
    `4242/tcp                   ALLOW       Anywhere`

    *Changing the SSH port in sshd_config does not automatically open the port in the firewall.*

    *So two things must exist at the same time:*

    - *SSH listening on port 4242*
    - *Firewall allowing port 4242*

10. Configuring Sudo Policies

    To configure the required sudo security policies, a custom configuration file will be created inside: `/etc/sudoers.d/`

    Files in this directory allow administrators to define sudo rules without modifying the main /etc/sudoers file, which is safer and easier to manage.

    - First create a new configuration file: `touch /etc/sudoers.d/sudo_config`

    The project requires logging both the input and output of sudo commands.

    - For this purpose, create a dedicated directory for sudo logs: `sudo mkdir /var/log/sudo`

    We must edit the file that we created in the first step of this section.
    
    - Open the file using a text editor: `nano /etc/sudoers.d/sudo_config`

    - Add the following configuration:
    ```bash
    Defaults passwd_tries=3
    Defaults badpass_message="Custom error message"
    Defaults logfile="/var/log/sudo/sudo_config"
    Defaults log_input,log_output
    Defaults iolog_dir="/var/log/sudo"
    Defaults requiretty
    Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
    ```
    **Explanation of the Policies**
    | Policy | Description |
    |-|-|
    | `passwd_tries=3` | Limits the number of attempts a user has to enter the sudo password to three tries. |
    | `badpass_message="message"` | Displays a custom message when the user enters the wrong password. |
    | `logfile="/var/log/sudo/sudo_config"` | Specifies the log file where sudo activity will be recorded. |
    | `log_input, log_output` | Logs both the commands entered by the user and their output. |
    | `iolog_dir="/var/log/sudo"` | Defines the directory where input/output logs are stored. |
    | `requiretty` | Ensures that sudo commands can only be executed from a real terminal session (TTY). This prevents certain types of automated or background sudo executions. |
    | `secure_path="..."` | Defines a safe execution path for sudo commands. This prevents malicious programs from being executed through modified environment paths. |

    Files inside /etc/sudoers.d/ must have strict permissions, otherwise sudo will ignore them.

    - Set the correct permissions: `sudo chmod 440 /etc/sudoers.d/sudo_config`

    *The safest way to edit sudo configs is actually:*

    *`sudo visudo -f /etc/sudoers.d/sudo_config`*

    *visudo checks the syntax before saving. If you mess up normal sudoers syntax, sudo can stop working completely.*

11. **Password Policy 🔑**
    
    **Setting up basic password aging**

    - Edit login definitions: `nano /etc/login.defs`

    - Modify password parameters:
    ```bash
    PASS_MAX_DAYS 99999 → PASS_MAX_DAYS 30
    PASS_MIN_DAYS 0 → PASS_MIN_DAYS 2
    ```
    | Parameter | Description |
    |-|-|
    | PASS_MAX_DAYS | Maximum number of days before password expires |
    | PASS_MIN_DAYS | Minimum number of days before password can be changed |
    | PASS_WARN_AGE | Number of days before password expiration to show warning |

    - Install the password quality library: `sudo apt install libpam-pwquality`
    - Confirm with `Y` and wait for installation.
    - Edit PAM configuration (Pluggable Authentication Modules): `nano /etc/pam.d/common-password`
    - Find the line containing: `retry=3`
    - Add the following parameters after it: `minlen=10 ucredit=-1 dcredit=-1 lcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root`

    Example line: `password requisite pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 lcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root`

    | Rule | Meaning |
    |-|-|
    | `minlen=10` | Password must contain at least 10 characters |
    | `ucredit=-1` | Must contain at least one uppercase letter |
    | `dcredit=-1` | Must contain at least one digit |
    | `lcredit=-1` | Must contain at least one lowercase letter |
    | `maxrepeat=3` | A character cannot repeat more than three times consecutively |
    | `reject_username` | Password cannot contain the username |
    | `difok=7` | At least 7 characters must differ from the previous password |
    | `enforce_for_root` | Applies the password policy to root |

## monitoring.sh

This Bash script collects and displays key system information for the virtual machine. It reports the operating system architecture, CPU (physical and virtual cores), RAM and disk usage, CPU load, last reboot time, LVM status, active TCP connections, logged-in users, network details (IP and MAC), and the number of commands executed with `sudo`.

1. **Shebang**
    `#!/bin/bash`
    - This tells Linux to run this script using the Bash shell

2. **Variables**
    - Every line like this: `arch=$(uname -a)`
    - Means: Run a command. Save the output into a variable
    - `$()` = command substitution.

    The script collects information first, then prints everything at the end.

3. **Architecture**
    - `arch=$(uname -a)`
    - Shows kernel information: OS, kernel version, architecture, hostname
    - Example output: `Linux debian 5.10.0-amd64 x86_64 GNU/Linux`
    - The script stores it in `arch`.

4. **CPU Physical Cores**
    - `cpuf=$(grep "physical id" /proc/cpuinfo | wc -l)`
    - This is a virtual file created by the kernel with CPU info: `/proc/cpuinfo`
    - Find lines mentioning the physical CPU: `grep "physical id"`
    - Count lines: `wc -l`
    - Result = number of physical CPUs.

5. **Virtual CPUs**
    - `cpuv=$(grep "processor" /proc/cpuinfo | wc -l)`
    - Every processor thread appears as:
        ```bash
        processor : 0
        processor : 1
        processor : 2
        ```
    - Counting them gives the total threads / virtual cores.

6. **RAM**
    - `free --mega` Shows memory in MB.
    - Script extracts fields using `awk` - text processing language for the terminal. *Linux spits out tons of text, and awk lets you filter it, pick pieces of it, and do calculations on it.*

    - **Total RAM**
        - `ram_total=$(free --mega | awk '$1 == "Mem:" {print $2}')`
        - Field $2 = total memory.

    - **Used RAM**
        - `ram_use=$(free --mega | awk '$1 == "Mem:" {print $3}')`
        - Field $3 = used memory.

    - **RAM Percentage**
        - `ram_percent=$(free --mega | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')`
        - Formula: used / total * 100
        - Printed with 2 decimals.

7. **Disk Usage**
    - `df -m` - Shows disk usage in MB.
    - Then filters:
        - `grep "/dev/"` - Only real disks.
        - `grep -v "/boot"`- Exclude boot partition.
    - Then awk adds values together.

    - **Total Disk**
        - `disk_total=$(df -m | ... | awk '{disk_t += $2} END {printf ("%.1fGb\n"), disk_t/1024}')`
        - Adds total space $2 and converts MB → GB.

    - **Used Disk**
        - `disk_use=$(df -m | ... | awk '{disk_u += $3} END {print disk_u}')`
        - Adds used space $3.

    - **Disk Percentage**
        - `disk_percent=$(df -m | ... | awk '{disk_u += $3} {disk_t+= $2} END {printf("%d"), disk_u/disk_t*100}')`
        - Same math as RAM.

8. **CPU Load**
    - `vmstat 1 2` - sample every 1 second and do it 2 times
        - First line = average
        - Second line = current data.
    - `tail -1`- Take the last line.
    - `awk '{print $15}'` - Column 15 = idle CPU percentage.
    - The script calculates: `CPU usage = 100 - idle` Then formats to 1 decimal.

9. **Last Boot**
    - `who -b` - Shows last system boot.
    - Example: `system boot 2026-03-15 13:12`
    - awk extracts the date + time.

10. **LVM Check**
    - `lsblk` - Lists disks.
    - Example:
        ```bash
        sda
        └─sda1
        └─debian--vg-root (lvm)
        ```
    - Script checks if the word lvm exists.
        - If yes: echo yes
        - If not: echo no

11. **TCP Connections**
    - `ss -ta | grep ESTAB | wc -l`
    - ss = socket statistics.
        - `-t = TCP`
        - `-a = all`
    - Then count connections in ESTABLISHED state.

12. **Logged Users**
    - `users | wc -w`
    - `users` prints logged users (john maria root)
    - `wc -w` counts words.

13. **IP Address**
    - `hostname -I` - Returns local IP.
    - Example: `10.0.2.15`

14. **MAC Address**
    - `ip link | grep "link/ether" | awk '{print $2}'`
    - `ip link` shows interfaces.
        - Example: `link/ether 08:00:27:ab:cd:ef`
    - `awk` extracts the MAC.

15. **Sudo Commands**
    - `journalctl _COMM=sudo | grep COMMAND | wc -l`
    - `journalctl` reads system logs.
    - `_COMM=sudo` - Only sudo entries.
    - `grep COMMAND` - Counts only executed commands.

16. **Final Output**
    - Everything gets printed using: `wall`
    - `wall` = write to all users
        - It broadcasts the message to every logged-in user.

The script outputs something like:
```bash
Architecture: Linux debian ...
CPU physical: 2
vCPU: 4
Memory Usage: 450/2000MB (22.5%)
Disk Usage: 3/20Gb (15%)
CPU load: 3.2%
Last boot: 2026-03-15 14:11
LVM use: yes
Connections TCP: 1 ESTABLISHED
User log: 1
Network: IP 10.0.2.15 (08:00:27:ab:cd:ef)
Sudo: 12 cmd
```

## Cron Configuration
- **crontab** is a task scheduler in Linux that allows commands or scripts to run automatically at specified time intervals.

- To configure scheduled tasks for the system, we edit the root user's crontab file with: `sudo crontab -u root -e`
    - This opens the crontab configuration file in a text editor.

- Inside this file, we add a rule to execute our monitoring script every 10 minutes: `*/10 * * * * sh /path_to_script.sh`

**A cron schedule consists of five time fields:**

| Field | Meaning |
|-|-|
| `*/10` | Every 10 minutes |
| `*` | Every hour |
| `*` | Every day of the month |
| `*` | Every month |
| `*` | Every day of the week|

- This configuration ensures that the monitoring script runs automatically every 10 minutes and broadcasts the system information using the wall command.

## Signature.txt