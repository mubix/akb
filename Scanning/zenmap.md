# Zmap Installation and Use

Installation
---------------

Info here: https://zmap.io/download.html

Clone the repo:

 * `git clone https://github.com/zmap/zmap.git`

Install Dependencies (Debian/Ubuntu)

 * `sudo apt-get install libgmp3-dev libpcap-dev gengetopt`

Install Not-mentioned dependencies:

 * `sudo apt-get install build-essential cmake flex byacc libcap2-bin libdumbnet-dev libevent-dev`


Then (inside the repo directory):

 1. `cmake ./`
 1. `make`
 1. `make install`
 1. `setcap cap_net_raw=ep /usr/local/sbin/zmap`


Running
-------------------
 
### Scan the entire internet for UDP 137

 `zmap -M udp -p 137 --probe-args=file:examples/udp-probes/netbios_137.pkt`
 
 
Other examples can be found here: https://github.com/zmap/zmap/tree/master/examples
