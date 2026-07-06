#  Family-oriented Home Network Management: Using Pi-hole as a content filter

This project utilises Pi-hole; a network-wide DNS sinkhole and makes use of its ability to configure upstream DNS to protect family members and children from  adult content, malicious websites, and invasive tracking before it ever reaches a device.

---

## Project Overview

The purpose of this project is to centralise control of URL access for a home network through DNS, blocking tracking requests, malicious websites as well as unwanted content (adult, gambling, drugs, violence etc.). 

### Key Benefits:
* **No Client Software:** As this is implemented through DNS, there is no need to configure endpoint devices with software or MDM, all devices, resident or guest, have the exact same enforcement rules applied; this prolongs endpoint battery life and reduces CPU usage, minimises administrative overhead and ensures peace of mind.
* **Privacy-Focused:** Blocks trackers and telemetry alongside harmful content.
* **Performance Boost:** As advertising media and URLs are blocked, webpages are able to be displayed faster.
*  **Granularity & control:** Whilst most ISP routers allow the use of upstream DNS servers such as OpenDNS FamilyShield, they don't provide any URL level control, opting instead for a everything or nothing approach. This solution grants parents complete control over everything and they can implement rules or changes as they see fit.  

---

## The Hardware & Environment

* **Hardware:** [e.g. Raspberry Pi or other SBC/ARM architecture devices or any 24/7 capable mini server, VM, literally anything that can run ]
* **Operating System:** [e.g., Raspberry Pi OS or Unix-based OS]
* **Installation Method:** [e.g., Docker Container / Bare-metal curl script]
* **Upstream DNS Provider:** [e.g., Cloudflare Families (1.1.1.3) for an extra layer of malware and adult content filtering]

---

## Network Architecture

Instead of modifying settings on individual devices, the network routes traffic seamlessly using the following flow:

1. **Device** requests a website (e.g., clicks a link).
2. **Router** is configured to point to the **Pi-hole** as its sole DNS server.
3. **Pi-hole** intercepts the request and cross-references it against the custom blocklists.
4. **Result:** If it's safe content, it resolves via the safe upstream DNS. If it's illicit or malware, the Pi-hole returns `0.0.0.0` (a dead end), and the content fails to load.

---

## Blocklist (Adlist) Configuration

To achieve reliable protection for kids without breaking the useful internet, I implemented a layered blocklist strategy:

| List Name / Source | Focus Area | Impact |
| :--- | :--- | :--- |
| **Default Pi-hole List** | General Ads & Malware | Blocks core tracking domains and known malicious sites. |
| **StevenBlack FMS** | Pornography & Social Media | Restricts access to adult sites and gambling. |
| **Firebog Ticked Lists]** | Trackers & Telemetry | Enhances general smart-TV and app privacy. |

> 💡 **Tip for families:** I also enabled **RegEx blocking** for specific high-risk keywords and explicitly blocked major malicious domains via the native blocklist settings.

---

##  Dashboard & Metrics

The interface allows me to audit what types of traffic are triggering blocks, ensuring the network stays clean without overly restricting normal web usage. 

<Image src="image_agent_tag_2194977650735676180" alt="Pi-hole dashboard blocking query metrics" caption="Real-time breakdown of permitted vs. blocked domain queries" />

---

##  Lessons Learned & Key Takeaways

* **The "Over-Blocking" Challenge:** At first, some strict blocklists broke legitimate educational sites. I learned how to use the Pi-hole Query Log to isolate the broken domain and safely add it to the **Whitelist**.
* **Hardcoded DNS Workaround:** Some devices (like certain smart TVs or streaming sticks) have hardcoded Google DNS (`8.8.8.8`). To prevent them from bypassing the filter, I configured my router to block outgoing port 53 traffic from any device except the Pi-hole.
* **Maintenance:** Blocklists update automatically once a week via Pi-hole's internal cron jobs, making the system low-maintenance once configured.

---

##  How to Replicate This Setup

1. Flash your Linux OS onto your hardware device.
2. Install Pi-hole using the official guide.
3. Access the web interface admin portal, navigate to **Adlists**, and paste your chosen family-safe blocklists.
4. Log into your home router's settings interface, find the **LAN/DHCP** settings, and change the Primary DNS server IP to match your Pi-hole's static IP address.
5. Flush your local device DNS caches or restart your devices to route them cleanly through the new filter.
