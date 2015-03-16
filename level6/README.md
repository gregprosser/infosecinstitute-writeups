# Level 6

The web page for this level offers nothing except a `sharkfin.pcap` file 

## Solution

PCAP is a file format used to store capture files saved from TCPDUMP, a network sniffing utility.

We can open this file in [Wireshark](https://www.wireshark.org/), a GUI for analyzing captures (and creating them).

Opening up this capture file, we can see a number of different types of traffic - but a vast majority of it is undecipherable because it uses SSL and/or TLS, which are encrypted protocols.  If we add the filter `!(tcp.port == 443)` we can filter out this encrypted traffic.

After we do so, we are left with the following data:

- ARP traffic: this is layer 2 signalling information and doesn't seem to contain anything interesting
- DHCPv6 requests: these appear to be typical, and although the Client Identifier's look interesting, this looks too normal
- SNMP requests: this is a network management protocol, but the community string (i.e. the password) used is 'public', which probably isn't a flag
- HTTP traffic: all the HTTP traffic appears to be either keepalives, or connection shutdowns - no useful data
- DNS traffic: requests for hostnames which doesn't appear to be visible from the outside world, and return no response

However, I left out one important frame: frame 0 contains UDP traffic for port 139, a common port associated with Windows File Sharing/NetBIOS traffic.  Interestingly, Wireshark doesn't offer to decode the data stream in this packet, which seems to indicate it doesn't look like typical NetBIOS traffic.

Sure enough, if we take the "Data" segment of this packet, and pull it apart, we can find our flag:

```python
>>> s = '696e666f7365635f666c616769735f736e6966666564'
>>> hexes = [s[i:i+2] for i in range(0, len(s), 2)]
>>> hexes
['69', '6e', '66', '6f', '73', '65', '63', '5f', '66', '6c', '61', '67', '69', '73', '5f', '73', '6e', '69', '66', '66', '65', '64']
>>> ''.join([chr(int(x, 16)) for x in hexes])
'infosec_flagis_sniffed'
>>>
```

## Flag

`infosec_flagis_sniffed`
