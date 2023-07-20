            * CREATE THREE VM's 1 HAS MASTER AND OTHER 2 HA SNOD1 AND NODE2 ,ALL MUST HAVE NAT AND HOSTONLY ADAPTOR
            * PREREQUISITS ON 3 MACHINES:
            * 1>Disable the SELINUX on all vm's.
            * 2>stop firewalld.service 
            * 3>update the /etc/hosts file.
            * 4>Configure the passwordless SSH.


            
  
# ON MASTER:

        * [root@pbs-master ~]# git clone https://github.com/openpbs/openpbs.git
          
        * [root@pbs-master ~]# yum --setopt=group_package_types=mandatory,default,optional groupinstall "Development Tools"
          
        * [root@pbs-master ~]# mv openpbs/ openpbs-23.06.06
          
        * [root@pbs-master ~]# tar -cvf /root/rpmbuild/SOURCES/openpbs-23.06.06.tar.gz openpbs-23.06.06/
          
        * [root@pbs-master openpbs-23.06.06]# yum install libtool-ltdl-devel hwloc-devel libedit-devel libical-devel ncurses-devel postgresql-devel postgresql-contrib python3-devel tcl-devel tk-devel zlib-devel expat-devel openssl-devel libXt-devel -y
          
        * [root@pbs-master openpbs-23.06.06]# rpmbuild -ba openpbs.spec
         
        * [root@pbs-master ~]# cd /root/rpmbuild/RPMS/x86_64/

  * We get following files
                
            output:
        openpbs-client-23.06.06-0.x86_64.rpm     openpbs-devel-23.06.06-0.x86_64.rpm      openpbs-server-23.06.06-0.x86_64.rpm
        openpbs-debuginfo-23.06.06-0.x86_64.rpm  openpbs-execution-23.06.06-0.x86_64.rpm

* [root@pbs-master x86_64]# yum install openpbs-server-23.06.06-0.x86_64.rpm

* [root@pbs-master openpbs-23.06.06]# vi /etc/pbs.conf          {YOU CAN CONFIGURE MOM AS WELL}

                                      OUTPUT:
                                      PBS_EXEC=/opt/pbs
                                      PBS_SERVER=pbs-master
                                      PBS_START_SERVER=1
                                      PBS_START_SCHED=1
                                      PBS_START_COMM=1
                                      PBS_START_MOM=0
                                      PBS_HOME=/var/spool/pbs
                                      PBS_CORE_LIMIT=unlimited
                                      PBS_SCP=/bin/scp
  
* [root@pbs-master openpbs-23.06.06]# cd /root/rpmbuild/RPMS/x86_64/

* [root@pbs-master x86_64]# chmod 4755 /opt/pbs/sbin/pbs_iff  /opt/pbs/sbin/pbs_rcp

* [root@pbs-master x86_64]# systemctl restart pbs
       
                     OR
  
* [root@pbs-master x86_64]# /etc/init.d/pbs restart

* [root@pbs-master x86_64]# systemctl restart pbs

* [root@pbs-master x86_64]# systemctl status pbs

                        OUTPUT:
                         Active: active (running) since Wed 2023-07-19 20:53:52 IST; 2s ago

* [root@pbs-master x86_64]# /etc/init.d/pbs status
  
                        OUTPUT:
                        pbs_server is pid 30660
                        pbs_sched is pid 29150
                        pbs_comm is 29144

* [root@pbs-master x86_64]# . /etc/profile.d/pbs.
  
                        OUTPUT:
                        pbs.csh  pbs.sh   

* [root@pbs-master x86_64]# which qstat

                        OUTPUT:
                        /opt/pbs/bin/qstat

* [root@pbs-master x86_64]# qstat -B
  
                      OUTPUT:
                      
                      Server             Max   Tot   Que   Run   Hld   Wat   Trn   Ext Status
                      ---------------- ----- ----- ----- ----- ----- ----- ----- ----- -----------
                      pbs-master           0     0     0     0     0     0     0     0 Active



===================================================================================================================================================================================================
# ON NODE 1

* yum installl git -y
  
* git clone https://github.com/openpbs/openpbs.git
  
* ls
  
* cd openpbs/
  
* ./autogen.sh
  
* yum provides */autoreconf -y
  
* yum install autoconf automake libtool -y

* [root@pbs-node1 openpbs]# ls
  
                        aclocal.m4  autom4te.cache       buildutils  CODE_OF_CONDUCT.md  configure.ac     COPYRIGHT  INSTALL  m4           Makefile.in        openpbs.spec     PBS_License.txt  src   valgrind.supp
                        autogen.sh  azure-pipelines.yml  ci          configure           CONTRIBUTING.md  doc        LICENSE  Makefile.am  openpbs-rpmlintrc  openpbs.spec.in  README.md        test

* [root@pbs-node1 openpbs]./configure
  
* [root@pbs-node1 openpbs]./configure --prefix=/opt/pbs
  
If we not any dependencies then run below commands

* [root@pbs-node1 openpbs]yum install openssl* -y
  
* [root@pbs-node1 openpbs]yum install libXt-devel -y

Create direcrtory.

* [root@pbs-node1 openpbs]mkdir -p /opt/pbs/lib
  
* [root@pbs-node1 openpbs]ls /opt/pbs/
  
	                          lib

Again configure the below commands

* [root@pbs-node1 openpbs]./configure --prefix=/opt/pbs
  
* [root@pbs-node1 openpbs]yum provides */autoreconf -y

If you still get any error or not get any dependencies then  run below command

* [root@pbs-node1 openpbs]# yum install libtool-ltdl-devel hwloc-devel libedit-devel libical-devel ncurses-devel postgresql-devel postgresql-contrib python3-devel tcl-devel tk-devel zlib-devel expat-devel openssl-devel libXt-devel -y

* [root@pbs-node1 openpbs]./configure --prefix=/opt/pbs

* Again configure with this commands

* [root@pbs-node1 openpbs] make  (run this command)
  
* [root@pbs-node1 openpbs]# make install
  
* [root@pbs-node1 openpbs]# . /etc/profile.d/pbs.sh
  
* [root@pbs-node1 openpbs]# which qstat
  
* [root@pbs-node1 openpbs]# chmod +x /opt/pbs/etc/pbs.sh
  
* [root@pbs-node1 openpbs]# export PATH=${PATH}:/opt/pbs/bin
  
* [root@pbs-node1 openpbs]# which qstat
  
                            /opt/pbs/bin/qstat
  
* [root@pbs-node1 openpbs]# vim /etc/pbs.conf
  
                                        PBS_EXEC=/opt/pbs
                                        PBS_SERVER=pbs-master
                                        PBS_START_SERVER=0
                                        PBS_START_SCHED=0
                                        PBS_START_COMM=0
                                        PBS_START_MOM=1
                                        PBS_HOME=/var/spool/pbs
                                        PBS_CORE_LIMIT=unlimited
                                        PBS_SCP=/bin/scp

* [root@pbs-node1 openpbs]# cat /etc/pbs.conf
  
* [root@pbs-node1 openpbs]# systemctl status pbs

=============================================================================================================

# ON MASTER MACHINE
* NFS CONFIGURATION
  
* yum install -y nfs-utils
  
* systemctl start nfs-server rpcbind

* systemctl enable nfs-server rpcbind
  
* mkdir /mnt/home
  
* chmod 777 /mnt/home/
  
* systemctl restart nfs

* ls /root/rpmbuild/RPMS/x86_64/
  
* cd /root/rpmbuild/RPMS/x86_64/
  
* cp * /home/rpms/
  
* cp * /mnt/home/rpms/
  
* cd
  
* mkdir /home/rpms
  
* cd /root/rpmbuild/RPMS/x86_64/
  
* cp * /home/rpms/
  
* vim /etc/exports
		                      /home *(rw,sync,no_root_squash)

* cp * /home/rpms/history
  
* [root@pbs-master ~]# vim /var/spool/pbs/server_priv/nodes
  
          				pbs-node1 np=1
          				pbs-node2 np=1
  
* [root@pbs-master ~]# pbsnodes -a
  
                pbsnodes: Server has no node list


* [root@pbs-master x86_64]# qmgr
  
              Max open servers: 49
              Qmgr: create node pbs-node1
              Qmgr: create node pbs-node2
              Qmgr: 

ctl c

* [root@pbs-master x86_64]# pbsnodes -a
  
                              pbs-node1
                                   Mom = pbs-node1
                                   Port = 15002
                                   pbs_version = 23.06.06
                                   ntype = PBS
                                   state = free
                                   pcpus = 2
                                   resources_available.arch = linux
                                   resources_available.host = pbs-node1
                                   resources_available.mem = 1863028kb
                                   resources_available.ncpus = 2
                                   resources_available.vnode = pbs-node1
                                   resources_assigned.accelerator_memory = 0kb
                                   resources_assigned.hbmem = 0kb
                                   resources_assigned.mem = 0kb
                                   resources_assigned.naccelerators = 0
                                   resources_assigned.ncpus = 0
                                   resources_assigned.vmem = 0kb
                                   resv_enable = True
                                   sharing = default_shared
                                   license = l
                                   last_state_change_time = Thu Jul 20 20:24:19 2023
                              
                              pbs-node2
                                   Mom = pbs-node2
                                   Port = 15002
                                   pbs_version = 23.06.06
                                   ntype = PBS
                                   state = free
                                   pcpus = 2
                                   resources_available.arch = linux
                                   resources_available.host = pbs-node2
                                   resources_available.mem = 1863028kb
                                   resources_available.ncpus = 2
                                   resources_available.vnode = pbs-node2
                                   resources_assigned.accelerator_memory = 0kb
                                   resources_assigned.hbmem = 0kb
                                   resources_assigned.mem = 0kb
                                   resources_assigned.naccelerators = 0
                                   resources_assigned.ncpus = 0
                                   resources_assigned.vmem = 0kb
                                   resv_enable = True
                                   sharing = default_shared
                                   license = l
                                   last_state_change_time = Thu Jul 20 20:24:33 2023
  
* [root@pbs-master x86_64]# vim /etc/pbs.conf
    
                                      PBS_EXEC=/opt/pbs
                                      PBS_SERVER=pbs-master
                                      PBS_START_SERVER=1
                                      PBS_START_SCHED=1
                                      PBS_START_COMM=1
                                      PBS_START_MOM=1
                                      PBS_HOME=/var/spool/pbs
                                      PBS_CORE_LIMIT=unlimited
                                      PBS_SCP=/bin/scp    (make MOM=1 )
  
Then Add Master Node
* [root@pbs-master x86_64]# qmgr
  
                          Max open servers: 49
                          Qmgr: create node pbs-master

* [root@pbs-master x86_64]# pbsnodes -a
  
                                          pbs-node1
                                               Mom = pbs-node1
                                               Port = 15002
                                               pbs_version = 23.06.06
                                               ntype = PBS
                                               state = free
                                               pcpus = 2
                                               resources_available.arch = linux
                                               resources_available.host = pbs-node1
                                               resources_available.mem = 1863028kb
                                               resources_available.ncpus = 2
                                               resources_available.vnode = pbs-node1
                                               resources_assigned.accelerator_memory = 0kb
                                               resources_assigned.hbmem = 0kb
                                               resources_assigned.mem = 0kb
                                               resources_assigned.naccelerators = 0
                                               resources_assigned.ncpus = 0
                                               resources_assigned.vmem = 0kb
                                               resv_enable = True
                                               sharing = default_shared
                                               license = l
                                               last_state_change_time = Thu Jul 20 20:30:34 2023
                                          
                                          pbs-node2
                                               Mom = pbs-node2
                                               Port = 15002
                                               pbs_version = 23.06.06
                                               ntype = PBS
                                               state = free
                                               pcpus = 2
                                               resources_available.arch = linux
                                               resources_available.host = pbs-node2
                                               resources_available.mem = 1863028kb
                                               resources_available.ncpus = 2
                                               resources_available.vnode = pbs-node2
                                               resources_assigned.accelerator_memory = 0kb
                                               resources_assigned.hbmem = 0kb
                                               resources_assigned.mem = 0kb
                                               resources_assigned.naccelerators = 0
                                               resources_assigned.ncpus = 0
                                               resources_assigned.vmem = 0kb
                                               resv_enable = True
                                               sharing = default_shared
                                               license = l
                                               last_state_change_time = Thu Jul 20 20:30:34 2023
                                          
                                          pbs-master
                                               Mom = pbs-master
                                               Port = 15002
                                               pbs_version = 23.06.06
                                               ntype = PBS
                                               state = free
                                               pcpus = 2
                                               resources_available.arch = linux
                                               resources_available.host = pbs-master
                                               resources_available.mem = 3861076kb
                                               resources_available.ncpus = 2
                                               resources_available.vnode = pbs-master
                                               resources_assigned.accelerator_memory = 0kb
                                               resources_assigned.hbmem = 0kb
                                               resources_assigned.mem = 0kb
                                               resources_assigned.naccelerators = 0
                                               resources_assigned.ncpus = 0
                                               resources_assigned.vmem = 0kb
                                               resv_enable = True
                                               sharing = default_shared
                                               license = l
                                               last_state_change_time = Thu Jul 20 20:30:37 2023
                                            
                                          

====================================================================================================================

# ON NODE 2

* yum install -y nfs-utils
  
* showmount -e 192.168.206.160(NAT ip of master)
  
* mkdir /mnt/home
  
* mount 192.168.206.160:/home /mnt/home
  
* ls /root/rpmbuild/RPMS/x86_64/
  
* showmount -e 192.168.206.160
  
* df -Th
  
* cd /mnt/home/
  
* ls
  
* cd rpms
  
* [root@pbs-node2 rpms]# ls

                  openpbs-client-23.06.06-0.x86_64.rpm     openpbs-devel-23.06.06-0.x86_64.rpm      openpbs-server-23.06.06-0.x86_64.rpm
                  openpbs-debuginfo-23.06.06-0.x86_64.rpm  openpbs-execution-23.06.06-0.x86_64.rpm

* yum install openpbs-execution-23.06.06-0.x86_64.rpm -y
  
* vim /etc/pbs.conf 
                            		PBS_EXEC=/opt/pbs
                            		PBS_SERVER=pbs-master
                            		PBS_START_SERVER=0
                            		PBS_START_SCHED=0
                            		PBS_START_COMM=0
                            		PBS_START_MOM=1
                            		PBS_HOME=/var/spool/pbs
                            		PBS_CORE_LIMIT=unlimited
                            		PBS_SCP=/bin/scp

* [root@pbs-node2 rpms]# vim /var/spool/pbs/mom_priv/config 
                      				$logevent 0x1ff
                      				#$clientname pbs-node2
                      				$restrict_user_maxsysid 999
  
* [root@pbs-node2 rpms]# vim /var/spool/pbs/mom_priv/config
  
			                    	$restrict_user_maxsysid 999

====================================================================================================================================================================================

* For submit the job we have to create user on all machines. because job submitted in user only

                       useradd testuser
                        passwd testuser


* Then switch user which is created in all machines.

        [root@pbs-master ~]# su - testuser
  
* To submit the job run command     `qsub -I`

        [root@pbs-master ~]# su - testuser
        [testuser@pbs-master ~]$ qsub -I 
                                    qsub: waiting for job 2.pbs-master to start
                                    qsub: job 2.pbs-master ready


* NOW we submit job to pbs

* For that we create in user `demo.sh` file

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
                                  
                                  :wq

 * [root@pbs-master ~]#qsub demo.sh
   
	                    4.pbs-master

 * [root@pbs-master ~]# qstat
 
=======================================================================================================================
* For 2nd job
  
* [testuser@pbs-master ~]$ vim testjob.sh

                                vim testjob.sh
                                		#!/bin/sh
                                		### Set the job name (for your reference)
                                		#PBS -N testjob
                                		### Set the project name, your department code by default
                                		#PBS -P cc
                                		### Request email when job begins and ends
                                		#PBS -m bea
                                		### Specify email address to use for notification.
                                		#PBS -M $USER@iitd.ac.in
                                		### Specify required no of node in select and specfify required cpu cores in ncpus
                                		#PBS -l select=1:ncpus=1
                                		### Specify "wallclock time" required for this job, hhh:mm:ss
                                		#PBS -l walltime=00:00:05
                                
                                		#PBS -l software=replace_with_Your_software_name
                                		# After job starts, must goto working directory.
                                		# $PBS_O_WORKDIR is the directory from where the job is fired.
                                		echo "==============================="
                                		echo $PBS_JOBID
                                		#cat $PBS_NODEFILE
                                		echo "==============================="
                                		hostname
                                		cd $PBS_O_WORKDIR
                                		#job
                                		###time -p mpirun -n {n*m} executable
                                		#NOTE
                                		# The job line is an example : users need to change it to suit their applications
                                		# The PBS select statement picks n nodes each having m free processors
                                		# OpenMPI needs more options such as $PBS_NODEFILE
                                :wq

* [testuser@pbs-master ~]$ qsub testjob.sh
  
                          6.pbs-master

* [testuser@pbs-master ~]$ qstat
  
                                          Job id            Name             User              Time Use S Queue
                                          ----------------  ---------------- ----------------  -------- - -----
                                          6.pbs-master      testjob.sh       testuser          00:00:00 E workq

  


  
  


  

  
