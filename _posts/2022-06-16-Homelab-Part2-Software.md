---
layout: post
title: Home Networking Part 2 - Software
description: Second part of my home networking tour, talking about software
summary: Home Networking Part 2 - Software
#tags: code
---


Welcome to the second part of my series of posts surrounding my network setup. This part is all about software and the different services I host on my network and why. Like before, I’ll start with a diagram of my current network.


![Current network layout](/img/2022-06-16-Homelab-Part2-Software/Layout.png)

## Pi-hole
Currently on my network I only really have 2 servers doing anything, one is my Proxmox server and the other is my Raspberry Pi. As the header suggests, it currently runs Pi-hole on top of ubuntu and in my opinion Pi-Hole is one of the best things to do with a raspberry pi. Currently my pi blocks around 500,000 domains all from a few different blocklists (which I found [here](https://firebog.net/)). As well as blocking domains Pi-hole practically acts as a DNS server which means I can set internal domain names. This feature I use A LOT and it makes connecting to Proxmox and my VMs so much easier.

## Proxmox
The other server running on my network currently is my Proxmox box. I wish I had an interesting reason for choosing Proxmox but rather it was because a friend told me it was ok, which it is. As seen on my network diagram there are 2 VMs and a container which I run 24/7 but I also have other VMs powered off which I use occasionally for hosting game servers or for testing. One really cool feature of Proxmox is the fact that it gives each of my VMs and containers a unique IP address so I can assign DNS records super easily.

#### Fileserver
The first of the virtualised services within Proxmox is my fileserver. At the moment it hosts an SMB server for basic file transfer, a plex server so I can use it seamlessly with my Chromecast and finally an SSH server for access. I know that SMB isn’t the most secure protocol to use but I used it here because it is firewalled to only accept internal traffic and the low latency and increased performance with small files makes it the favourable choice for me as I stream movies and transfer a lot of documents at once. Plex was an obvious choice; I know there are other media players but plex is very streamlined and again I wanted something that works out of the box with my Chromecast. 
The next goal for my fileserver is to move it to a dedicated system with more drives so I can implement RAID in case one of my drives fail. This would also allow me to run TrueNAS on bare metal so I can better protect the system and not have to deal with virtualisation overhead.

#### Husky
You might look at my diagram and ask, “what the hell is husky?”, the answer to that is it is my rudimentary bastion box and general purpose VM. Although it does run a webserver (which was me mainly figuring out nginx configs) it also runs a dynamic DNS client and acts as a proxy so I can access my network remotely.
The way I have this configured is to only accept SSH connections from a specific static IP of a virtual private server which I rent. This way the only way to access my network remotely is to hop through 2 proxy servers. I also disabled password authentication and have only generated a total of 2 keys (one for my desktop, one for my laptop) which I believe to be very secure. If it isn’t however and I have made some security/network engineers very upset, please tell me!

#### Home assistant
The final service I host on my network is home assistant. Anyone who enjoys the luxury of smart home stuff has probably heard of home assistant, but for those who haven’t, it is basically a server which acts as a universal hub for smart devices which provides a web interface, automations, [etc](https://www.home-assistant.io). 
I currently have connected all my lights to it and set up some basic automations and scenes so I can turn on/off lights as well as set the colour with a single button press. Also mentioned, it also has a web interface which is customizable, so I have set it up to have my common scenes as well as controls for my lamp and basic weather updates.

![Current network layout](/img/2022-06-16-Homelab-Part2-Software/home_assistant.png)

## Conclusion
This concludes this episode of my home network tour. As stated before, my next goals are to get a dedicated NAS as well as to set up proper backups from my computer but also important files from my NAS to the cloud. That will probably be my next update but if something else comes up before then, there will likely be an update for it!



