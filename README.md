# cloudshare
Securely share files and subversion with a cloud server or personal home computer ideal for small businesses.

This repository contains some tools that allow for securely sharing files and source code with other friends/colleagues in secure environment ideal for small businesses easily.

## Tools Introduction
Linux is the platform of choice being free and available. Debian (recommended) (Tutorial uses Debian 8.6) or Ubuntu Server are excellent platforms for this.

- OpenSSH is included with linux. OpenSSH is one of the most secure methods for sharing and accessing information across the internet.
- CHROOT is a great way to limit users scope to the full linux environment, and limit file system access.
- RSSH is an excellent linux tool which restricts users to specific features, such as sftp, scp, svn, etc and gives us the ability to chroot the user to a special directory. OpenSSH has the ability to also chroot the user, but not limit specific features.
- SVN is a good code sharing repository for businesses that can't use more powerful features such as history rewrite that git allows
- FAIL2BAN protects from maliciously accessing your system by blocking IP addresses for a timeout after so many incorrect login attempts.
- BINDFS is extremely useful for mounting a directory elsewhere and allowing you to override user and group permissions. SSH cannot override permissions, only mask out permissions, which makes SFTP not very useful with many users. BINDFS gets around this by allowing us to force the file permissions.
- Netbook, such as <a href="http://www.pcworld.com/article/192795/samsung_N210_review.html">Samsung N210</a> or other Atom processor, are excellent low power devices. You can run a full server for several dozen users on approx 8 Watts (saving significant electrial costs compared to high power servers)
- RAM 1GB or more is recommended. For devices less than or equal to 4GB of ram, recommend using i386 build since x64 instructions are twice the size and take up a lot more ram.

## Server Setup
Install Debian 8.6. During installation uncheck options for desktop environment, and do check the option for ssh server.

Install custom packages
- apt-get install sudo
  - Add yourself to sudo group <code>useradd -G sudo <i>[username]</i></code>
- apt-get install openssh
  - In /etc/ssh/sshd_config, be sure to remember where Subystem sftp points to (/usr/lib/openssh/sftp-server)
- apt-get install fail2ban
- apt-get install rssh
  - Edit /etc/rssh.conf and uncomment allowscp and allowsftp to allow file sharing for select users.
- apt-get install cifs
- apt-get install bindfs
- apt-get install samba
- apt-get install ufw (or pgl)

### Create the chroot
Make a new directory and cd into this directory.
```bash
mkdir -p ./{dev,etc,lib,usr,bin,usr/bin}
mknod ./dev/null c 1 3
mknod ./dev/zero c 1 5
```
Copy over the apps and associated libraries.
```bash
APPS="/bin/bash /bin/ls /bin/mkdir /bin/mv /bin/pwd /bin/rm /usr/bin/id /usr/bin/ssh /bin/ping /usr/bin/dircolors"
for prog in $APPS;  do
        cp -L $prog ./$prog

        # obtain a list of related libraries
        ldd $prog > /dev/null
        if [ "$?" = 0 ] ; then
                LIBS=`ldd $prog | awk '{ print $3 }'`
                for l in $LIBS; do
                        mkdir -p ./`dirname $l` > /dev/null 2>&1
                        cp -L $l ./$l
                done
        fi 
done
```

Find libnss and libnsl libraries necessary for uid/gid conversion 
If you see "Unknown user 0" error, these libraries are missing.
```bash
find /lib -name libnss_compat.so.2
find /lib -name libnsl.so.1
find /lib -name libnss_files.so.2
```
Next copy the files listed from the above commands to the chroot and match the directory structure root.

Create groups script
```bash
echo '#!/bin/bash' > usr/bin/groups
echo "id -Gn" >> usr/bin/groups
```

Create passwd with root
```Bash
grep /etc/passwd -e "^root" > etc/passwd
```


