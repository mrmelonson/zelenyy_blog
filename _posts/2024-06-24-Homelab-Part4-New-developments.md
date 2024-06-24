---
layout: post
title: Home Networking Part 4 - New developments
description: Alot has changed, this will be a few posts at least
summary: Home Networking Part 4 - New developments
#tags: code
---

My god it has been a minute. In my last blog post I mentioned the next time I would post I would have a NAS, well I didn't lie, but I also didn't expect it to take almost a year to get one then another year to bother writing a blog post about it! Ethier way here we are, another network diagram with a lot more servers and new network devices!
![Network diagram](/img/2024-06-24-Homelab-Part4-New-developments/diagram.png)

Alot has changed since the last post, for one I got a job at Cisco Meraki who do cloud-managed business networking equipment and as such I managed to snag a few things for free. But before I talk about the networking stuff, let’s talk servers!

## Fileserver

![What my fileserver looks like](/img/2024-06-24-Homelab-Part4-New-developments/fileserver.jpg)

This is my new fileserver, it boasts a ripping fast i3-12100F processor, 64GB of RAM, 5x 10TB hard drives and 10gig networking. Its then all enclosed within a Silverstone RM21-308 server chassis modded with Noctua fans to keep the noise levels down. TrueNAS was then the obvious choice for an operating system as I’m cheap and didn't want to pay for anything.

As you can tell, this my attempt at a more professional looking home-lab server. But does it perform well? Of course it does! You might be scoffing at the weak processor but in fact ZFS and TrueNAS use remarkably little CPU and I can easily pull 1GB/s when transferring locally. As you can see, I can transfer a small 5GB windows ISO in little under 6 seconds now!

![A gif showing off how fast my server is](/img/2024-06-24-Homelab-Part4-New-developments/transfer.gif)

So, what do I actually run on my server? Well SMB shares is the first choice as that allows me to add the NAS as a shared drive anywhere on my network (and even remotely if I use a VPN). After this, Plex was next as this is an easy way to watch all the definitely legally obtained movies and TV shows I store on my NAS, I then installed Nextcloud so I can have a fancy front-end for my shared drive much like google drive. But with both SMB and Nextcloud I would need user accounts and there was no way I was going to create duplicate accounts, so I didn't! Instead I just used Windows server 2019, installed active directory and used LDAP to authenticate both those accounts (I'll talk more about this in a future post).

Now with all that installed I'm done now right? Well you may have noticed that ISO upload was fast, like REALLY fast, like faster than my SSD fast? Wait that can't be right... right? Well I have 2 Samsung 860 EVO SSDs I used in RAID 0 as a boot drive for a while (I know younger me was clinically insane) which have now been downgraded to game storage but those only have a read/write speed of around 500MB/s. So theoretically if I set up my NAS as an iSCI drive then used that for game storage I would get both faster speeds and higher capacity right?

![A gif showing off how fast my server is](/img/2024-06-24-Homelab-Part4-New-developments/crystaldisk.png)

Yes, In fact the iSCI drive beats out the SSD in almost every stat except for random writes. But again, does it work? And yet again of course it does, I've been playing all of my steam games from my NAS for over 6 months now and I have had no problems with it, in fact my loading times have gotten better because of it! But in order to achieve this I need to have 10gig between my server and my desktop, so what does the networking look like?

## New Networking

![A picture of my untidy switch and router](/img/2024-06-24-Homelab-Part4-New-developments/switch.jpg)

In order to get 10gig networking I would need the following things: 2x SFP+ PCIE cards, 2x Direct Attach Copper cables, 1x switch (with SFP+). Now I can do 10gig networking! Except because of my need to be a "homelabber" I’ve set up my servers in a different VLAN to my desktop. This means it would need to be routed and this means it needs a router. This is normally no problem except I use a Meraki MX85 firewall which only has SFP (1gig) and RJ45 (1gig) LAN ports meaning there is a bottleneck. Luckily the switch I have is a Meraki MS225-24P which means it has Layer 3 routing!

If you've never set up L3 switching before, please go and understand L2 and L3 networking, I don't mean this in a bad way but L3 routing is actually a little complicated and I have seen so many people screw it up because they don't know what they're doing. But if you're still reading, here’s how to set it up.

For this recipe you'll need the following ingredients:
- 1x Transit VLAN
- Any amount of VLANs on your L3 switch
- Static routes from the router to the switch
- Default route from your L3 switch to your router

Now we can set up the following network:

![An example diagram of a simple L3 topology](/img/2024-06-24-Homelab-Part4-New-developments/L3.jpg)

(Of course, this is a very simple diagram with a simplified route table but it gets my point across.)

Steps:
1. Set up the transit VLAN. In this case I use VLAN 999 with a /30 range, any range works but there needs to be a single VLAN that both the router and the switch have a routeable interface on (in this case the router has .1 and the switch has .2).
2. Set up the VLANs on your L3 switch. In Cisco talk these are called SVIs (Switch Virtual Interface) which are routeable interfaces for the VLANs you plan to route between on the switch. In this case we create 4 VLANs each using .1 as the SVI.
3. Set up the static routes on the router. We create 4 static routes on the router for all 4 VLANs we just created which then route to the switch's SVI in the transit VLAN (in this case 192.168.99.2).
4. Set up the default route on the L3 switch to point to the router. In this case we create the 0.0.0.0/0 route to point to the router's interface in the transit VLAN (192.168.99.1).

If everything is set up correctly and you take a trace route it will look something like this: 192.168.10.1 -> 192.168.99.1 -> WAN. then the return will look like this WAN -> 192.168.99.2 -> 192.168.10.10 (client). Now it feels like nothing has changed when connecting to the internet, but now all the inter-VLAN routing is done on the switch! This means in my case I can do 10gig inter-VLAN routing without having to use the 1gig link back up to my router! If you wanna impress your network engineer friends you can now say you know how to configure a switch to provide east-west networking.

Anyways, that’s how I got my desktop hooked up with 10gig networking so I can play steam games from my NAS. That pretty much wraps up most of the new stuff I have on my network, I will try to make another post about Windows server and why I use it/what I use it for at a later date but for now thanks for reading!

