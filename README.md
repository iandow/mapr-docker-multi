Quickstart for running multi-node MapR clusters in Docker
=========================================================

This project provides a simple bash script for creating multi-node MapR clusters in Docker. Each node in the cluster will run in its own Docker container.

Docker Requirements and set-up:
-------------------------------

1. Docker v1.6.0 or later is required to run this set-up script.

2. Docker Network : Docker containers MapR cluster require public IPs. Please work with your IT to get a routable IP network range for the docker bridge.
	- Configure a Network brige with an IP from the routable IP range.
	- Add the following options to the docker daemon : '-b <bridgename> --fixed-cidr=cidr-or-routable-range'
		Eg: '-b br0 --fixed-cidr=10.10.101.16/29' - This makes docker to allocate the IPs 10.10.101.17 - 10.10.101.22 (6 IPs). 

3. (optional) Docker Disk options: Please add the following options to the docker daemon : '--storage-opt dm.basesize=30G --storage-opt dm.loopdatasize=200G'

4. Restart the docker daemon with the above options.

Container Requirement: 
----------------------

Each container in the cluster requires at least one disk. Please make sure enough number of free disks are there in the system. Create a disk list file with one disk/partition per line. For example:
	
	# cat /tmp/disklist.txt 
	/dev/sdb
	/dev/sdc
	/dev/sdd
	/dev/sde
	/dev/sdf
	#

Usage: 
------

Run the launch script corresponding to the MapR version you want to deploy, like this:  

`./(mapr version)/launch-cluster.sh ClusterName NumberOfNodes MemSize-in-kB Path-to-DisklistFile`

For example, `./5.2.0/launch-cluster.sh demo 3 24576000 /tmp/diskfile.txt` will launch a 3 node MapR version 5.2.0 cluster. Each node will run as a different Docker container with 24GB of RAM memory allocated to it. 

Port forwarding will be setup to provide access to the MapR Control System on port 9443 of the control node.

