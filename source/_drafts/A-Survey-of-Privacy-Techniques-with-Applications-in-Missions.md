---
title: A Survey of Privacy Techniques with Applications in Missions
tags:
- privacy
- missions
---

This post is motivated by the problems faced by missionaries in regions hostile to free, unsurveilled communication. By finding ways to circumvent this hostility we allow Kingdom-builders freedom in pursuing their missionary calls. In carrying out [the Great Commission](https://www.biblegateway.com/passage/?search=Matthew+28%3A16-20&version=NLT) this way, we open access to the Word of God and empower people through the freedom accompanied by anonymity and privacy. 

{% asset_img darkweb.jpg Scaaaaary! %}

<!-- more -->

What follows is an introductory survey of selected resources on the topics of VPNs and the Tor Proxy. The goal is to demystify some privacy tools currently at our disposal and provide direction on how to access the darkweb. An example darkweb Bible deployment is provided as a proof of concept. You will be able to read this Bible once you have installed a _Tor browser_. My Bible deployment is live on the darkweb now.


## First, what's the _darkweb_?

{% asset_img tor.jpg The Onion Router %}

When we talk about the _darkweb_, we're typically talking about _Tor_. From [vpnMentor](https://www.vpnmentor.com/blog/tor-vs-vpn/):

> Tor, short for The Onion Router, is free software that disguises your identity by encrypting your traffic and routing it through a series of volunteer-operated servers, known as nodes. When your traffic is received by the last node – the exit node – it is decrypted and forwarded to the website you’re visiting.

The Tor proxy allows you to access a website without revealing your IP address. It is easy for a website, government, internet service provider, or even friendly neighbour, to identify your location through your IP address. It also encrypts the information you are sending so that _almost_ no one can eavesdrop on your communication.

Tor is not foolproof, as one node in the Tor relay, the _exit node_, can potentially access your data. This problem can be mitigated with a VPN.


## What's a VPN?

{% asset_img vpns101-diagram.png Virtual Private Networking %}

> A Virtual Private Network (VPN) connects your device through a secure tunnel to a remote server in a country of your choice. This masks your IP address, making it appear as though you are accessing the internet from the location of the remote server instead of your actual location.

A VPN is different from Tor. When used in combination with Tor, data is encrypted so that it cannot be read by the exit node. In combining technologies like these, we seek to conceal our location and the fact that we're using Tor at all. Again, this is not foolproof, because even working in tandem, it seems that at least one third-party may be able to discern one (but not both) of these details. That said, a trusted VPN provider need not divulge your Tor use to anybody. Assuming sufficient means, providing confidential VPN services is something a church might provide to its missionaries.

## Practical darkweb deployment

From [CSO Online](https://www.csoonline.com/article/3287653/what-is-the-tor-browser-how-it-works-and-how-it-can-help-you-protect-your-identity-online.html):

> For most people reading this article, Tor Browser is completely legal to use. In some countries, however, Tor is either illegal or blocked by national authorities. China has outlawed the anonymity service and blocks Tor traffic from crossing the Great Firewall. Countries such as Russia, Saudi Arabia and Iran, are working hard to prevent citizens from using Tor. Most recently, Venezuela has blocked all Tor traffic.

Clearly, Tor by itself cannot mitigate the threat imposed by repressive regimes. We do not know the details of how places like China and Venezuela _block_ Tor traffic. In places like China, a darkweb Bible may still be accessible if deployed behind their firewall. But even if proven possible, the use of Tor still must be concealed.

In all this, there are many unknowns. The how-tos of deploying a darkweb server, finding a trusted VPN to conceal Tor use, and even getting Tor browsers on the phones and computers of people living under such regimes are yet to be determined. 

In light of the contextual difficulties, accessing and providing darkweb services is easy by comparison.


## Easy darkweb access

A trustworthy Tor browser can be obtained here: https://www.torproject.org/

I'm uncertain of availability across operating systems. There are Tor browsers available for Linux and Android. I use [Orfox/Orbot](https://guardianproject.info/apps/orfox/) on my Android phone.

{% asset_img check-tor-project.png The Tor Browser %}

Once you have a Tor browser installed, using it is not much different than using the ordinary World Wide Web. You can even still access your regular Web pages. One difference you will notice right away is speed. Using Tor takes longer than the regular Web.

Another difference: with Tor you can access _onion_ sites.

{% asset_img 04-donations.jpg QR code with onion address %}

The black squares are a QR code. If you have a QR reader on your phone, it's easy to copy the onion address onto your phone so you can paste it into your Tor browser. The QR code simply encodes this URL:

### http://h2hhjrw3nio35uah.onion/

An _onion_ address looks a bit like an ordinary URL, except instead of an easy-to-read domain name (like WhatDanDoes.info), you have a series of numbers and letters. If you were to click or copy/paste that URL into an ordinary browser, you would be told that _This site can't be reached_. You cannot access an onion site with an ordinary browser.

If you paste that URL into a Tor browser, it'll be a different story. That seemingly meaningless string of URL characters plays a key role in encrypting and decrypting your data so that the location of your onion site remains concealed.

{% asset_img bible-url.png URL in Tor browser %}

## Easy darkweb deployment

Okay, maybe it's not _easy_, but it is quickly repeatable once you've documented the process. Bear in mind, there is no VPN explicitly involved in this deployment. That is a topic of further research as resources become available.

The darkweb is _Web technology_. Though it is slower, you can still deliver Web applications over the darkweb. I've detailed deployment steps on my [GitHub repository](https://github.com/WhatDanDoes/nlt-bible-server). It takes a bit of know-how to deploy, but it is a process that is repeatable for even moderately savvy computer users. Certainly it is well within reach of missionaries on the ground. The onion address provided above (i.e., http://h2hhjrw3nio35uah.onion/) will take you to a live deployment of that service.

[{% asset_img github.png GitHub %}](https://github.com/WhatDanDoes/nlt-bible-server)

## Conclusion

This was a high-level overview of some basic anonymizing technology currently at our disposal. In terms of real-world privacy considerations, there is much more to be discussed. It is uncertain as to how best wield these tools, especially in repressive environments. Toward that end, there is a discussion of the role churches might play in providing anonymizing services to enable missionary work. VPN services and running Tor nodes are two possible ways churches can support missionaries working under hostile regimes. 
