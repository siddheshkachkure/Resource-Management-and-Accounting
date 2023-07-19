# Open-PBS Configuration

`On all Nodes`

## Disable Firewalld

    systemctl stop firewalld
    systemctl disable firewalld
    systemctl status firewalld

## Disable SELinux:

    setenforce 0
    getenforce


## Set-up host config file /etc/hosts file 

    192.168.100.211 master master.cdac.in
    192.168.100.210 cn1 cn1.cdac.in
    192.168.100.197 cn2 cn2.cdac.in
    
## sync hosts files across nodes

    rsync /etc/hosts root@192.168.100.211:/etc/hosts
    rsync /etc/hosts root@192.168.100.197:/etc/hosts

` ON MASTER`

## Setup password-less SSH 

    ssh-keygen -t rsa

Now copy the key to other nodes

    ssh-copy-id root@node address

## Install GIT and clone openpbs repo (ON MASTER)

    yum install git -y

Clone OpenPBS repo from git hub

    git clone https://github.com/openpbs/openpbs.git

## install Development Tools on a CentOS / RHEL server (ON MASTER)

Type the following yum command as root user:

    yum group install "Development Tools"

If above command failed, try:

    yum groupinstall "Development Tools"

A note about failing groupinstall on CentOS/RHEL 
To install all the packages belonging to a package group called “Development Tools” use the following command:

     yum --setopt=group_package_types=mandatory,default,optional groupinstall "Development Tools"

OR

     yum --setopt=group_package_types=mandatory,default,optional group install "Development Tools"


-------------------------------------------------------------------------------------------------------------------------------
history

    15  yum --setopt=group_package_types=mandatory,default,optional groupinstall "Development Tools"
       16  yum group list
       17  ll
       18  cd Downloads/
       19  ll
       20  cd
       21  ll
       22  ll openpbs/
       23  clear
       24  ll
       25  ll openpbs/
       26  cd openpbs/
       27  rpmbuild -ba openpbs.spec
       28  cat openpbs.spec
       29  cd
       30  mv openpbs/ openpbs-23.06.06
       31  cd openpbs-23.06.06/
       32  rpmbuild -ba openpbs.spec
       33  cd
       34  tar -cvf /root/rpmbuild/SOURCES/openpbs-23.06.06.tar.gz openpbs-23.06.06/
       35  cd openpbs-23.06.06/
       36  ll
       37  rpmbuild -ba openpbs.spec
       38  history



[root@master openpbs-23.06.06]# yum install libtool-ltdl-devel hwloc-devel libXt-devel libedit-devel libical-devel ncurses-devel postgresql-devel postgresql-contrib python3-devel  tcl-devel tk-devel zlib-devel expat-devel openssl-devel  -y
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: centos.mirror.net.in
 * extras: centos.mirror.net.in
 * updates: centos.mirror.net.in
Package libtool-ltdl-devel-2.4.2-22.el7_3.x86_64 already installed and latest version
Package hwloc-devel-1.11.8-4.el7.x86_64 already installed and latest version
Package libXt-devel-1.1.5-3.el7.x86_64 already installed and latest version
Package libedit-devel-3.0-12.20121213cvs.el7.x86_64 already installed and latest version
Package libical-devel-3.0.3-2.el7.x86_64 already installed and latest version
Package ncurses-devel-5.9-14.20130511.el7_4.x86_64 already installed and latest version
Package postgresql-devel-9.2.24-8.el7_9.x86_64 already installed and latest version
Package postgresql-contrib-9.2.24-8.el7_9.x86_64 already installed and latest version
Package python3-devel-3.6.8-19.el7_9.x86_64 already installed and latest version
Package 1:tcl-devel-8.5.13-8.el7.x86_64 already installed and latest version
Package 1:tk-devel-8.5.13-6.el7.x86_64 already installed and latest version
Package zlib-devel-1.2.7-21.el7_9.x86_64 already installed and latest version
Package expat-devel-2.1.0-15.el7_9.x86_64 already installed and latest version
Package 1:openssl-devel-1.0.2k-26.el7_9.x86_64 already installed and latest version
Nothing to do

## Now buid the rpm, edit the config file of pbs

      cd openpbs-23.06.06/
      rpmbuild -ba openpbs.spec
      cd /root/rpmbuild/RPMS/x86_64/
      ls
      yum install openpbs-server-23.06.06-0.x86_64.rpm
      vim /etc/pbs.conf
            PBS_EXEC=/opt/pbs
            PBS_SERVER=master
            PBS_START_SERVER=1
            PBS_START_SCHED=1
            PBS_START_COMM=1
            PBS_START_MOM=1
            PBS_HOME=/var/spool/pbs
            PBS_CORE_LIMIT=unlimited
            PBS_SCP=/bin/scp
      chmod 4755 /opt/pbs/sbin/pbs_iff /opt/pbs/sbin/pbs_rcp
      systemctl status pbs
      systemctl start pbs
      systemctl status pbs
## 

    /etc/init.d/pbs status
    . /etc/profile.d/pbs.sh
    which qstat
    qstat -B
