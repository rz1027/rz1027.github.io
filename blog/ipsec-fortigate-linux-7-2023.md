---
layout: post
title: "Connected to Fortigate IPSec using Strongswan"
author: Rz1027
date: 2023-07-03
---

# Background

IPSec (Internet Protocol Security) is a protocol suite for securing Internet Protocol (IP) communications by authenticating and encrypting each IP packet in a data stream. It is primarily used to create encrypted tunnels between secure and insecure networks.

If you are in an internal penetration test or just a Linux user that needs to jump into your company VPN, IPSEC might be the only available option. And for a Linux user like me, sometimes the option is not there as the case of Fortigate, who don't support IPSEC from the Linux version of Forticlient VPN. I had to jump back and forth in Window's VMs to connect to my company's network, just because Fortigate don't want to add this simple option. 

To solve this problem we need to connect to the VPN service using lower level softwares. A lot are free and open source like openswan and vpnc, but for me I am gonna showcase Strongswan.

# The Process

I wont dive a lot into the "Networking" theory of IPSEC, I'll just go with the technical point of view and what each parameter means. 

In my case I was connecting to a gateway using IPSEC IKEv1, where the types of authentication are PSK (preshared key) and XAUTH (extended authentication with a username and password). Thus my credentials will be:

* A gateway ip : lets say 123.123.123.123
* A preshared key : ABCDEFG
* A username and password pair -> username:password
* A local ID 

The local ID is an extra piece of data sent in negotiation, it is used by gateway for more verification especially when multiple users on the same ip. (You might not have this in your configuration)

This is what **Windows** version of forticlient IPSEC configuration might look like:

![Windows Forticlient IPSEC](https://rz1027.github.io/assets/images/ipsec.png)

We need to translate such information into strongswan configuration.
To install strongswan you can use:
`pamac install strongswan` on Arch based systems
`sudo apt-get install strongswan` on Ubuntu based systems

Strongswan has 2 important files:

*`/etc/ipsec.conf` 
```shell
conn snt
        left=10.11.11.1
        leftid= chocolate
        leftsubnet=10.0.1.0/24
        right=192.168.22.1
        rightsubnet=10.0.2.0/24 
        keylife=80000s
```

*`/etc/ipsec.secrets`



