# GP VM Prerequisite Checker

## Overview

`gp_vm_prerequisite_checker.sh` is a shell script to verify if the necessary prerequisites for deploying a Greenplum Virtual Machine.

Please check the following document:

https://docs.vmware.com/en/VMware-Greenplum/6/greenplum-database/vsphere-deploying-byo-template-mirrorless.html

## Functionality

The script checks for the following prerequisites:

- SELinux status
- The status of various services, including 'firewalld', 'tuned', 'chronyd', 'cgconfig.service', and 'ntpd'
- NTP servers configuration
- The mount status and fstab entries of the device
- Ownership of directories such as '/gpdata/master', '/gpdata/mirror' (if using standby master), and '/gpdata/primary' 
- Kernel parameters including 'transparent_hugepage=never' and 'elevator=deadline'
- Ulimit values
- Cgroup directories and their permissions
- System parameters such as vm.swappiness
- SSH password-less login for user 'gpadmin' to 'localhost'
- Readahead values of specified devices
- RX Jumbo settings
- MTU size
- Installed RPM packages
- hosts, hosts-all, and hosts-segments files
## Usage

To execute the script, navigate to the directory containing the script and run the following command:

```bash
sh gp_vm_prerequisite_checker.sh
```

## Example Output

Here is an example of what the output of the script might look like:

```bash
[root@greenplum-db-base-vm ~]# sh gp_vm_prerequisite_checker.sh
PASSED: The SELinux status is correctly set to 'disabled'.
PASSED: The firewalld is correctly set to 'disabled'.
PASSED: The tuned is correctly set to 'disabled'.
PASSED: The chronyd is correctly set to 'disabled'.
PASSED: The cgconfig.service is correctly set to 'enabled'.
PASSED: The ntpd is correctly set to 'enabled'.
PASSED: NTP is properly configured with 2 server(s). Server details:
*192.168.1.2   192.168.1.3     3 u  586 1024  377    0.185    0.567   0.255
 192.168.1.4   .STEP.          16 u    - 1024    0    0.000    0.000   0.000
PASSED: The /dev/sdb mount point is correctly set to '/gpdata'.
PASSED: The fstab entry for /dev/sdb is correctly set to '/dev/sdb /gpdata/ xfs rw,nodev,noatime,inode64 0 0'.
PASSED: The Owner of /gpdata/master is correctly set to 'gpadmin'.
PASSED: The Group of /gpdata/master is correctly set to 'gpadmin'.
PASSED: The Owner of /gpdata/primary is correctly set to 'gpadmin'.
PASSED: The Group of /gpdata/primary is correctly set to 'gpadmin'.
PASSED: The Presence of parameter transparent_hugepage=never in /proc/cmdline is correctly set to 'Present'.
PASSED: The Presence of parameter elevator=deadline in /proc/cmdline is correctly set to 'Present'.
PASSED: The ulimit -n is correctly set to '524288'.
PASSED: The ulimit -u is correctly set to '131072'.
PASSED: The Owner of /sys/fs/cgroup/cpu/gpdb is correctly set to 'gpadmin'.
PASSED: The Group of /sys/fs/cgroup/cpu/gpdb is correctly set to 'gpadmin'.
PASSED: The Owner of /sys/fs/cgroup/cpuacct/gpdb is correctly set to 'gpadmin'.
PASSED: The Group of /sys/fs/cgroup/cpuacct/gpdb is correctly set to 'gpadmin'.
PASSED: The Owner of /sys/fs/cgroup/cpuset/gpdb is correctly set to 'gpadmin'.
PASSED: The Group of /sys/fs/cgroup/cpuset/gpdb is correctly set to 'gpadmin'.
PASSED: The Owner of /sys/fs/cgroup/memory/gpdb is correctly set to 'gpadmin'.
PASSED: The Group of /sys/fs/cgroup/memory/gpdb is correctly set to 'gpadmin'.
PASSED: The Value of vm/min_free_kbytes is correctly set to '943718'.
PASSED: The Value of vm/overcommit_memory is correctly set to '2'.
PASSED: The Value of vm/overcommit_ratio is correctly set to '95'.
PASSED: The Value of net/ipv4/ip_local_port_range is correctly set to '10000    65535'.
PASSED: The Value of kernel/shmall is correctly set to '3932160'.
PASSED: The Value of kernel/shmmax is correctly set to '16106127360'.
PASSED: The Value of vm/dirty_background_ratio is correctly set to '3'.
PASSED: The Value of vm/dirty_ratio is correctly set to '10'.
PASSED: Passwordless SSH login for gpadmin on localhost is enabled.
ERROR: The The readahead value for /dev/sdb is '256', but we expected it to be '16384'.
ERROR: The The output of the command 'ethtool -g ens192' is '256', but we expected it to be '4096'.
ERROR: The MTU size for ens192 is '1500', but we expected it to be '9000'.
PASSED: The apr installation is correctly set to 'installed'.
PASSED: The apr-util installation is correctly set to 'installed'.
PASSED: The dstat installation is correctly set to 'installed'.
PASSED: The greenplum-db-6 installation is correctly set to 'installed'.
PASSED: The krb5-devel installation is correctly set to 'installed'.
PASSED: The libcgroup-tools installation is correctly set to 'installed'.
PASSED: The libevent installation is correctly set to 'installed'.
PASSED: The libyaml installation is correctly set to 'installed'.
PASSED: The net-tools installation is correctly set to 'installed'.
PASSED: The ntp installation is correctly set to 'installed'.
PASSED: The perl installation is correctly set to 'installed'.
PASSED: The rsync installation is correctly set to 'installed'.
PASSED: The sos installation is correctly set to 'installed'.
PASSED: The tree installation is correctly set to 'installed'.
PASSED: The wget installation is correctly set to 'installed'.
PASSED: The which installation is correctly set to 'installed'.
PASSED: The zip installation is correctly set to 'installed'.

Please make sure theses name resolution settings are correct:
Contents of /etc/hosts:
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.1.5   mdw
192.168.1.6  sdw1

Contents of /home/gpadmin/hosts-all:
mdw
sdw1

Contents of /home/gpadmin/hosts-segments:
sdw1
```
## Tested Versions and info
- CentOS7
- greenplum-db-6.24.3-rhel7-x86_64
- Mirrorless deployment on vSphere 8.0U1 and vSAN 8.0U1
