---
title: "Unveiling the Arcane Art of Intercepting HTTPS Traffic in Desktop Apps & Games!"
datePublished: Thu Feb 22 2024 08:51:44 GMT+0000 (Coordinated Universal Time)
cuid: clswzi77t000108l4ebrc2fn3
slug: unveiling-the-arcane-art-of-intercepting-https-traffic-in-desktop-apps-games
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708591948454/40188a28-c04e-4b98-a67b-dee651eda9cc.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1708591889097/288fe0af-8df1-483c-935f-aea7159d712f.jpeg

---

NOTE: This journey is fraught with challenges like SSL pinning - a hurdle I'll tackle in my next post. For now, let's master the basics.

⏪ Quick Recap: In my last thread, we explored bugs in game hacking. Some of you inquired further - how do I intercept traffic beyond just finding bugs? Here’s the detailed workflow, addressing your curiosity:

1\. Laying the Foundation with Proxies: Start by choosing a proxy - Burp Suite, Charles Proxy, or an MITM proxy. Each has its merits. For HTTPS, here’s how to trust your proxy’s root certificate on your device:

* In Burp Suite, go to 'Proxy' &gt; 'Options' tab. Click 'Import / export CA certificate', then export it in the format suitable for your OS.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708591526170/0f86a6b7-bf57-4d40-865c-176b61916a40.png align="left")
    
* Install this certificate. On Windows, it goes into the 'Trusted Root Certification Authorities' store via the Microsoft Management Console (mmc). On macOS, add it to the System keychain in the 'Keychain Access' app and set it to 'Always Trust'.
    
* This allows your device to trust the encrypted traffic passing through the proxy, avoiding security alerts.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708591628007/5b5ed414-6ee0-4286-9cb8-ef9c8f864d9f.png align="left")
    

2\. Monitoring with Wireshark:

* Fire up Wireshark and begin capturing packets on the network interface the app uses. But the flood of data can be overwhelming.
    
* Filter the domain names. use a filter like " dns " -[dns.qry.name](http://dns.qry.name)\- to narrow down the packets to be able to see the domains that your game or app are making requests to
    

3\. Rerouting via Hosts File:

* This step is about rerouting app traffic through your proxy. The hosts file is like your PC’s address book for the internet.
    
* You’ll find this file at “C:\\Windows\\System32\\drivers\\etc\\hosts” on Windows or “/etc/hosts” on Linux and macOS.
    
* Add lines mapping the domain names your app contacts to 127.0.0.1 (your local machine) in this format:
    

127.0.0.1 example(dot)com

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708591720113/da3c207e-f226-4a60-aec1-a2b865e66542.png align="left")

Now, the traffic is set to go through your proxy.

4\. Configuring Proxy Listeners:

* In Burp Suite, add a new listener on 127.0.0.1 and the port you choose. Ensure it matches the redirected traffic from your hosts file.
    
* Any traffic to the mapped domains in your hosts file now hits your proxy listener.
    
* Enable invisible proxying from the Request Handling tab.
    

5\. Burp's Hostname Resolution:

* Here, we translate human-friendly domain names to IP addresses. Under 'Project options' &gt; 'Hostname Resolution', input the domains and their IPs based on what you discovered in Wireshark or through ‘dig’ or ‘nslookup’ commands.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708591822529/6ffd808b-7d9e-45c2-a777-62fa42a31b7b.png align="left")

as in dig example(dot)com +short

In case there is no A record, you need to get IP address of that NS record

as in dig example(dot)com(dot)cdn(dot)cloudflare(dot)net +short

And now you're good to go!

Stay tuned for SSL pinning complexities and how to navigate them. Until then, sharpen these skills and hack ethically. !