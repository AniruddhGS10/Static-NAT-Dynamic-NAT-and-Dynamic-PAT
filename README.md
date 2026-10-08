# Cisco Networking Lab | EVE-NG

A hands-on Cisco networking lab built using EVE-NG to practice IPv4 addressing, static routing, ACLs, NAT, PAT, and network troubleshooting in a multi-router topology.

## Lab Environment

- Network Emulator: EVE-NG
- Devices: Cisco IOS routers and VPCS hosts
- Packet Analysis: Wireshark
- Networking: IPv4, Static Routing, ACL, NAT, PAT, ICMP

## Network Topology

The lab consists of multiple LAN networks connected through Cisco routers and an ISP router.

The topology was built to practice routing, address translation, access control, and end-to-end connectivity testing.

![Network Topology](topology.png)

## Technologies Practiced

### IPv4 Networking

- IPv4 addressing
- Subnet masks
- Default gateways
- Network and host addressing
- Inter-router connectivity

### Static Routing

Static routes were configured to provide connectivity between different networks.

Static routing was verified using:

    show ip route

Connectivity was tested using:

    ping <destination>
    traceroute <destination>

### Access Control Lists

ACLs were configured to control which traffic was permitted through the router.

ACL configuration and operation were verified using:

    show access-lists

ACLs were also used as part of the NAT configuration to identify internal networks that should be translated.

## NAT and PAT

The lab includes multiple NAT configurations to understand how private IPv4 addresses are translated when communicating with external networks.

### Static NAT

Static NAT was configured to provide a one-to-one mapping between an internal host and a translated address.

Example:

    Inside Local       Inside Global
    10.2.2.1      ->   100.2.2.1

Configuration:

    ip nat inside source static 10.2.2.1 100.2.2.1

The configuration was verified using:

    show ip nat translations

### Dynamic NAT

Dynamic NAT was configured using a pool of available translated addresses.

General configuration:

    ip nat pool PUBLIC_POOL <START-IP> <END-IP> netmask <MASK>
    ip nat inside source list <ACL> pool PUBLIC_POOL

This allows eligible internal hosts to receive an address from the configured NAT pool.

### PAT / NAT Overload

PAT was configured to allow multiple internal hosts to share a single public IP address.

PAT uses TCP and UDP port numbers to distinguish between different connections.

Example:

    10.3.3.1:27633
            |
            v
    100.3.3.1:27633

    10.3.3.2:35265
            |
            v
    100.3.3.1:35265

PAT translations were verified using:

    show ip nat translations

## NAT Interface Configuration

The appropriate router interfaces were configured as NAT inside and NAT outside interfaces.

Example:

    interface g0/1
     ip nat inside

    interface g0/0
     ip nat outside

This allows the router to identify traffic entering from the internal network and traffic leaving toward the external network.

## Troubleshooting

The lab was used to practice troubleshooting connectivity and packet flow using Cisco IOS commands.

### Connectivity Testing

    ping <destination>

Used to verify basic reachability.

### Path Analysis

    traceroute <destination>

Used to identify the path taken by packets through the network.

### Routing Table

    show ip route

Used to verify static routes and determine how the router forwards traffic.

### NAT Translation Table

    show ip nat translations

Used to verify active NAT and PAT translations.

### ACL Verification

    show access-lists

Used to verify ACL entries and packet matching.

## Troubleshooting Example: Static NAT

During testing, Static NAT was initially configured using the router's gateway address instead of the intended internal host.

Initial mapping:

    10.2.2.10 -> 100.2.2.1

The router interface was:

    10.2.2.10

while the intended internal host was:

    10.2.2.1

The configuration was corrected to:

    ip nat inside source static 10.2.2.1 100.2.2.1

Connectivity was then verified using:

    ping
    traceroute

and the NAT table was checked using:

    show ip nat translations

This helped confirm the correct packet path and NAT translation.

## Packet Analysis

Wireshark can be used with the EVE-NG topology to capture and inspect network traffic.

Useful display filters include:

    icmp

    arp

    ip.addr == 10.2.2.1

    tcp

Packet analysis can be used to inspect:

- Source and destination IP addresses
- MAC addresses
- ICMP messages
- TCP and UDP ports
- TTL values
- Packet flow between network segments
- NAT and PAT traffic

## Lab Files

The repository contains the exported EVE-NG lab:

    NAT&PAT.zip

The lab can be imported into an EVE-NG environment containing the required Cisco IOS and VPCS images.

Cisco IOS images are not included in this repository. Users must provide their own legally obtained images.

## Learning Objectives

This lab was created to develop practical understanding of:

- IPv4 addressing
- Static routing
- Access Control Lists
- Static NAT
- Dynamic NAT
- PAT / NAT Overload
- NAT inside and outside interfaces
- NAT pools
- ACL based NAT
- Routing table verification
- NAT translation verification
- End-to-end connectivity testing
- Network troubleshooting using ping and traceroute

## Tools Used

| Tool | Purpose |
|------|---------|
| EVE-NG | Network emulation |
| Cisco IOS | Router configuration |
| VPCS | End host simulation |
| Wireshark | Packet capture and analysis |
| GitHub | Project documentation |

## Future Improvements

Possible future additions to this lab include:

- OSPF
- EIGRP
- VLANs
- Inter VLAN routing
- HSRP
- DHCP
- DNS
- Extended ACLs
- IPv6 routing
- BGP
- VPN configuration
- Firewall integration
- Detailed Wireshark packet analysis

## Author

**Aniruddh G Bhagwat**

Electronics and Communication Engineering Graduate

Network Security and Cybersecurity Learner


This project is intended for educational and laboratory purposes.

Cisco IOS software and images are proprietary to Cisco and are not included in this repository.
