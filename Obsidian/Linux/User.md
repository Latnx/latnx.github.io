### useradd
- 在 /etc/passwd 里面创建一行与帐号相关的数据，包括创建 UID/GID/主文件夹等；
- 在 /etc/shadow 里面将此帐号的密码相关参数填入，但是尚未有密码； 
- 在 /etc/group 里面加入一个与帐号名称一模一样的群组名称； 
- 在 /home 下面创建一个与帐号同名的目录作为使用者主文件夹，且权限为 700
### passwd 
### usermod

sudo
visudo
/etc/sudoers
su -