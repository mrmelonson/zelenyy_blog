---
layout: post
title: Home Networking Part 1 - Hardware
description: First part of me talking about my home network
summary: Home Networking Part 1 - Hardware
#tags: code
---
Welcome to the first part of a series of posts that explains my current home network setup. In this post I will write about the hardware that makes my network tick. I'll start many of these parts with my current network diagram at the time of writing. For this post, I'll start with the router and go down the diagram explaining what hardware I have and why.

![Current network layout](/img/2022-06-06-Homelab-Part1-Hardware/Layout.png)


## Router

![TP-Link Archer VR400](/img/2022-06-06-Homelab-Part1-Hardware/router.jpg)

I'll start with the router I'm using. Its nothing special, a TP-Link Archer VR400 that a friend at work gave me. It is limited in some cases such as forcing me to use TP-Link's pre-configured VPN settings and only 3 choices of dynamic DNS providers, but otherwise it gets the job done. Right now I’ve configured it to be my DHCP server with a subnet prefix of 24 and configured the default DNS server to point to my Pi-Hole (I'll get into detail about in part 2).

## Managed Switch

![SG500-28 Managed Switch](/img/2022-06-06-Homelab-Part1-Hardware/switch.jpg)

My newest addition to my network, a used Cisco SG500-28 managed switch. It boasts 24 gigabit RJ-45 ports and 4  5G SFP ports. I bought it mainly because I want to start experimenting with setting up and using a managed switch and I intend to use VLans to experiment with DMZs and creating other separate networks and 24 ports will be more than enough ports to do so. As for configuration, I haven't done much other than changing the default password and assigning the DNS record "dogpark.dognet".

## Raspberry Pi 4

![Raspberry Pi 4](/img/2022-06-06-Homelab-Part1-Hardware/raspberry.jpg)

Next in the diagram is my Raspberry Pi 4. As you may notice it is currently sitting on top of a book to make sure the bottom solder traces don't short on the metal case under the book (Don't worry I am going to get a case for it). Right now it's running ubuntu server and its purpose is to host pi-hole.

## Custom Built Server

![Custom built server](/img/2022-06-06-Homelab-Part1-Hardware/server.jpg)

Now for my pride and joy, my custom built server. Although I say *"custom built"* I really mean more *"Frankenstein’s server"*. It features an i5-8600k processer, 16gb of RAM, a 4TB hard drive and 2 500gb SSDs, the CPU, motherboard and RAM were left over from me upgrading my gaming PC, the 4TB drive is a shucked desktop hard drive, the SSDs I found around the house and it's all housed in a 4u server chassis.

As you can see from the network diagram, I use it for most of the services hosted on my network. I use proxmox for all the virtualisation and ZFS as the filesystem.

## Tradfri

![Tradfri](/img/2022-06-06-Homelab-Part1-Hardware/tradfri.jpg)

I have a tradfri hub from Ikea so all my smart lights can work. I use it because it works almost flawlessly with home assistant and does not require a cloud service to work. The tradfri series of smart devices are also ZigBee based so in the future if I wanted to use other ZigBee devices I could use just a single ZigBee controller.

## Conclusion

So that concludes the tour of the hardware that makes up my home network. In the next part I will talk about the operating systems I use and the services I host on my network. Until then, enjoy this picture of my networking setup. You might also notice the blue computer case and another raspberry pi, the case is empty and the other raspberry pi is for a future project

![Custom built server](/img/2022-06-06-Homelab-Part1-Hardware/setup.jpg)
