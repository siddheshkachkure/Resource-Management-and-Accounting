# Prerequisite
* Need 3 VM
    * Master : 1
        * Adapters : NAT HO
    * Clients : 2
        * Adapters :  NAT HO 

# Configure NFS Server Master

### Disable Firewalld & SELinux

    systemctl status firewalld
    systemctl stop firewalld
    systemctl disable firewalld
    systemctl status firewalld
    
    setenforce 0
    getenforce

#### Install NFS Server

    yum install -y nfs-utils

#### Once the packages are installed, enable and start NFS services.

    systemctl start nfs-server rpcbind
    systemctl enable nfs-server rpcbind

#### Create NFS Share

Now, let’s create a directory to share with the NFS client. 
Here I will be creating a new directory named home in the / partition.

    mkdir /home

#### Allow NFS client to read and write to the created directory.

    chmod 777 /home/

#### We have to modify /etc/exports file to make an entry of directory /home that you want to share.

    vi /etc/exports

and add this 

     /home *(rw,sync,no_root_squash)  
     
     [/home ip needs to be here where clients are there(rw,sync,no_root_squash)]



#### Export the shared directories using the following command.

    exportfs -r

# Configure NFS client

## Disable Firewalld & SELinux

    systemctl status firewalld
    systemctl stop firewalld
    systemctl disable firewalld
    systemctl status firewalld

    setenforce 0
    getenforce

#### Install NFS Client

    yum install -y nfs-utils

#### Check NFS Share

Before mounting the NFS share,
I request you to check the NFS shares available on the NFS server by running the following command on the NFS client.
    
    showmount -e 192.168.100.186

#### Mount NFS Share

Now, create a directory on NFS client to mount the NFS share /home which we have created in the NFS server.

    mkdir /mnt/home

Use below command to mount a NFS share /home from NFS server 192.168.100.186 in /mnt/nfsfileshare on NFS client.

    mount 192.168.100.186:/home /mnt/home

Verify the mounted share on the NFS client using mount command.

    mount | grep nfs

OUTPUT: 

    [root@RMA3 ~]# mount | grep nfs
    sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw,relatime)
    192.168.100.186:/home on /mnt/home type nfs4 (rw,nosuid,relatime,sync,vers=4.1,rsize=1048576,wsize=1048576,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,clientaddr=192.168.100.188,local_lock=none,addr=192.168.100.186)


Also, you can use the df -hT command to check the mounted NFS share.

    df -hT

![Capture](https://github.com/shubnimkar/RMA/assets/46809421/440a4d3d-dd2b-4f12-b6e1-4a8744cd0c68)

Create a file on the mounted directory to verify the read and write access on NFS share.

    touch /mnt/home/test
    
#### Automount NFS Shares

To mount the shares automatically on every reboot, you would need to modify /etc/fstab file of your NFS client.
    
    vi /etc/fstab

Add an entry something like below.

     192.168.100.186:/home /mnt/home			nfs 	nosuid,rw,sync,hard,intr 0 0

     [root@rma2 rma2]# cat /etc/fstab

    #
    # /etc/fstab
    # Created by anaconda on Thu Jul 13 15:32:36 2023
    #
    # Accessible filesystems, by reference, are maintained under '/dev/disk'
    # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
    #
    /dev/mapper/centos-root /                       xfs     defaults        0 0
    UUID=1b5f2893-aa43-4137-b7c9-bb06a599339d /boot                   xfs     defaults        0 0
    /dev/mapper/centos-home /home                   xfs     defaults        0 0
    /dev/mapper/centos-swap swap                    swap    defaults        0 0
    192.168.100.186:/home /mnt/home			nfs 	nosuid,rw,sync,hard,intr 0 0


#### Save and close the file.

Reboot the client machine and check whether the share is automatically mounted or not.

    reboot

#### Verify the mounted share on the NFS client using mount command.

    mount | grep nfs


## PASSWORD LESS SSH

### on master

    ssh-keygen -t rsa
    ssh-copy-id root@client address


### on all 3

* update host files
* yum install epel-release -y
* yum install munge munge-libs munge-devel

### master

		[root@master home]# rpm -qa | grep munge
		munge-libs-0.5.11-3.el7.x86_64
		munge-devel-0.5.11-3.el7.x86_64
		munge-0.5.11-3.el7.x86_64

* /usr/sbin/create-munge-key -r
  
		[root@master home]# /usr/sbin/create-munge-key -r
		Please type on the keyboard, echo move your mouse,
		utilize the disks. This gives the random number generator
		a better chance to gain enough entropy.
		Generating a pseudo-random key using /dev/random completed.

### To check the key 

		[root@master home]# ll /etc/munge/
		total 4
		-r--------. 1 munge munge 1024 Jul 13 16:55 munge.key
		
		scp /etc/munge/munge.key client1 client2:/etc/munge/
		chown munge:munge /etc/munge/

### on all clients
      
	chown munge:munge /etc/munge/munge.key
      
### on all three

	systemctl start munge
	systemctl enable munge
	systemctl status munge

[GET SLURM FROM HERE](https://download.schedmd.com/slurm/slurm-20.11.9.tar.bz2)

### ONMASTER 

	wget https://download.schedmd.com/slurm/slurm-20.11.9.tar.bz2
	
	yum install rpm-build -y

	[root@master ~]# rpmbuild -ta slurm-20.11.9.tar.bz2 
	error: Failed build dependencies:
		python3 is needed by slurm-20.11.9-1.el7.x86_64
		readline-devel is needed by slurm-20.11.9-1.el7.x86_64
		perl(ExtUtils::MakeMaker) is needed by slurm-20.11.9-1.el7.x86_64
		pam-devel is needed by slurm-20.11.9-1.el7.x86_64

### on all 3

	yum install python3 readline-devel perl-ExtUtils-MakeMaker pam-devel gcc mysql-devel -y

TO BUILD REPO (MASTER

	rpmbuild -ta slurm-20.11.9.tar.bz2 

### on all 3 

	export SLURMUSER=900
	groupadd -g $SLURMUSER slurm
	useradd -m -c "SLURM workload manager" -d /var/lib/slurm -u $SLURMUSER -g slurm -s /bin/bash slurm

### ON MASTER

	mkdir /home/rpms
	cd /root/rpmbuild/RPMS/x86_64/
	cp * /home/rpms/

### on all 3

	[root@master rpms]# yum --nogpgcheck localinstall * -y

on client slurmctld and slurmdbd packages are not req 

	[root@client2 rpms]# yum --nogpgcheck localinstall * -y
	[root@rma2 rpms]# yum --nogpgcheck localinstall * -y

TO check packages on all nodes no will be 12

	rpm -qa | grep slurm | wc -l
 

### on all 3

	mkdir /var/spool/slurm
	chown slurm:slurm /var/spool/slurm
	chmod 755 /var/spool/slurm/

	mkdir /var/log/slurm
	chown slurm:slurm /var/log/slurm
	chmod 755 /var/log/slurm/
	chown -R slurm . /var/log/slurm/

### MASTER: 

	[root@master ~]# touch /var/log/slurm/slurmctld.log
	[root@master ~]# chown slurm:slurm /var/log/slurm/slurmctld.log
	[root@master ~]# touch /var/log/slurm_jobacct.log /var/log/slurm_jobcomp.log
	[root@master ~]# chown slurm: /var/log/slurm_jobacct.log /var/log/slurm_jobcomp.log
	[root@master ~]# cp /etc/slurm/slurm.conf.example /etc/slurm/slurm.conf
	[root@master ~]# vi /etc/slurm/slurm.conf

		#
		# Example slurm.conf file. Please run configurator.html
		# (in doc/html) to build a configuration file customized
		# for your environment.
		#
		#
		# slurm.conf file generated by configurator.html.
		#
		# See the slurm.conf man page for more information.
		#
		ClusterName=hpcsa
		ControlMachine=master
		#ControlAddr=
		#BackupController=
		#BackupAddr=
		#
		SlurmUser=slurm
		#SlurmdUser=root
		SlurmctldPort=6817
		SlurmdPort=6818
		AuthType=auth/munge
		#JobCredentialPrivateKey=
		#JobCredentialPublicCertificate=
		StateSaveLocation=/var/spool/slurm/ctld
		SlurmdSpoolDir=/var/spool/slurm/d
		SwitchType=switch/none
		MpiDefault=none
		SlurmctldPidFile=/var/run/slurmctld.pid
		SlurmdPidFile=/var/run/slurmd.pid
		ProctrackType=proctrack/pgid
		#PluginDir=
		#FirstJobId=
		ReturnToService=0
		#MaxJobCount=
		#PlugStackConfig=
		#PropagatePrioProcess=
		#PropagateResourceLimits=
		#PropagateResourceLimitsExcept=
		#Prolog=
		#Epilog=
		#SrunProlog=
		#SrunEpilog=
		#TaskProlog=
		#TaskEpilog=
		#TaskPlugin=
		#TrackWCKey=no
		#TreeWidth=50
		#TmpFS=
		#UsePAM=
		#
		# TIMERS
		SlurmctldTimeout=300
		SlurmdTimeout=300
		InactiveLimit=0
		MinJobAge=300
		KillWait=30
		Waittime=0
		#
		# SCHEDULING
		SchedulerType=sched/backfill
		#SchedulerAuth=
		SelectType=select/cons_tres
		SelectTypeParameters=CR_Core
		#PriorityType=priority/multifactor
		#PriorityDecayHalfLife=14-0
		#PriorityUsageResetPeriod=14-0
		#PriorityWeightFairshare=100000
		#PriorityWeightAge=1000
		#PriorityWeightPartition=10000
		#PriorityWeightJobSize=1000
		#PriorityMaxAge=1-0
		#
		# LOGGING
		SlurmctldDebug=info
		SlurmctldLogFile=/var/log/slurmctld.log
		SlurmdDebug=info
		SlurmdLogFile=/var/log/slurmd.log
		JobCompType=jobcomp/none
		#JobCompLoc=
		#
		# ACCOUNTING
		#JobAcctGatherType=jobacct_gather/linux
		#JobAcctGatherFrequency=30
		#
		#AccountingStorageType=accounting_storage/slurmdbd
		#AccountingStorageHost=
		#AccountingStorageLoc=
		#AccountingStoragePass=
		#AccountingStorageUser=
		#
		# COMPUTE NODES
		#NodeName=linux[1-32] Procs=1 State=UNKNOWN
		NodeName=client1 CPUs=2 Boards=1 SocketsPerBoard=2 CoresPerSocket=1 ThreadsPerCore=1 RealMemory=5666 State=UNKNOWN
		NodeName=client2 CPUs=2 Boards=1 SocketsPerBoard=2 CoresPerSocket=1 ThreadsPerCore=1 RealMemory=5666 State=UNKNOWN
		PartitionName=standard Nodes=ALL Default=YES MaxTime=INFINITE State=UP




	[root@master ~]# scp /etc/slurm/slurm.conf client1:/etc/slurm/
	slurm.conf                                                                                                                                                                       100% 2188     2.5MB/s   00:00    
	[root@master ~]# scp /etc/slurm/slurm.conf client2:/etc/slurm/
	slurm.conf                                                                                                                                                                       100% 2188   297.3KB/s   00:00    
	[root@master ~]# systemctl start slurmctld
	[root@master ~]# systemctl enable slurmctld
	[root@master ~]# systemctl status slurmctld




	[root@rma2 ~]# slurmd -C
	NodeName=client1 CPUs=2 Boards=1 SocketsPerBoard=2 CoresPerSocket=1 ThreadsPerCore=1 RealMemory=5666
	UpTime=0-03:28:53
	[root@rma2 ~]# systemctl start slurmd
	[root@rma2 ~]# systemctl enable slurmd
	[root@rma2 ~]# systemctl status slurmd

	[root@client2 ~]# slurmd -C
	NodeName=client2 CPUs=2 Boards=1 SocketsPerBoard=2 CoresPerSocket=1 ThreadsPerCore=1 RealMemory=5666
	UpTime=0-03:28:58
	[root@client2 ~]# systemctl start slurmd
	[root@client2 ~]# systemctl enable slurmd
	[root@client2 ~]# systemctl status slurmd

#### To check is clients are online :

	[root@master ~]# sinfo
	PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
	standard*    up   infinite      2   idle client[1-2]

	[root@master ~]# sinfo -R
	REASON               USER      TIMESTAMP           NODELIST

#### if nodes are down:

	scontrol update node <nodename> state=idle

* if it shows `down*`

		restart slurmd and then fire above command
  
### how to debug error in slurm

	[root@master ~]# slurmctld -Dvv

##JOBS SUBMISSION:


### Submit jobs on master:

* -w: name of node
* --pty: shell it wants

* For single client:
   
		[root@master ~]# srun -w client1 --pty /bin/bash
		[root@client1 ~]#

* For multiclient:
  
		[root@master ~]# srun -w client[1-2] --pty /bin/bash
		[root@client1 ~]#

### To put any node in maintenance

	[root@master ~]# scontrol update node=client1 state=down reason=main
	[root@master ~]# sinfo -R
	REASON               USER      TIMESTAMP           NODELIST
	main                 root      2023-07-18T17:14:10 client1

### to remove from it

	[root@master ~]# scontrol update node=client1 state=resume
	[root@master ~]# sinfo -R
	REASON               USER      TIMESTAMP           NODELIST
	
	### SBATCH

### Submit jobs in batch via script

#### Script

	[root@master ~]# cat demo.sh

		#!/bin/bash
		#SBATCH --partition=standard  #chose the partition
		#SBATCH --job-name=my_job
		#SBATCH --nodes=2              # Number of nodes
		#SBATCH --ntasks=1             # Number of tasks across all nodes
		#SBATCH --cpus-per-task=1      # Number of OpenMP threads for each MPI process/rank
		#SBATCH --time=00:05:00      # Walltime in hh:mm:ss or d-hh:mm:ss
		#SBATCH --output=output%j.log   # Standard output
		#SBATCH --output=error%j.log   # Standard error log
		date
		sleep 3000

### To submit job via sbatch

### When jobs are running `squeue` and `sinfo` state


	[root@master ~]# sbatch demo.sh
	Submitted batch job 7
 
	[root@master ~]# squeue
	             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
	                 6  standard   my_job     root  R       1:29      1 client1
	                 7  standard   my_job     root  R       0:04      2 client[1-2]
	

	[root@master ~]# sinfo
	PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
	standard*    up   infinite      1    mix client2
	standard*    up   infinite      1  alloc client1

	[root@master ~]# squeue
	        JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
	            7  standard   my_job     root  R       3:48      2 client[1-2]

### When no jobs are running `squeue` and `sinfo` state

	[root@master ~]# squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
	     
	[root@master ~]# sinfo
	PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
	standard*    up   infinite      2   idle client[1-2]

### To see job status in deep

	[root@master ~]# sbatch demo.sh
	Submitted batch job 8
 
	[root@master ~]# squeue
	             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
	                 8  standard   my_job     root  R       0:06      2 client[1-2]
		  
	[root@master ~]# scontrol show job 8
	
 	JobId=8 JobName=my_job
	   UserId=root(0) GroupId=root(0) MCS_label=N/A
	   Priority=4294901753 Nice=0 Account=(null) QOS=(null)
	   `JobState=RUNNING` Reason=None Dependency=(null)
	   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
	   RunTime=00:00:26 TimeLimit=00:05:00 TimeMin=N/A
	   SubmitTime=2023-07-18T17:49:05 EligibleTime=2023-07-18T17:49:05
	   AccrueTime=2023-07-18T17:49:05
	   StartTime=2023-07-18T17:49:05 EndTime=2023-07-18T17:54:05 Deadline=N/A
	   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2023-07-18T17:49:05
	   Partition=standard AllocNode:Sid=master:4060
	   ReqNodeList=(null) ExcNodeList=(null)
	   NodeList=client[1-2]
	   BatchHost=client1
	   NumNodes=2 NumCPUs=2 NumTasks=2 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
	   TRES=cpu=2,node=2,billing=2
	   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
	   MinCPUsNode=1 MinMemoryNode=0 MinTmpDiskNode=0
	   Features=(null) DelayBoot=00:00:00
	   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
	   Command=/root/demo.sh
	   WorkDir=/root
	   StdErr=/root/error8.log
	   StdIn=/dev/null
	   StdOut=/root/error8.log
	   Power=
	   NtasksPerTRES:0
    
### To cancel jobs

	[root@master ~]# scancel 8
 
	[root@master ~]# squeue
	             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
	      
	[root@master ~]# scontrol show job 8
 
	JobId=8 JobName=my_job
	   UserId=root(0) GroupId=root(0) MCS_label=N/A
	   Priority=4294901753 Nice=0 Account=(null) QOS=(null)
	   `JobState=CANCELLED` Reason=None Dependency=(null)
	   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:15
	   RunTime=00:00:46 TimeLimit=00:05:00 TimeMin=N/A
	   SubmitTime=2023-07-18T17:49:05 EligibleTime=2023-07-18T17:49:05
	   AccrueTime=2023-07-18T17:49:05
	   StartTime=2023-07-18T17:49:05 EndTime=2023-07-18T17:49:51 Deadline=N/A
	   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2023-07-18T17:49:05
	   Partition=standard AllocNode:Sid=master:4060
	   ReqNodeList=(null) ExcNodeList=(null)
	   NodeList=client[1-2]
	   BatchHost=client1
	   NumNodes=2 NumCPUs=2 NumTasks=2 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
	   TRES=cpu=2,node=2,billing=2
	   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
	   MinCPUsNode=1 MinMemoryNode=0 MinTmpDiskNode=0
	   Features=(null) DelayBoot=00:00:00
	   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
	   Command=/root/demo.sh
	   WorkDir=/root
	   StdErr=/root/error8.log
	   StdIn=/dev/null
	   StdOut=/root/error8.log
	   Power=
	   NtasksPerTRES:0

### To see user account

	[root@master ~]# sshare
	             Account       User  RawShares  NormShares    RawUsage  EffectvUsage  FairShare
	-------------------- ---------- ---------- ----------- ----------- ------------- ----------

 # SLURM RESERVATION CREATION

 [SLURM RESERVATION CREATION](https://slurm.schedmd.com/reservations.html#creation){:target="_blank"}
 .
 abc
