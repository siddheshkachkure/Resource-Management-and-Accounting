
# Open-PBS Configuration

`Make 3 VMs master node1 node2`

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

## now install all dependencies required

    yum install libtool-ltdl-devel hwloc-devel libXt-devel libedit-devel libical-devel ncurses-devel postgresql-devel postgresql-contrib python3-devel  tcl-devel tk-devel zlib-devel expat-devel openssl-devel  -y
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
## Now On node1

### Install git

    yum install git -y

### Clone the git openpbs repository and do futher commands given below

    git clone https://github.com/openpbs/openpbs.git
    cd openpbs/
    ./autogen.sh
    yum provides */autoreconf -y
    ./configure
    yum install openssl* libXt-devel -y
    mkdir -p /opt/pbs/lib
    yum install libtool-ltdl-devel hwloc-devel libXt-devel libedit-devel libical-devel ncurses-devel postgresql-devel postgresql-contrib python3-devel tcl-devel tk-devel zlib-devel expat-devel openssl-devel -y Loaded plugins: fastestmirror, langpacks Loading mirror speeds from cached hostfile
    ./configure --prefix=/opt/pbs
    make
    make install
    . /etc/profile.d/pbs.sh
    which qstat
    chmod  +x /opt/pbs/etc/pbs.sh
    bash /opt/pbs/etc/pbs.sh
    which qstat
    export PATH=${PATH}:/opt/pbs/bin
    vim /etc/pbs.conf
            PBS_EXEC=/opt/pbs
            PBS_SERVER=master
            PBS_START_SERVER=0
            PBS_START_SCHED=0
            PBS_START_COMM=0
            PBS_START_MOM=1
            PBS_HOME=/var/spool/pbs
            PBS_CORE_LIMIT=unlimited
            PBS_SCP=/bin/scp
    which qstat

### Now Configure the nfs on ```master``` and ```node3``` to mount execution 

#### On master

        yum install -y nfs-utils
        systemctl start nfs-server rpcbind
        systemctl enable nfs-server rpcbind

#### Check NFS Share

Before mounting the NFS share,
I request you to check the NFS shares available on the NFS server by running the following command on the NFS client.
    
    showmount -e master

#### Mount NFS Share

Now, create a directory on NFS client `node2` to mount the NFS share /home which we have created in the NFS server `master`

    mkdir /mnt/home

Use below command to mount a NFS share /home from NFS server 192.168.100.186 in /mnt/nfsfileshare on NFS client `master`

    mount -t master:/home /mnt/home

Verify the mounted share on the NFS client `node2` using mount command.

    mount | grep nfs

Also, you can use the df -hT command to check the mounted NFS share.

    df -hT

Now edit the following conf file on both the nodes `node1` `node2`

    vim /etc/pbs.conf
                PBS_EXEC=/opt/pbs
                PBS_SERVER=master
                PBS_START_SERVER=0
                PBS_START_SCHED=0
                PBS_START_COMM=0
                PBS_START_MOM=1
                PBS_HOME=/var/spool/pbs
                PBS_CORE_LIMIT=unlimited
                PBS_SCP=/bin/scp

Now copy the rpm files from master to node2

    cd /root/rpmbuild/RPMS/x86_64/
    cp * /home/RPMS/

Now install openpbs execution rpm files  on `node2` and make file in `/var/spool/pbs/mom_priv/config` 

    yum install openpbs-execution-23.06.06-0.x86_64.rpm
    vim /var/spool/pbs/mom_priv/config 
                     
                     $logevent 0x1ff
                     #$clientname node2
                     $restrict_user_maxsysid 999

Now on `node1`

    vim /var/spool/pbs/mom_priv/config 
                     $logevent 0x1ff
                     #$clientname node1
                     $restrict_user_maxsysid 999
                     
Now in `master` 
            
    vim /var/spool/pbs/server_priv/nodes
            node1 np=1
            node2 np=1
Now create

    qmgr
        Max open servers: 49
        Qmgr: create node node1
        Qmgr: create node node2
        Qmgr: 
    pbsnodes -a

You will get like this output 

                    master
                         Mom = master
                         Port = 15002
                         pbs_version = 23.06.06
                         ntype = PBS
                         state = free
                         pcpus = 2
                         resources_available.arch = linux
                         resources_available.host = master
                         resources_available.mem = 4811568kb
                         resources_available.ncpus = 2
                         resources_available.vnode = master
                         resources_assigned.accelerator_memory = 0kb
                         resources_assigned.hbmem = 0kb
                         resources_assigned.mem = 0kb
                         resources_assigned.naccelerators = 0
                         resources_assigned.ncpus = 0
                         resources_assigned.vmem = 0kb
                         resv_enable = True
                         sharing = default_shared
                         license = l
                         last_state_change_time = Thu Jul 20 14:55:47 2023
                    
                    node1
                         Mom = node1
                         Port = 15002
                         pbs_version = 23.06.06
                         ntype = PBS
                         state = free
                         pcpus = 2
                         resources_available.arch = linux
                         resources_available.host = node1
                         resources_available.mem = 4811568kb
                         resources_available.ncpus = 2
                         resources_available.vnode = node1
                         resources_assigned.accelerator_memory = 0kb
                         resources_assigned.hbmem = 0kb
                         resources_assigned.mem = 0kb
                         resources_assigned.naccelerators = 0
                         resources_assigned.ncpus = 0
                         resources_assigned.vmem = 0kb
                         resv_enable = True
                         sharing = default_shared
                         license = l
                         last_state_change_time = Thu Jul 20 14:55:47 2023
                    
                    node2
                         Mom = node2
                         Port = 15002
                         pbs_version = 23.06.06
                         ntype = PBS
                         state = free
                         pcpus = 2
                         resources_available.arch = linux
                         resources_available.host = node2
                         resources_available.mem = 4811568kb
                         resources_available.ncpus = 2
                         resources_available.vnode = node2
                         resources_assigned.accelerator_memory = 0kb
                         resources_assigned.hbmem = 0kb
                         resources_assigned.mem = 0kb
                         resources_assigned.naccelerators = 0
                         resources_assigned.ncpus = 0
                         resources_assigned.vmem = 0kb
                         resv_enable = True
                         sharing = default_shared
                         license = l
                         last_state_change_time = Thu Jul 20 14:55:54 2023

First make user in all VMs 

            adduser srk
            passwd srk

Now login to srk
           
            su - srk 
            qsub -I   
            qstat

            vim demo.sh
                            #!/bin/sh
                            ### Set the job name (for your reference)
                            #PBS -N testjob
                            ### Set the project name, your department code by default
                            #PBS -P cc
                            ####
                            #PBS -l select=1:ncpus=1
                            ### Specify "wallclock time" required for this job, hhh:mm:ss
                            #PBS -l walltime=00:01:00
                            
                            #PBS -l software=replace_with_Your_software_name
                            # After job starts, must goto working directory.
                            # $PBS_O_WORKDIR is the directory from where the job is fired.
                            echo "==============================="
                            echo $PBS_JOBID
                            #cat $PBS_NODEFILE
                            echo "==============================="
                            cd $PBS_O_WORKDIR

        qsub demo.sh
                            7.master
        qtsat

        qstat -B
        qstat -ans

        


