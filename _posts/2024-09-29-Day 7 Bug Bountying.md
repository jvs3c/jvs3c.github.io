---
title: Weaponizing Favicons - Bug Bountying 7/100
tags: BugBounty
categories: 
---
Welcome back to **Day 7** of my 100 Days of Bug Bountying journey! 🌍

While reading a blog on day 5 (which I caught up on today), I discovered something new—**weaponizing Favicon hashes** to find subdomains. This technique was something I hadn’t come across before, so I decided to fork the [FavFreak](https://github.com/devanshbatham/FavFreak) repository and created [my own version](https://github.com/jvs3c/FavFreak_jvs3c) of the tool to handle Favicons.

I made adjustments to the output and parsing logic, making it easier to integrate into the automated tool I started working on yesterday. This modification should allow smoother integration and enhance the overall recon process in my automation setup.

![f2ea3862599b99c460a3c6f10eb6072c.png](/assets/img/screenshots/BugBounty/f2ea3862599b99c460a3c6f10eb6072c.png)

🐍 **Tool Features:**

- [x] Process input with httpx
- [x] Save output to a new directory
- [x] Added Favicon Weaponinzg

📚 **Resources:**

- https://medium.com/@Asm0d3us/weaponizing-favicon-ico-for-bugbounties-osint-and-what-not-ace3c214e139
- https://payatu.com/blog/favicon-hash/
- https://elhackeretico.com/enumerando-subdominios-con-favicon-ico/
- Reading **Bug Bounty Bootcamp** by Vickie Li

&nbsp;