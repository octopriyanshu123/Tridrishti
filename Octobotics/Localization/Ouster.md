
Ouster Need to connect to the Using the Lan and turn on the local link

ip of the Ouster is 162.254.65.90 
set the ip of the control pc ```ifconfig``` 162.254.188.188
port if the imu ```8854```
port of the point cloud ```4545```

```tcp dump to check the data is comming to the pc or not```

**

Enp39s0  
  

Check the Data is coming on  
sudo tcpdump -i enp39s0 port 37807

sudo tcpdump -i any src host 169.254.63.90 and src port 55627 and dst host 169.254.118.118 and dst port 37807  
  

sudo tcpdump -i enp39s0 port 57571

sudo tcpdump -i any src host 169.254.63.90 and src port 52072 and dst host 169.254.118.118 and dst port 57571  
  
  

  

/*

  

[1] Connecting to sensor 169.254.63.90  (UDP dest -> 169.254.118.118) ...

[2026-04-29 13:11:42.608] [ouster::sdk::core] [info] initializing sensor client: 169.254.63.90 expecting lidar port/imu port: 0/0

[2026-04-29 13:11:42.608] [ouster::sdk::core] [info] (0 means a random port will be chosen)

[1] Connected.

[2] Fetching metadata ...

[2] Sensor : OS-1-32-U1  W=1024  H=32

[3] Lidar packet size: 6400 bytes

[4] Waiting for data  (Ctrl-C to stop) ...

  
  
  

UDP Destination Address

169.254.118.118

UDP Port Lidar

7502

UDP Port IMU

7503

  
  
  
  
  

Cursor ID Category Level Msg Msg Verbose Realtime

0   0x01000011  ETHERNET_LINK_BAD   WARNING Sensor has detected an issue with the connected ethernet link. Please check the network setup including the network switch and harnessing can support 1 Gbps Ethernet.  Link transitioned to 100/Full   2547537969382

18  0x01000016  UDP_TRANSMISSION    WARNING Could not send lidar data UDP packet to host. Check that network is up and the destination is reachable.    Failed to send lidar UDP data to destination host 169.254.118.118:7502  3073646663089

17  0x01000019  UDP_TRANSMISSION    WARNING Could not send IMU UDP packet to host; check that network is up and the destination is reachable.   Failed to send imu UDP data to destination host 169.254.118.118:7503    3072654166524

  
  
  
  

// ip addr show | grep -B2 "169.254.118.118"

  

// 2: enp39s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000

  

//     link/ether 2c:f0:5d:83:4b:d4 brd ff:ff:ff:ff:ff:ff

  

//     inet 169.254.118.118/16 brd 169.254.255.255 scope link noprefixroute enp39s0

  
  
  

// ethtool enp39s0 | grep -E "Speed|Duplex|Link"

  

// ethtool enp39s0 | grep -E "Speed|Duplex|Link"

  

// netlink error: Operation not permitted

  

//         Link partner advertised link modes:  10baseT/Half 10baseT/Full

  

//         Link partner advertised pause frame use: No

  

//         Link partner advertised auto-negotiation: Yes

  

//         Link partner advertised FEC modes: Not reported

  

//         Speed: 100Mb/s

  

//         Duplex: Full

  

//         Link detected: yes

  
  
  

// sudo ethtool -s enp39s0 speed 1000 duplex full autoneg on

// ip addr show | grep -B2 "169.254.246.120"

  
  
  

// ethtool enp38s0 | grep Speed   # must say 1000Mb/s

  
  

// */

  
  

/**

* Ouster LiDAR - Raw Data Reader (No ROS)

* Compatible with Ouster SDK >= 0.16 (modern API)

*

* Sensor IP : 169.254.63.90

* PC IP     : 169.254.118.118

* Interface : enp38s0

*

* Build: see CMakeLists.txt

* Run  : sudo ./build/ouster_raw   (SO_BINDTODEVICE needs root)

*/

  
  
  

//cmake --build . --target live_lidar

  
  

terminate called after throwing an instance of 'std::invalid_argument'

 what():  FieldView: ineligible dereference type for field of element type UINT16. Dereference type must match or be void.

Aborted (core dumped)

  
  
  

/**

* Ouster LiDAR - Raw Data Reader (No ROS)

* Compatible with Ouster SDK >= 0.16 (modern API)

*

* Sensor IP : 169.254.63.90

* PC IP     : 169.254.118.118

* Interface : enp38s0

*

* Build: see CMakeLists.txt

* Run  : sudo ./build/ouster_raw   (SO_BINDTODEVICE needs root / CAP_NET_RAW)

*/

  
  
  
**