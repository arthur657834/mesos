https://github.com/wiselyman/study/blob/master/mesos/mesos%E5%AE%89%E8%A3%85.md
https://open.mesosphere.com/getting-started/install/

Master:
	yum -y groupinstall 'Development Tools'
	yum -y install http://repos.mesosphere.com/el/7/noarch/RPMS/mesosphere-el-repo-7-3.noarch.rpm
	yum install -y mesos marathon mesosphere-zookeeper

	echo 1 > /var/lib/zookeeper/myid #a unique integer between 1 and 255 on each node.
	vi /etc/zookeeper/conf/zoo.cfg
	server.1=10.1.240.125:2888:3888
	#server.2=2.2.2.2:2888:3888

	systemctl start zookeeper
	
	vi /etc/mesos/zk
	zk://10.1.240.125:2181/mesos
	#zk://1.1.1.1:2181,2.2.2.2:2181,3.3.3.3:2181/mesos
	
	vi /etc/mesos-master/quorum
	1
	#greater than the number of masters divided by 2
	
	hostnamectl set-hostname mesos-master2
	
	vi /etc/hosts
	10.1.50.250 mesos-master
	10.1.240.125 mesos-master2
	10.1.50.252 mesos-slave
	
	systemctl start mesos-master
	systemctl start mesos-slave
	systemctl start marathon
	
	http://10.1.240.125:5050/
	http://10.1.240.125:8080/
	
Slave:
	hostnamectl set-hostname mesos-master2
	vi /etc/mesos/zk
	zk://10.1.240.125:2181/mesos
	
	systemctl disable mesos-master
	systemctl start mesos-slave
	

	python -m SimpleHTTPServer $PORT
	

