# Born2beroot

This project aims to introduce you to the world of virtualization.

Welcome to Born2be"autoroot".
# 🛣️ 🏎️ 💨

Take your time for truly understand the purpose of each commands.

If this tutorial has helped you, feel free to leave a star ! 

## Final Grade 125/100

## Installation

The latest stable version of Debian is Debian 10 Buster. Download latest debian version on this [link](https://www.debian.org/).

Warning : Before watching the video, don't forget to setup your root password with secure politic (see Step 1: Setting Up a Strong Password Policy section).
Please visit this [link](https://www.youtube.com/watch?v=2w-2MX5QrQw&ab_channel=hanshazairi) for watching a Virtual Machine installation walkthrough.

Thanks to hbaddrul for this tutorial.

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

	line15 #Port 22

with:

	line15 Port 4242

To disable SSH login as root irregardless of authentication mechanism, replace below line

	line34 #PermitRootLogin prohibit-password

with:

	line34 PermitRootLogin no

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

### Step 3: Connecting to Server via SSH

SSH into your virtual machine using Port 4242 via `ssh <username>@<ip-address> -p 4242`.

	$>ssh <username>@<ip-address> -p 4242

Terminate SSH session at any time via `logout`.

	$>logout

Alternatively, terminate SSH session via `exit`.

	$>exit

## User Management

### Step 1: Setting Up a Strong Password Policy

#### Password Age

Configure password age policy via `sudo vi /etc/login.defs`.

	$>sudo vi /etc/login.defs

To set password to expire every 30 days, replace below line

	line160 PASS_MAX_DAYS   99999

with:

	line160 PASS_MAX_DAYS   30

To set minimum number of days between password changes to 2 days, replace below line

	line161 PASS_MIN_DAYS   0

with:

	line161 PASS_MIN_DAYS   2

To send user a warning message 7 days _(defaults to 7 anyway)_ before password expiry, keep below line as is.

	line162 PASS_WARN_AGE   7

#### Password Strength

Secondly, to set up policies in relation to password strength, install the _libpam-pwquality_ package.

	$>sudo apt install libpam-pwquality

Verify whether _libpam-pwquality_ was successfully installed via `dpkg -l | grep libpam-pwquality`.

	$>dpkg -l | grep libpam-pwquality

Configure password strength policy via `sudo vi /etc/pam.d/common-password`, specifically the below line:

	$>sudo vi /etc/pam.d/common-password
	<~~~>
	line25 password        requisite                       pam_pwquality.so retry=3
	<~~~>

To set password minimum length to 10 characters, add below option to the above line.

	minlen=10

To require password to contain at least an uppercase character and a numeric character:

	ucredit=-1 dcredit=-1

To set a maximum of 3 consecutive identical characters:

	maxrepeat=3

To reject the password if it contains `<username>` in some form:

	reject_username

To set the number of changes required in the new password from the old password to 7:

	difok=7

To implement the same policy on _root_:

	enforce_for_root

Finally, it should look like the below:

	password        requisite                       pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root

### Step 2: Creating a New Group

Create new _user42_ group via `sudo addgroup user42`.

	$>sudo addgroup user42

Add user to _user42_ group via `sudo adduser <username> user42`.

	$>sudo adduser <username> user42

Alternatively, add user to _user42_ group via `sudo usermod -aG user42 <username>`.

	$>sudo usermod -aG user42 <username>

Verify whether user was successfully added to _user42_ group via `getent group user42`.

	$>getent group user42

## cron

#### Setting Up a cron Job

#### For this part check the monitoring.sh file

Configure _cron_ as _root_ via `sudo crontab -u root -e`.

	$>sudo crontab -u root -e

To schedule a shell script to run every 10 minutes, replace below line

	line23 # m h  dom mon dow   command

with:

	line23 */10 * * * * bash /path/to/script | wall

Check root's scheduled cron jobs via `sudo crontab -u root -l`.

	$>sudo crontab -u root -l

# Bonus

## Linux Lighttpd MariaDB PHP (LLMP) Stack

### Step 1: Installing Lighttpd

Install _lighttpd_ via `sudo apt install lighttpd`.

	&>sudo apt install lighttpd

Verify whether _lighttpd_ was successfully installed via `dpkg -l | grep lighttpd`.

	$>dpkg -l | grep lighttpd

Allow incoming connections using Port 80 via `sudo ufw allow 80`.

	$>sudo ufw allow 80

### Step 2: Installing & Configuring MariaDB

Install _mariadb-server_ via `sudo apt install mariadb-server`.

	$>sudo apt install mariadb-server

Verify whether _mariadb-server_ was successfully installed via `dpkg -l | grep mariadb-server`.

	$>dpkg -l | grep mariadb-server

Start interactive script to remove insecure default settings via `sudo mysql_secure_installation`.

	$>sudo mysql_secure_installation
	Enter current password for root (enter for none): #Just press Enter (do not confuse database root with system root)
	Set root password? [Y/n] n
	Remove anonymous users? [Y/n] Y
	Disallow root login remotely? [Y/n] Y
	Remove test database and access to it? [Y/n] Y
	Reload privilege tables now? [Y/n] Y

Log in to the MariaDB console via `sudo mariadb`.

	$>sudo mariadb
	MariaDB [(none)]>

Create new database via `CREATE DATABASE <database-name>;`.

	MariaDB [(none)]> CREATE DATABASE <database-name>;

Create new database user and grant them full privileges on the newly-created database via `GRANT ALL ON <database-name>.* TO '<username-2>'@'localhost' IDENTIFIED BY '<password-2>' WITH GRANT OPTION;`.

	MariaDB [(none)]> GRANT ALL ON <database-name>.* TO '<username-2>'@'localhost' IDENTIFIED BY '<password-2>' WITH GRANT OPTION;

Flush the privileges via `FLUSH PRIVILEGES;`.

	MariaDB [(none)]> FLUSH PRIVILEGES;

Exit the MariaDB shell via `exit`.

	MariaDB [(none)]> exit

Verify whether database user was successfully created by logging in to the MariaDB console via `mariadb -u <username-2> -p`.

	$ mariadb -u <username-2> -p
	Enter password: <password-2>
	MariaDB [(none)]>

Confirm whether database user has access to the database via `SHOW DATABASES;`.

	MariaDB [(none)]> SHOW DATABASES;
	+--------------------+
	| Database           |
	+--------------------+
	| <database-name>    |
	| information_schema |
	+--------------------+

Exit the MariaDB shell via `exit`.

	MariaDB [(none)]> exit

### Step 3: Installing PHP

Install _php-cgi_ & _php-mysql_ via `sudo apt install php-cgi php-mysql`.

	$>sudo apt install php-cgi php-mysql

Verify whether _php-cgi_ & _php-mysql_ was successfully installed via `dpkg -l | grep php`.

	$>dpkg -l | grep php

### Step 4: Downloading & Configuring WordPress

Install _wget_ via `sudo apt install wget`.

	$>sudo apt install wget

Download WordPress to `/var/www/html` via `sudo wget http://wordpress.org/latest.tar.gz -P /var/www/html`.

	$>sudo wget http://wordpress.org/latest.tar.gz -P /var/www/html

Extract downloaded content via `sudo tar -xzvf /var/www/html/latest.tar.gz`.

	$>sudo tar -xzvf /var/www/html/latest.tar.gz

Remove tarball via `sudo rm /var/www/html/latest.tar.gz`.

	$>sudo rm /var/www/html/latest.tar.gz

Copy content of `/var/www/html/wordpress` to `/var/www/html` via `sudo cp -r /var/www/html/wordpress/* /var/www/html`.

	$>sudo cp -r /var/www/html/wordpress/* /var/www/html

Remove `/var/www/html/wordpress` via `sudo rm -rf /var/www/html/wordpress`

	$>sudo rm -rf /var/www/html/wordpress

Create WordPress configuration file from its sample via `sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php`.

	$>sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php

Configure WordPress to reference previously-created MariaDB database & user via `sudo vi /var/www/html/wp-config.php`.

	$>sudo vi /var/www/html/wp-config.php

Replace the below

	line23 define( 'DB_NAME', 'database_name_here' );
	line26 define( 'DB_USER', 'username_here' );
	line29 define( 'DB_PASSWORD', 'password_here' );

with:

	line23 define( 'DB_NAME', '<database-name>' );
	line26 define( 'DB_USER', '<username-2>' );
	line29 define( 'DB_PASSWORD', '<password-2>' );

### Step 5: Configuring Lighttpd

Enable below modules via `sudo lighty-enable-mod fastcgi; sudo lighty-enable-mod fastcgi-php; sudo service lighttpd force-reload`.

	$>sudo lighty-enable-mod fastcgi
	$>sudo lighty-enable-mod fastcgi-php
	$>sudo service lighttpd force-reload

## 3: File Transfer Protocol (FTP)

### Step 1: Installing & Configuring FTP

Install FTP server via `sudo apt install vsftpd`.

	$>sudo apt install vsftpd

Verify whether _vsftpd_ was successfully installed via `dpkg -l | grep vsftpd`.

	$>dpkg -l | grep vsftpd

Install FTP client via `sudo apt install ftp`.

	$>sudo apt install ftp

Verify whether _ftp_ was successfully installed via `dpkg -l | grep ftp`.

	$>dpkg -l | grep ftp

Allow incoming connections using Port 21 via `sudo ufw allow 21`.

	$>sudo ufw allow 21

Configure _vsftpd_ via `sudo vi /etc/vsftpd.conf`.

	$>sudo vi /etc/vsftpd.conf

To enable any form of FTP write command, uncomment below line:

	line31 #write_enable=YES

To set root folder for FTP-connected user to `/home/<username>/ftp`, add below lines:

	$>sudo mkdir /home/<username>/ftp
	$>sudo mkdir /home/<username>/ftp/files
	$>sudo chown nobody:nogroup /home/<username>/ftp
	$>sudo chmod a-w /home/<username>/ftp

Add these lines to the top of vsftpd.conf via `sudo vi /etc/vsftpd.conf`.

	<~~~>
	user_sub_token=$USER
	local_root=/home/$USER/ftp
	<~~~>

To prevent user from accessing files or using commands outside the directory tree, uncomment below line:

	line114 #chroot_local_user=YES

To whitelist FTP, add below lines:

	$>sudo vi /etc/vsftpd.userlist

To add your username, save and leave the file, and write the command below:

	$>echo <username> | sudo tee -a /etc/vsftpd.userlist

Add these lines to the file vsftpd.userlist via `sudo vi /etc/vsftpd.userlist`.

	<~~~>
	userlist_enable=YES
	userlist_file=/etc/vsftpd.userlist
	userlist_deny=NO
	<~~~>

### Step 2: Connecting to Server via FTP

FTP into your virtual machine via `ftp <ip-address>`.

	$>ftp <ip-address>

Terminate FTP session at any time via `CTRL + D`.

## Difference Between CentOS and Debian

CentOS vs Debian are two flavors of Linux operating systems. CentOS, as said above, is a Linux distribution. It is free and open-source. It is enterprise-class – industries can use meaning for server building; it is supported by a large community and is functionally supported by its upstream source, Red Hat Enterprise Linux. Debian is a Unix like computer operating system that is made up of open source components. It is built and supported by a group of individuals who are under the Debian project.

Debian uses Linux as its Kernel. Fedora, CentOS, Oracle Linux are all different distribution from Red Hat Linux and are variant of RedHat Linux. Ubuntu, Kali, etc., are variant of Debian. CentOS vs Debian both are used as internet servers or web servers like web, email, FTP, etc.

## Difference between APT and Aptitude

Besides the main difference being that Aptitude is a high level package manager while APT is a lower level package manager that can be used by other higher level package managers, the other main strengths that separate these two package managers are:

Aptitude is more feature-rich than apt-get and incorporates functionality from apt-get and its other variants, including apt-mark and apt-cache.
While apt-get handles all package installation, upgrading, system upgrading, package purging, dependency resolution, etc., Aptitude handles a lot more things than apt, including including apt-mark and apt-cache functionality, i.e. finding a package in the list of installed packages, marking a package to be installed automatically or manually, containing a package making it unavailable for upgrading, etc.

## What is SELinux ?

SELinux (Security-Enhanced Linux) is a security architecture for Linux® systems that allows administrators to better control access to the system. This architecture was originally designed by the NSA, the national security agency of the United States, as a series of fixes for the Linux kernel based on the LSM (Linux Security Modules) framework.

## What is AppArmor ?

AppArmor (Application Armor) is a security software for Linux systems made under GNU General Public License.

AppArmor allows the system admin to associate to each program a security profile which limits its' accesses to the operating system. It completes Unix's traditional way of Discretionary access control (DAC) by allowing the use of Mandatory access control (MAC).

### How to create a New User ?

Create new user via `sudo adduser <username>`.

	$>sudo adduser <username>

Verify whether user was successfully created via `getent passwd <username>`.

	$>getent passwd <username>

Verify newly-created user's password expiry information via `sudo chage -l <username>`.

	$ sudo chage -l <username>
	Last password change					: <last-password-change-date>
	Password expires					: <last-password-change-date + PASS_MAX_DAYS>
	Password inactive					: never
	Account expires						: never
	Minimum number of days between password change		: <PASS_MIN_DAYS>
	Maximum number of days between password change		: <PASS_MAX_DAYS>
	Number of days of warning before password expires	: <PASS_WARN_AGE>

At least for delete user via `sudo userdel <username>`.

	$>sudo userdel <username>
