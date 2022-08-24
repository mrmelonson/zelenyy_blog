---
layout: post
title: Home Networking Part 3 - VPN, Access Points and Routers
description: Updates to my network, a server cabinet, VPN, Access Point and a new router
summary: Home Networking Part 3 - VPN, Access Points and Routers
#tags: code
---

Welcome back to another episode of me boasting about my home networking setup! It's been a while since the last part and I’m glad to announce that there have been some relatively significant changes to how its organised. Firstly, I shall present my updated network diagram, then continue on to the first item.

![Network diagram](/img/2022-08-12-Homelab-Part3-VPN-adventures/Network.png)

## Server Cabinet

I'll start with the most visually different update to my setup which is the addition of a 24U server cabinet. 

![Server Cabinet](/img/2022-08-12-Homelab-Part3-VPN-adventures/server_cabinet.jpg)

































































The first thing you might think is why would someone like me buy a server cabinet? To answer your question, it was free! A friend of mine was trying to get rid of it which was really convenient as I was actually looking to possibly buy a small rack. So, me taking it was a win for both of us (Plus she threw in some rack-mount power strips and shelves!). The second question you may have is, “Why did you need a rack?”. This question was asked almost immediately by all of my friends as soon as I told them about it. Although I didn't actually need a rack, it really does help with organisation of my gear and how the room looks and low and behold, all my friends agreed that the room looks a lot better with a nice cabinet rather than a stack of gear atop a box. In addition to looks, it would help in maintenance as I can slide individual servers out which will become important once I get a new server, as well as that it helps with sound isolation.

NOTE: *If you happen to be reading this and are looking to buy a server cabinet, One thing I found out the hard way is that 2 of the measurements (height and width) are standardised, However depth is NOT standardised and thus make sure you double check your servers and gear can fit into the rack before buying it!*


## New Router

The next item on the agenda is a new router, you may have even seen it in the previous photo, and it is my new router! Specifically, it is a TP-Link Omada ER605 router.
























![omada er605 router](/img/2022-08-12-Homelab-Part3-VPN-adventures/router.jpg)
This was a bit of an upgrade from my old router and allows some more advanced configurations such as VLANs and VPNs. As well as those features, I can also set up the Omada software on a server and monitor my network through that. But the main reason I bought the router was because I wanted my router closer to my network so I could actually do VLANs with multiple patch cables going into my switch instead of running separate longer cables around my apartment to my old router.

## VPN
Along with the basic setup of my router I also configured a VPN for my network. Luckily my router supports client-to-network VPNs with some different protocols, that being IPsec, L2TP, PPTP and OpenVPN. At first, I set up L2TP/IPsec which seems like the best option for my use case. Specifically, my use case would be really only 2 things:

1. Client-to-network tunnelling
2. The ability to resolve internal DNS queries

L2TP/IPsec worked well on my laptop as within the router I was able to set up an account for my laptop and specify the DNS server. Along with DNS it was also built into windows already, so I didn't need to install special clients. The only problem with this setup was that when I tried to set up the connection on my phone, although it does have the ability to set up VPN connections, from android 12 onwards L2TP and some older protocols were removed which meant I couldn't connect on my phone.

This made me switch to OpenVPN which although was not built into windows nor android, it did have official clients. On the router end, the setup was a little more complicated as for some weird reason it seems that OpenVPN was an afterthought on TP-Link's half, as you are able to configure VPN IP pools which L2TP can use, but within the config for OpenVPN you need to manually specify the range with a subnet prefix. Though despite that oddity, it was relatively painless to configure, and the router even generates keys and the client config file for me to import on my laptop and phone!

However, there were some more issues with OpenVPN I needed to iron out. Because of the lack of options on my router, I had to manually edit the client confirmation file in order to add my PiHole as the default DNS server and replace the ip address with my dynamic DNS domain. Once the extra configuration was done and the file imported onto my remote devices, I am now able to access my internal services such as my fileserver and my home assistant instance without needing to expose extra ports to WAN!

## Access Point

The last thing I added since the last update was an access point. 

![DLink Viper 2600](/img/2022-08-12-Homelab-Part3-VPN-adventures/dlink_ap.jpg)

I added this for 2 reasons, the first is I wanted access point because I bought a VR headset that can use airlink which works over Wi-Fi, And the second reason was that after I converted my old router into a media converter this was the only access point. Again, the main question you may ask is "What was wrong with your old router?". Firstly, as I literally just said, my old router is now just a media converter. The second reason is my old router had a combined link rate of 1200Mbit/s which is usually distributed to 300 on 2.4GHz and 867 on 5GHz. My new router is a DLink Viper 2600 which is twice the combined link rate meaning that I can theoretically get up to 800Mbit/s on 2.4Ghz and 1,733Mbit/s on 5GHz which is more than enough for airlink.

## Conclusion

Thank you again for reading my semi-coherent ramblings about my ever-growing network setup. I know in the last episode I said my next project was going to be a dedicated NAS, but trust me this time, the next update should be the NAS so stay tuned for that!
