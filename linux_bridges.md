Create a Linux Bridge

Create the xml file
<network>
  <name>service</name>
  <bridge name="service" />
  <forward mode="route" />
  <ip address="172.16.254.1" netmask="255.255.255.0" />
</network>

List the current networks
[root@centos images]# virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes

Create the service network
[root@centos andy]# virsh net-define /home/images/service.xml
Network service defined from /home/images/service.xml




Start the new bridge
[root@centos andy]# virsh net-start service
Network service started



List the networks
[root@centos images]# virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
 service              active     no            yes



Autostart the new bridge
[root@centos andy]# virsh net-autostart service
Network service marked as autostarted

[root@centos andy]# virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes
 service              active     yes           yes


Verify that the bridge is created
[root@centos images]# ifconfig service
service: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.16.254.1  netmask 255.255.255.0  broadcast 172.16.254.255
        ether 52:54:00:3d:43:d0  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
