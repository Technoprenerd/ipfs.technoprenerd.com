---
title: "Creation of blog on IPFS"
date: 2020-05-14T13:37:00+02:00
draft: false
toc: false
images:
tags:
  - ipfs
  - web 3.0
  - fleek
series:
  - first
---

Welcome to my blog on Cyber Security and Blockhain. During the past few years I've been following the IPFS project, which is interesting to say the least. In this post I'll describe what IPFS is, how it operates and which tools are available. This in order to get you started with IPFS.


<a href="https://ipfs.io/">
{{< image src="/img/posts/hello-world/ipfslogo.png" alt="IPFS logo" position="center" style="border-radius: 8px; width:50%; height:50%;" >}}
</a>

<h2>What is IPFS</h2>

IPFS stands for [Inter-Planetary File System](https://ipfs.io/), which is a decentralized network of content. With the Internet, we are used to going where we want to go and the location is known <i>where</i> to look for content (for example the image of meme.jpg is on examplewebsite.com/meme.jpg). 

However, as the Internet has become a global phenomenon, this isn't quite scalable. Since every time meme.jpg is downloaded, it puts more load on the website and therefore more resources (harddisk, bandwith, etc) are needed. 
Once meme.jpg is very popular, the website that handles the requests needs to scale and provide the image very fast to the user.

For this, Content Delivery Networks (CDN's) came into existance and spread the meme.jpg image between different geographic locations, so that the user can download the image faster.
However, this isn't quite sustainable and scalable either, since every meme.jpg picture needs to reside on different CDN's all around the world.

Now, Image this: 

>What if your coworker downloaded that meme video from youtube, and instead of you downloading that video from youtube directly, your coworker can share it directly with you?


This is basically what IPFS does, letting users run their own IPFS systems which become a global decentralised CDN. It increases speed and latency, without putting resource constraints on the system providing the content system.

The technical differences between the Internet and IPFS are **location-addressing** and **content-addressing**. The [technical docs](https://docs-beta.ipfs.io/concepts/) of IPFS explains it quite well.

Large companies like [Netflix](blog.ipfs.io/2020-02-14-improved-bitswap-for-container-distribution/) and [Cloudflare](https://blog.cloudflare.com/distributed-web-gateway/) are already running it, so why wouldn't you give it a try?

<h2>Hosting content on IPFS</h2>

Since IPFS is best for hosting static content, such as images or this blog, there are multiple ways to host it. But first, we need to add files. 

The following is an IPFS address of an image - [https://gateway.ipfs.io/ipfs/QmPL9KnRbE4qBkSsazNM9YXb3NXwnMhmeg4BtHGYY8HoyE](https://gateway.ipfs.io/ipfs/QmPL9KnRbE4qBkSsazNM9YXb3NXwnMhmeg4BtHGYY8HoyE).
As you can see, the last part of the is the Content IDentifier (which can be translated to the hash of the file).
Like this <code>https://ipfs.io/ipfs/ CID  </code>.

<h3>Adding files</h3>

There are numerous ways to add content to the network, from using the IPFS Desktop application, to the IPFS Command Line tool.

Here is a snippet of uploading content to the network through the IPFS CLI:

<code>
	ipfs add -r ./sample_website
</code>

Which give the folllowing output:

1. added Qme5TzVW4en8HWP49N9nLHwUkAMhCn6zvWteoknmFHELLO sample_website/index.html
1. added QmThzr7LgYmUz6tfTArJK5tbrtidXSA12ZCL4ESy6DEMO0 sample_website/index.js
1. added QmeUG2oZvyx4NzfpP9rruKbmV5UNDmTQ8MoxuhTJGVTECH sample_website
  1.01 KiB / 1.01 KiB [=================================================] 100.00%


The latest hash printed is the CID for the whole directory and can be used to view the website.

The documentation and tutorials has walk throughs which explain more in detail about this (take note of the ipfs <i>add</i> and <i>files</i> command, only the <i>add</i> will pin the file).

<h3>File Pinning</h3>

Content is cached within the IPFS network and when peers communicate with each other, the closest neighbour that has that content will reply and send the provided file. 
However, since it's a Cached network, this means that files will be deleted when they're not very popular (i.e. when more people download meme.jpg, it'll be cached longer over multiple peers).

This is where file Pinning comes in. If you find some data of value to you, or the network, than there is the option to pin the file. This means that the file is hosted on an IPFS node, and even after a reboot or clearing of the cache, the Pinned files will still be there. 

This is powerfull from both a user and creator perspective. For example, this blog is pinned on the IPFS network on at least a few nodes, hence the content will exist forever (untill it's unpinned and unpopular).

<a href="https://pinata.cloud/">
{{< image src="/img/posts/hello-world/pinata.png" alt="Pinata logo" position="center" style="border-radius: 8px; width:50%; height:50%;" >}}
</a>

In order to pin files, the content needs to be available 24/7 on an IPFS node. When you don't want to run your own server, this is where file pinning services come in handy such as [Pinata](https://pinata.cloud/).

For this project, I'm hosting my own IPFS node on a server, which is a spare Raspberry PI that I had laying around (because RPI's are great).

Of course, this blog wouldn't be about IPFS if this blog is also not pinned on my own  IPFS node. So, for any neighbour near the location of Amsterdam, the files of this blog will load pretty fast :)


<h3>Automated deployments</h3>

So far we've covered what IPFS is, how it works and how to add files. Quite easy. However, since it's only for static files, which when changed are different files (CID's/hashes), we need to add those files again with the <code>ipfs add</code> command. 
Since this can be time consuming, automated deployments are the way to go. 

<a href="https://fleek.co/">
{{< image src="/img/posts/hello-world/FleekForDark.png" alt="Fleek logo" position="center" style="border-radius: 8px; width:50%; height:50%;" >}}
</a>

The service provided by [Fleek](https://fleek.co) is of great use to help with this. Think of it as Travis for IPFS deployments. In essence, Fleek links a public github repository and uploads the repository contents to the IPFS network ***everytime*** a change is detected (with pinning enabled). Isn't this great?
What's more, it has a great UI/UX and supportive community behind it.

Now that we'ver got our static files setup on automated deployments, we can continue with the domain name.

<h3>DNS for IPFS</h3>

In the IPFS world, links aren't quite readable for humans. This is because of the CID of the file, like <code>gateway.ipfs.io/ipfs/QmPL9KnRbE4qBkSsazNM9YXb3NXwnMhmeg4BtHGYY8HoyE</code>

This is not readable, especially for a blog or website. 
Luckily there are several options, namely DNSLink, Inter-Planetary Name Service, Ethereum Name Service (and possibly Unstoppable Domains).
At this moment, I've setup DNSLink.

<h4>DNSLink</h4>

DNSLink provides a link between a DNS record and the IPFS content. Within Fleek it can be setup easily, just pointing the subdomain record towards Fleek (I did this for [ipfs.technoprenerd.com](ipfs.technoprenerd.com))

However, I also wanted my main www subdomain to point to Fleek and ran into some issues. Luckily [Tarun Batra](ipfs.tarunbatra.com/blog/decentralization/Deploy-your-website-on-IPFS-Why-and-How/) provided me with some guidance on the correct DNS settings.
This was to point the CNAME record of <code>_dnslink.ipfs.</code> subdomain to my Fleek subdomain (instead of the dnslink TXT record to point to the static content of the website which didn't update <i>obviously</i>).


<h2>Conclusion</h2>

By now you know what the IPFS network is, how it operates and how you can get started.
Furthermore, the reason why it is important and hope that you see the added value of it and what it means for the future of the Web 3.0. 


We've only just started what's possible and where the future will take us! 
<br>
If you find this blog helpful, I'll be honoured if you pin this blog:)

<h4>Resources</h4>
Of course, I could not have done this without any good pointers and documentation.

[IPFS Documentation](https://docs-beta.ipfs.io/concepts/) 
<br>
[Fleek](https://gateway.ipfs.io/ipns/blog.fleek.co/posts/go-with-hugo-and-fleek)
<br>
[Pinata](https://pinata.cloud)
<br>
[Tarun Batra](https://gateway.ipfs.io/ipns/ipfs.tarunbatra.com/blog/decentralization/Deploy-your-website-on-IPFS-Why-and-How/)
