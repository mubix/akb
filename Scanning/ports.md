# Ports

## MSF Port Easy Copy List

```
1,7,9,13,19,21-23,25,37,42,49,53,69,79-81,85,105,109-111,113,123,135,137-139,143,161,179,222,264,384,389,402,407,443-446,465,500,502,512-515,523-524,540,548,554,587,617,623,689,705,771,783,888,902,910,912,921,993,995,998,1000,1024,1030,1035,1090,1098-1103,1128-1129,1158,1199,1211,1220,1234,1241,1300,1311,1352,1433-1435,1440,1494,1521,1530,1533,1581-1582,1604,1720,1723,1755,1811,1900,2000-2001,2049,2100,2103,2121,2199,2207,2222,2323,2362,2380-2381,2525,2533,2598,2638,2809,2947,2967,3000,3037,3050,3057,3128,3200,3217,3273,3299,3306,3389,3460,3500,3628,3632,3690,3780,3790,3817,4000,4322,4433,4444-4445,4659,4679,4848,5000,5038,5040,5051,5060-5061,5093,5168,5247,5250,5351,5353,5355,5400,5405,5432-5433,5498,5520-5521,5554-5555,5560,5580,5631-5632,5666,5800,5814,5900-5910,5920,5984-5986,6000,6050,6060,6070,6080,6101,6106,6112,6262,6379,6405,6502-6504,6542,6660-6661,6667,6905,6988,7001,7021,7071,7080,7144,7181,7210,7443,7510,7579-7580,7700,7770,7777-7778,7787,7800-7801,7879,7902,8000-8001,8008,8014,8020,8023,8028,8030,8080-8082,8087,8090,8095,8161,8180,8205,8222,8300,8303,8333,8400,8443-8444,8503,8800,8812,8834,8880,8888-8890,8899,8901-8903,9000,9002,9080-9081,9084,9090,9099-9100,9111,9152,9200,9390-9391,9495,9809-9815,9855,9999-10001,10008,10050-10051,10080,10098,10162,10202-10203,10443,10616,10628,11000,11099,11211,11234,11333,12174,12203,12221,12345,12397,12401,13364,13500,13838,14330,15200,16102,17185,17200,18881,19300,19810,20010,20031,20034,20101,20111,20171,20222,22222,23472,23791,23943,25000,25025,26000,26122,27000,27017,27888,28222,28784,30000,30718,31001,31099,32764,32913,34205,34443,37718,38080,38292,40007,41025,41080,41523-41524,44334,44818,45230,46823-46824,47001-47002,48899,49152,50000-50004,50013,50500-50504,52302,55553,57772,62078,62514,65535
```

Discovery Ports
-------

This is a list of common ports that will give you a pretty good list of "alive" system when scanning internally or externally.

 * easy copy - `21,22,23,25,139,443,445,631,3389,6000-6009,8080,8000,8443`
 * FTP: 21
 * SSH: 22
 * Telnet: 23
 * SMTP: 25
 * Finger: 79
 * HTTP: 80
 * Kerberos: 88
 * POP3: 110
 * SUNRPC (Unix RPC): 111 (think: rpcinfo)
 * NetBIOS: 139
 * IMAP 143
 * LDAP: 389
 * HTTPS: 443
 * LotusNotes: 1352
 * Microsoft DS: 445
 * RSH: 514
 * CUPS: 631
 * NFS: 2049
 * Webrick(Ruby Webserver): 3000
 * RDP: 3389
 * Munin: 4949 
 * SIP: 5060
 *PCAnywhere: 5631 (5632)
 * NRPE (*nix) /NSCLIENT++ (win): 5666 (evidence of Nagios server on network)
 * Alt-HTTP: 8080
 * Alt-HTTP tomcat: 9080
 * Another HTTP: 8000 (mezzanine in development mode for example)
 * Nessus HTTPS: 8834
 * Proxmox: 8006
 * Splunk: 8089 (also on 8000)
 * Alt HTTPS: 8443
 * vSphere: 9443
* X11: 6000-6009 (+1 to portnum for additional displays) (see xspy, xwd, xkey for exploitation)
* VNC: 5900, 5901+ (Same as X11; +1 to portnum for each user/dipslay over VNC. SPICE is usually in this range as well)
Printers: 9100, 515
 * Dropbox lansync: 17500


UDP Discovery
----------------

 * easy copy - `53,123,161,1434`
 * DNS: 53
 * XDMCP: 177 (via NSE script --script broadcast-xdmcp-discover, discover nix boxes hosting X)
 * OpenVPN: 1194
 * MSSQL Ping: 1434
  * SUNRPC (Unix RPC): 111 (yeah, it's UDP, too)
 * SNMP 161
 * Network Time Protocol (NTP): 123 
 * syslog : 514
 * UPNP: 1900
* Isakmp - 500 (ike PSK Attack)
* vxworks debug: 17185 (udp)


Authentication Ports
------------------------

 * easy copy - `1494`
 * Citrix: 1494
 * WinRM: 80,5985 (HTTP), 5986 (HTTPS)
 * VMware Server: 8200, 902, 9084
 * DameWare: 6129

Easy-win Ports:
------------------------

* Java RMI - 1099, 1098
* coldfusion default stand alone - 8500
* IPMI UDP(623) (easy crack or auth bypass)
* 6002, 7002 (sentinel license monitor (reverse dir traversal, sometimes as SYSTEM))
* GlassFish: 4848
* easy copy - `9060`
* IBM Web Sphere: 9060
* Webmin or BackupExec: 10000
* memcached: 11211
* DistCC: 3632
* SAP Router: 3299

Database Ports
--------------------

 * easy copy - `3306,1521-1527,5432,5433,1433,3050,3351,1583,8471,9471`
 * MySQL: 3306
 * PostgreSQL: 5432
 * PostgreSQL 9.2: 5433
 * Oracle TNS Listener: 1521-1527
 * Oracle XDB: 2100
 * MSSQL: 1433
 * Firebird / Interbase: 3050
 * PervasiveSQL: 3351, 1583
 * DB2/AS400 8471, 9471
 * Sybase 5000
 
NoSQL Ports
---------------------

Reference: [Abusing NoSQL Def Con 21](references/DEFCON-21-Chow-Abusing-NoSQL-Databases.pdf)

 * easy copy - `27017,28017,27080,5984,900,9160,7474,6379,8098`
 * MongoDB: 27017,28017,27080
 * CouchDB: 5984
 * Hbase 9000
 * Cassandra:9160
 * Neo4j: 7474
 * Redis: 6379
 * Riak: 8098

SCADA / ICS
-------------------

source: http://www.digitalbond.com/tools/the-rack/control-system-port-list/ )

* BACnet/IP: UDP/47808
* DNP3: TCP/20000, UDP/20000
* EtherCAT: UDP/34980
* Ethernet/IP: TCP/44818, UDP/2222, UDP/44818
* FL-net: UDP/55000 to 55003
* Foundation Fieldbus HSETCP/1089 to 1091, UDP/1089 to 1091
* ICCP: TCP/102
* Modbus TCP: TCP/502
* OPC UA Binary: Vendor Application Specific
* OPC UA Discovery Server: TCP/4840
* OPC UA XML: TCP/80, TCP/443
* PROFINET: TCP/34962 to 34964, UDP/34962 to 34964
* ROC PLus: TCP/UDP 4000

Interesting Port Ranges
----------------------------

* HTTP(S) Ports: 8000-9000


Script:
`awk '$2~/tcp$/' nmap-services | sort -r -k3 | head -n 1000 # same for udp`

