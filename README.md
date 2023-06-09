# GP VM Prerequisite Checker

## Overview

`gp_vm_prerequisite_checker.sh` is a shell script to verify if the necessary prerequisites for deploying a Greenplum Virtual Machine.

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
