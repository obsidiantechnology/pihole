#  Family-oriented Home Network Management: Using Pi-hole as a content filter

This project utilises Pi-hole; a network-wide DNS sinkhole and makes use of its ability to configure upstream DNS to protect family members and children from  adult content, malicious websites, and invasive tracking before it ever reaches a device.

---

## Project Overview

The purpose of this project is to centralise control of URL access for a home network through DNS, blocking tracking requests, malicious websites as well as unwanted content (adult, gambling, drugs, violence etc.). 

### Key Benefits:
* **No Client Software:** As this is implemented through DNS, there is no need to configure endpoint devices with software or MDM, all devices, resident or guest, have the exact same enforcement rules applied; this prolongs endpoint battery life and reduces CPU usage, minimises administrative overhead and ensures peace of mind.
* **Privacy-Focused:** Blocks trackers and telemetry alongside harmful content.
* **Performance Boost:** As advertising media and URLs are blocked, webpages are able to be displayed faster.
* **Granular Control:** Whilst most ISP routers allow the use of upstream DNS servers such as OpenDNS FamilyShield for blocking harmful content, they don't provide any URL level control, opting instead for a everything or nothing approach. This solution grants parents complete control over everything and they can implement rules or changes as they see fit.
* **Logging:** Pi-hole's admin dashboard allows a user to see in real-time what resources are being requested and by whom. It allows parents to identify whether children may be attempting to circumvent restrictions, detect other resources which may need blocking and to understand problem areas - something not possible when using upstream DNS through an ISP router.

---

## The Hardware & Environment

* **Hardware:** Raspberry Pi or other SBC/ARM architecture devices or any 24/7 capable mini server, VM, literally anything that can run Pi-hole
* **Operating System:** for ease of installation and following this guide, I recommend a Raspberry Pi OS or Unix-based OS
* **Installation Method:** Terminal/CLI
* **Upstream DNS Provider:** 208.67.222.123 & 208.67.220.123 ([OpenDNS Family Shield](https://www.bing.com/ck/a?!&&p=57f180442291fddac6393f33b0bf6db08590621d15434d18c44ff05c92fe115cJmltdHM9MTc4MzM4MjQwMA&ptn=3&ver=2&hsh=4&fclid=10400cc6-3b86-6c91-2561-1b493a7d6d48&psq=opendns+family+shield&u=a1aHR0cHM6Ly9zaWdudXAub3BlbmRucy5jb20vZmFtaWx5c2hpZWxkLw))


---

## How it Works

Instead of modifying settings on individual devices, the network routes traffic seamlessly using the following flow:

1. **Device** requests a website (e.g., clicks a link).
2. **Router** is configured to point to the **Pi-hole** as its sole DNS server.
3. **Pi-hole** intercepts the request and cross-references it against the custom blocklists.
4. **Result:** If it's safe content, it resolves via the safe upstream DNS. If it's illicit or malware, the Pi-hole returns `0.0.0.0` (a dead end), and the content fails to load.

---

## Blocklist (Adlist) Configuration

To achieve reliable protection for kids without breaking the useful internet, I implemented a layered blocklist strategy:

| List Name / Source | Focus Area | Impact | Link |
| :--- | :--- | :--- | :--- |
| **StevenBlack FMS** | Pornography, Social Media, Fake News & Gambling | Restricts access to adult sites and gambling. | https://github.com/StevenBlack/hosts 
| **Hagezi DNS Blocklists** | Mixed Content | Block lists of varying degrees and a wide range of categories. | https://github.com/hagezi/dns-blocklists

> 💡 **Tip for families:** I also enabled **RegEx blocking** for specific high-risk keywords and explicitly blocked major malicious domains via the native blocklist settings.

---

##  Dashboard & Metrics

The admin interface allows a user to audit pretty much anyhting related to DNS, allowed requests, blocked requests, client making the requests, number of requests made, percentage of blocked requests and more. This is where Pi-hole truly shines compared to other solutions, it provides parents the ability to see what their children might be attempting to access, whether that be social media, violent content or anything else - it gives parents the opportunity to have a conversation with children and follow up with education and countermeasures as opposed to just blocks. 

## Main Dashboard
<img width="1248" height="723" alt="image" src="https://github.com/user-attachments/assets/19925acd-86d6-4e1e-8531-8a5c46e23206" />

### Continued
<img width="1021" height="533" alt="image" src="https://github.com/user-attachments/assets/c54bdc09-231e-43fe-b5a9-5f0b2f65ad4b" />

### Continued
<img width="1021" height="293" alt="image" src="https://github.com/user-attachments/assets/2b11186b-8ebc-48b3-bf5a-e3565db05d30" />

## Query Log with advanced filters 
<img width="1253" height="813" alt="image" src="https://github.com/user-attachments/assets/17bfc460-e70f-42f8-8912-d711fd3ff86b" />

As you can see, the metrics, logging and auditing capabilties are far more advanced than merely selecting an usptream DNS server via an ISP router's admin console would allow. I'm all for more control and data as it empowers parents to make informed choice about their kids and their families. 

## Acknowledgements

* This project is configured to work alongside [Pi-hole®](https://pi-hole.net/), a network-wide ad blocker protected by Pi-hole, LLC.
* Pi-hole is open-source software licensed under the European Union Public Licence (EUPL 1.2).
