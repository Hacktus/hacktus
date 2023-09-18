---
title: "OAuth Misconfiguration Leading to Unauthorized Admin Access For All Org Products"
datePublished: Mon Sep 18 2023 19:44:21 GMT+0000 (Coordinated Universal Time)
cuid: clmpaoptn000e09k3f9jx2iyw
slug: oauth-misconfiguration-leading-to-unauthorized-admin-access-for-all-org-products
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/0oDypg0s2WU/upload/047c3b6e0bb4a50e30fc74a0eaecacd2.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1695034316893/5e3a5c12-0ccf-49e8-80c8-d97a39fc79a6.jpeg
tags: hacking, hacker, penetration-testing, bugbounty, bugbountytips

---

## TL;DR ?

I signed up using any unclaimed email on application\_2 (e.g., [victim@example.com](mailto:victim@example.com)) due to no email verification, then logged into the victim's account on application\_1 using the SSO feature that allowed me to log in using application\_2.

### **Introduction**

In this report, I am going to detail a notable vulnerability I discovered on \[Redacted Company\]'s platforms. This vulnerability, stemming from an OAuth misconfiguration, allowed an attacker to access an admin panel by exploiting a single sign-on system. This insight underscores the significance of meticulous security configurations and the potential dangers that even slight missteps can harbor.

### **The Discovery**

While evaluating several of \[Redacted Company\]'s products:

* Product
    
* Another product
    
* Another one?
    
* ...and more.
    

I noted an unusual occurrence during the login process. There were around 8 to 9 sign-in options provided. However, only the [`vulnerable.com`](http://vulnerable.com) option allowed users to create a new account, and that option belongs to the same company.

With curiosity, I forged ahead and created an admin account for me on `vulnerable.com`. Subsequent to this, I added another user to my organization using an email affiliated with a \[Redacted Company\] admin which was as simple as `admin@redacted.com`and I set a password for it.

The crux of the issue here was that `vulnerable.com` didn’t demand email verification. Given that \[Redacted Company\] authenticated based solely on the email address, I exploited this loophole to sign into the admin panel of their product, utilizing the `vulnerable.com` login option.

### **The Impact**

An individual armed with the right information could:

1. Full compromise of every product and every admin panel the company has.
    
2. By taking over admin accounts, I was able to takeover any customer/user accounts as well and leak all their data.
    

The ramifications are broad, impacting numerous services under \[Redacted Company\]'s umbrella.

### **Conclusion**

Discovering this vulnerability in a renowned platform was quite an eye-opener. It's a testament to the intricate nature of digital security and serves as a reminder that overlooking even the smallest details can lead to significant security lapses.

If I had rushed to only take note of the multiple login options without delving deeper into the single registration option, I might have missed out on the magnitude of the vulnerability. Thus, like the [subdomain takeover I mentioned in my last blog](https://hacktus.tech/subdomain-takeover-leading-to-full-account-takeover), further probing often reveals more severe impact points.

### **Timeline**

* **2023-08-11:** Reported
    
* **2023-08-14:** Delved deeper into the admin panels.
    
* **2023-08-15:** Discovered an active ex-admin employee account which helped the company to terminate as well.
    
* **2023-08-15:** Triaged
    
* **2023-09-05:** Fixed
    
    Severity: Critical(10.0)  
    Bounty: 4000$
    

---

**Final Word:** Continuous probing, persistence, and a keen eye for details are essential in the world of cybersecurity. Ensure you're always a step ahead. Happy hacking!