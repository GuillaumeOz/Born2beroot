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

	$>su - Password:

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

`reboot` for changes to take effect, then log in and verify sudopowers via `sudo -v`.

## reboot







## Difference between APT and Aptitude

Besides the main difference being that Aptitude is a high level package manager while APT is a lower level package manager that can be used by other higher level package managers, the other main strengths that separate these two package managers are:

Aptitude is more feature-rich than apt-get and incorporates functionality from apt-get and its other variants, including apt-mark and apt-cache.
While apt-get handles all package installation, upgrading, system upgrading, package purging, dependency resolution, etc., Aptitude handles a lot more things than apt, including including apt-mark and apt-cache functionality, i.e. finding a package in the list of installed packages, marking a package to be installed automatically or manually, containing a package making it unavailable for upgrading, etc.

## What is SELinux ?

SELinux (Security-Enhanced Linux) is a security architecture for Linux® systems that allows administrators to better control access to the system. This architecture was originally designed by the NSA, the national security agency of the United States, as a series of fixes for the Linux kernel based on the LSM (Linux Security Modules) framework.

## What is AppArmor ?

AppArmor (Application Armor) est un logiciel de sécurité pour Linux édité sous Licence publique générale GNU.

AppArmor permet à l'administrateur système d'associer à chaque programme un profil de sécurité qui restreint ses accès au système d'exploitation. Il complète le traditionnel modèle d'Unix du contrôle d'accès discrétionnaire (DAC, Discretionary access control) en permettant d'utiliser le contrôle d'accès obligatoire (MAC, Mandatory access control).