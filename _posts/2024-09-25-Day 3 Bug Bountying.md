---
title: IAM Enumeration Tool - Bug Bountying 3/100
tags: BugBounty
categories: 
---
Welcome to **Day 3** of my 100 Days of Bug Bountying! 🚀 Today was all about **building a custom Python script** to help identify **potential privilege escalation vulnerabilities** in AWS IAM policies. This tool is part of my effort to create real, practical tools for pentesting AWS environments and finding misconfigurations that can be exploited by attackers.

![84a21f5c0cec7bb3d937818fe709a11e.png](/assets/img/screenshots/BugBounty/84a21f5c0cec7bb3d937818fe709a11e.png)

Link to the tool: [https://github.com/jvs3c/iam_enumerator](https://github.com/jvs3c/iam_enumerator)

This script is designed to detect potential privilege escalation vulnerabilities in AWS IAM policies. It enumerates AWS IAM users and checks both attached managed and inline policies for risky permissions. Specifically, it looks for actions that could allow users to elevate their privileges :D 