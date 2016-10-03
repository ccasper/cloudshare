# cloudshare
Securely share files and subversion with a cloud server or personal home computer ideal for small businesses.

This repository contains some tools that allow for securely sharing files and source code with other friends/colleagues in secure environment ideal for small businesses easily.

<H2>Server Setup</H2>
Linux is the platform of choice being free and available. Debian (recommended) or Ubuntu Server are excellent platforms for this.

- OpenSSH is included with linux. OpenSSH is one of the most secure methods for sharing and accessing information across the internet.
- RSSH is an excellent linux tool which restricts users to specific features, such as sftp, scp, svn, etc.
- CHROOT is a great way to limit users scope to the full linux environment, and limit file system access.
- SVN is a good code sharing repository for businesses that can't use more powerful features such as history rewrite that git allows
- FAIL2BAN protects from maliciously accessing your system by blocking IP addresses for a timeout after so many incorrect login attempts.
- Netbook (Such as Samsung N210) are excellent low power devices. You can run a full server for a few dozen or more users with file sharing using less than 8 Watts (saving significant electrial costs from high power servers)
- 
