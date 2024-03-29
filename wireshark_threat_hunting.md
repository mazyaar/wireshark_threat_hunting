<h1 align="center">  WireShark </h1>
                
> <h1 align="center">Basic Filters 👋</h1>

```javascrip
import copyCodeBlock from '@pickra/copy-code-block';

(http.request or tls.handshake.type eq 1) and !(ssdp)
(http.request or tls.handshake.type eq 1 or tcp.flags eq 0x0002) and !(ssdp)
(http.request or tls.handshake.type eq 1 or tcp.flags eq 0x0002 or dns) and !(ssdp)
(http.request or http.response or tls.handshake.type eq 1) and !(ssdp)
```
```
Go on Preference -> Columns -> add new -> Title: Host Type:Custom-> field: 
http.host or tls.handshake.extensions_server_name
```
------------------------------------------------------------------------------------------------------

                
><h1 align="center">Find Detatils :zap: </h1>           

>dhcp: To finding Dhcp Request

>nbns: To finding NetBIOS Name Service

>smb: to FInding Server Message Block

>kerberos.CNameString ( then filter this string): To finding Cname and Users

>udp and !dns: search udp and not dns

>ftp.request.command or ftp-data : To finding FTP Request

>smtp: for find pop protocol on smtp protocol on mail service

>tcp.port eq 110 and tcp.flags eq 0x0002 : hese attempted connections can be revealed by including TCP SYN segments in your filter and ports

#### file transfer over SMB: smb filter and Export SMB object from file -> export object -> smb

##### tor Browser:
>Decode ftom tls to tcp in Wireshark:
Anlyze -> Decode As -> add entry Field: tcp port value: 8080 Current: TLS

wireshark filter to search for certificate issuer:

```
tls.handshake.type eq 11
```

find tcp port and sync :
```
tcp.port eq 2555 and tcp.flags eq 0x0002
```

--------------------------------------------------------------------------
><h1 align="center">Most usefull Wireshark Filters :zap: </h1>   


###### 1. Display traffic to and from 192.168.65.129
```
ip.addr == 192.168.65.129
```
###### 2. Display tcp and dns packets both
```
tcp or dns
```
###### 3. Display traffic with source or destination port as 443
```
tcp.port == 443
```
###### 4.Display tcp flags
```
tcp.analysis.flags
```
###### 5. display all protocols other than arp, icmp and dns 
```
!(arp or icmp or dns)
```
###### 6. Show traffic which contains google
```
tcp contains google
```
###### 7. Display http response code of 200 in network traffic
```
http.response.code == 200
```
Display http request 
```
http.request
```
###### 9.Display tcp.flags.syn
```
10. 9. tcp.flags.syn
```
###### 10. Show only SMTP (port 25) and ICMP traffic:
```
tcp.port eq 25 or icmp
```
###### 11. Show only traffic in the LAN (192.168.x.x), between workstations and servers -- no Internet:
```
ip.src==192.168.0.0/16 and ip.dst==192.168.0.0/16
```
###### 12. TCP buffer full -- Source is instructing Destination to stop sending data
```
tcp.window_size == 0 && tcp.flags.reset !=1
 ```
###### 13.Filter on Windows -- Filter out noise, while watching Windows Client - DC exchanges
 ```
 smb || nbns || dcerpc || nbss || dns
```
###### 14. ! ( ip.addr == 192.168.65.129 )
which is equivalent to
 ```
 ! (ip.src == 192.168.65.129 or ip.dst == 192.168.65.129)
```
This translates to "pass any traffic except with a source IPv4 address of 192.168.65.129 or a destination IPv4 address of 192.168.65.129"

###### 15.Some filter fields match against multiple protocol fields. For example, "ip.addr" matches against both the IP source and destination addresses in the IP header. The same is true for "tcp.port", "udp.port", "eth.addr", and others. It's important to note that
 ```
ip.addr == 192.168.0.100
```
is equivalent to
```
ip.src == 192.168.0.100 or ip.dst == 192.168.0.100
```
###### 16. Filter out any traffic to or from 10.43.54.65
```
ip.addr != 192.168.0.100
```
* which is equivalent to
```
ip.src != 192.168.0.100 or ip.dst != 192.168.0.100
```

* Wireshark Filter HTTP GET Request
```
http.request.method == “GET”
```
* Wireshark Filter HTTP POST
```
http.request.method == “POST”
```
* Wireshark Filter Website URL
```
http.host == "exact.name.here"
```
```
http.host contains "partial.name.here"
```
* Wireshark Filter by Time (Timestamp)
```
* frame.time >= "July 14, 2018 18:04:00" && frame.time <= "July 14, 2018 18:40:00"
```

* Wireshark Filter Packet Number
```
frame.number == 500
```
* Wireshark Filter SYN
```
tcp.flags.syn == 1
```
* This filter will show both the TCP packets containing SYN and SYN/ACK. If you only want SYN you can use
```
tcp.flags.syn == 1  and tcp.flags.ack == 0
```
* Wireshark Ack Filter
```
tcp.flags.ack == 1
```
* Wireshark Syn Ack Filter
```
tcp.flags.syn == 1
```
* Wireshark RST Filter
```
tcp.flags.reset == 1
```
* Wireshark Beacon Filter
```
wlan.fc.type_subtype = 0x08
```
Wireshark Broadcast Filter
```
eth.dst == ff:ff:ff:ff:ff:ff
```
Wireshark Multicast Filter
```
(eth.dst[0] & 1)
```
* This will show multicast and broadcast. Since broadcast is a type of multicast it’s a valid expression. If you don’t want any broadcast multicast results you can use:
```
(eth.dst[0]&1) && !(eth.dst == ff:ff:ff:ff:ff:ff)
```
```
eth[0x47:2] == 01:80 [This is an example of an offset filter. It sets a filter for the HEX values of 0x01 and 0x80 specifically at the offset location of 0x47]  
```
```
tcp.analysis.flags && !tcp.analysis.window_update [displays all retransmissions, duplicate acks, zero windows, and more in the trace. Helps when tracking down slow application performance and packet loss. It will not include the window updates, since these aren't really important for me to see in most cases.] 
```


---------------------------------------------------------------------------
		

<h1 align="center">Filters :zap: </h1>



| No | Filter | Meaning 
|----|------------|-------|
| 1  |ip.addr ip.src ip.dst ip.host| Filter by IPv4
| 2  | ipv6.addr ipv6.src ipv6.dst | Filter by IPv6
| 3  | tcp tcp.port tcp.dstport tcp.srcport | TCP Filters
| 4  | tcp tcp.port tcp.dstport tcp.srcport | UDP Filters
| 5  | arp arp.src arp.dst arp.dst.hw_mac| ARP filters
| 6  | icmp icmpv6 icmp.type | ICMP filters
| 7  | eth eth.addr eth.dst eth.src | Ethernet Filters
| 8  | http http2 http.host http.request  | Http Filters 
| 9  | http.content_type | HTTP protocol
| 10 | sip rtp rtcp raw_sip iax2 | Filters for IP telephony traffic
| 11 | rdp | Remote Desktop Protocol
| 13 | vnc | Virtual Network Computing
| 14 | l2tp | Layer 2 Tunneling Protocol
| 15 | ldap | Lightweight Directory Access Protocol
| 16 | openvpn | OpenVPN Protocol
| 17 | ppp | Point-to-Point Protocol
| 18 | pppoe pppoed pppoes: | PPP-over-Ethernet
| 19 | pptp | Point-to-Point Tunneling Protocol
| 20 | smtp imap pop| Mail application protocols
| 21 | ftp tftp uftp  | File Transfer Protocol
| 21 | ssh  | Secure Shell




## Below are the most popular examples of Wireshark filters:

>### Filters by IP address:

```
ip.addr == 192.168.30.0/24
```
```
!(ip.addr == 192.168.30.1)
```
```
ip.src == 192.168.11.22 && ip.dst == 192.168.22.11
```
```
ip.addr => 172.16.10.10 && ip.addr <= 172.16.10.50
```

>### DNS name filters:

```
tcp contains "poweradm.com"
```
```
http.host == "blog.google.com" (looking up the exact value in the HTTP headers)
```
```
http.host contains "poweradm.com" (search for content in HTTP headers)
```
```
http.host (all packets with the host field in the HTTP header )
```

>### Filters by TCP or UDP ports:

```
tcp.port == 443
```
```
tcp.dstport == 80
```
```
udp.srcport == 53
```
```
tcp.dstport>=8000 && tcp.dstport<=8180
```
```
DHCP Filters:
```
```
udp.dstport == 67
```
```
bootp.option.dhcp
```

>### Filters by MAC address:

```
eth.src == 00:2a:3b:4e:5c:6d
```
```
eth.dst == 00:2a:3b:4e:5c:6d
```

>### Content filters:

```
http.content_type contains "jpeg"
```
```
http.content_type contains "image"
```
```
http.content_type contains "xml"
```
```
http.request.uri contains "rar"
```

>### Filters for ICMP:

```
icmp
```
```
icmp.type==0(ping responses )
```
```
icmp.type==3 (destination unreachable message)
```

>### HTTP Header Filters:
```
http.content_type == "text/plain" (by the value of the content-type field )
http.request.method == "POST"
http.request.method == "GET"
http.response.code == 404 (by HTTP response code )
http.response.code != 200 (by HTTP response code )
http.server == "nginx" (by the value of the server field )
```

>### Filters for analyzing SIP traffic:

```
sip
rtp
rtcp
rtpevent
udp.srcport >= 10000 && udp.srcport <= 20000
udp.port == 5060 || tcp.port == 5060
```
-----------------------------------------------------------


