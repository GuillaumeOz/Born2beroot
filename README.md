# Born2beroot

This project aims to introduce you to the world of virtualization.

Take your time for truly understand the purpose of each commands.

Thanks to hbaddrul for this tutorial.

## Installation

The latest stable version of Debian is Debian 10 Buster. Download latest debian version on this [link](https://www.debian.org/).

Please visit this [link](https://www.youtube.com/watch?v=2w-2MX5QrQw&ab_channel=hanshazairi) for watching a Virtual Machine installation walkthrough.

## sudo

### Step 1: Installing sudo

Switch to root and its environment via : `su -`.

	$>su -
	$>Password:

It is advisable to install _vim_ for this tutorial, for commands that will use _vi_, use _vim_ instead.

	$>apt-get update
	$>apt-get install vim

Install _sudo_ via `apt install sudo`.

	$>apt install sudo

Verify whether _sudo_ was successfully installed via `dpkg -l | grep sudo`.

	$>dpkg -l | grep sudo

### Step 2: Adding User to _sudo_ Group

Add user to _sudo_ group via `adduser <username> sudo`.

	$>adduser <username> sudo

Alternatively, add user to sudo group via `usermod -aG sudo <username>`.

	$>usermod -aG sudo <username>

Verify whether user was successfully added to sudo group via `getent group sudo`.

	$>getent group sudo

`reboot` for changes to take effect, then log in and verify `sudopowers` via `sudo -v`.

	$>reboot

	<--->
	Debian GNU/Linux 10 <hostname> tty1

	$><hostname> login: <username>
	$>Password: <password>
	<--->

	$>sudo -v
	$>[sudo] password for <username>: <password>

### Step 3: Running root-Privileged Commands

From here on out, run root-privileged commands via prefix `sudo`. For instance:

	$>sudo apt update

### Step 4: Configuring sudo

Configure _sudo_ via `sudo vi /etc/sudoers.d/<filename>`. `<filename>` shall not end in `~` or contain `.`.

	$>sudo vi /etc/sudoers.d/<filename>

To limit authentication using sudo to 3 attempts (defaults to 3 anyway) in the event of an incorrect password, add below line to the file.

	Defaults        passwd_tries=3

To add a custom error message in the event of an incorrect password:

	Defaults        badpass_message="<custom-error-message>"

To log all _sudo_ commands to `/var/log/sudo/<filename>`:

	$>sudo mkdir /var/log/sudo
	<~~~>
	Defaults        logfile="/var/log/sudo/<filename>"
	<~~~>

To archive all _sudo_ inputs & outputs to `/var/log/sudo/`:

	Defaults        log_input,log_output
	Defaults        iolog_dir="/var/log/sudo"

To require TTY:

	Defaults        requiretty

To set _sudo_ paths to `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin`:

	Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

## SSH

### Step 1: Installing & Configuring SSH

Install _openssh-server_ via `sudo apt install openssh-server`.

	$>sudo apt install openssh-server

Verify whether openssh-server was successfully installed via `dpkg -l | grep ssh`.

	$>dpkg -l | grep ssh

Configure SSH via `sudo vi /etc/ssh/sshd_config`.

	$>sudo vi /etc/ssh/sshd_config

To set up SSH using Port 4242, replace below line:

	line13 #Port 22

with:

	line13 Port 4242

To disable SSH login as root irregardless of authentication mechanism, replace below line

	line32 #PermitRootLogin prohibit-password

with:

	line32 PermitRootLogin no

Check SSH status via `sudo service ssh status`.

	$>sudo service ssh status

Alternatively, check SSH status via `systemctl status ssh`.

	$>systemctl status ssh

### Step 2: Installing & Configuring UFW

Install _ufw_ via `sudo apt install ufw`.

	$>sudo apt install ufw

Verify whether ufw was successfully installed via `dpkg -l | grep ufw`.

	$>dpkg -l | grep ufw

Enable Firewall via `sudo ufw enable`.

	$>sudo ufw enable

Allow incoming connections using Port 4242 via `sudo ufw allow 4242`.

	$>sudo ufw allow 4242

Check UFW status via `sudo ufw status`.

	$>sudo ufw status

### XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

### Step 3: Connecting to Server via SSH

SSH into your virtual machine using Port 4242 via `ssh <username>@<ip-address> -p 4242`.

	$>ssh <username>@<ip-address> -p 4242

Terminate SSH session at any time via `logout`.

	$>logout

Alternatively, terminate SSH session via `exit`.

	$>exit

## User Management













## Difference between APT and Aptitude

Besides the main difference being that Aptitude is a high level package manager while APT is a lower level package manager that can be used by other higher level package managers, the other main strengths that separate these two package managers are:

Aptitude is more feature-rich than apt-get and incorporates functionality from apt-get and its other variants, including apt-mark and apt-cache.
While apt-get handles all package installation, upgrading, system upgrading, package purging, dependency resolution, etc., Aptitude handles a lot more things than apt, including including apt-mark and apt-cache functionality, i.e. finding a package in the list of installed packages, marking a package to be installed automatically or manually, containing a package making it unavailable for upgrading, etc.

## What is SELinux ?

SELinux (Security-Enhanced Linux) is a security architecture for Linux® systems that allows administrators to better control access to the system. This architecture was originally designed by the NSA, the national security agency of the United States, as a series of fixes for the Linux kernel based on the LSM (Linux Security Modules) framework.

## What is AppArmor ?

AppArmor (Application Armor) est un logiciel de sécurité pour Linux édité sous Licence publique générale GNU.

AppArmor permet à l'administrateur système d'associer à chaque programme un profil de sécurité qui restreint ses accès au système d'exploitation. Il complète le traditionnel modèle d'Unix du contrôle d'accès discrétionnaire (DAC, Discretionary access control) en permettant d'utiliser le contrôle d'accès obligatoire (MAC, Mandatory access control).