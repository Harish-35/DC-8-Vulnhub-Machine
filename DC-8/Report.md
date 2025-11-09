
# Description
DC-8 is another purposely built vulnerable lab designed to help gain experience in the world of penetration testing. This challenge is a bit of a hybrid between being an actual challenge and a “proof of concept” to determine whether two-factor authentication installed and configured on Linux can prevent the Linux server from being exploited.

The “proof of concept” portion of this challenge arose from a question about two-factor authentication and Linux on Twitter, coupled with a suggestion by @theart42. The ultimate goal of this challenge is to bypass two-factor authentication, gain root access, and read the one and only flag.

You probably wouldn’t even know that two-factor authentication was installed and configured unless you attempted to log in via SSH, but it’s definitely there and doing its job. Linux skills and familiarity with the Linux command line are essential, as is some experience with basic penetration testing tools. For beginners, Google can be of great assistance, but you can always tweet me at @DCAU7 for help. However, take note: I won’t give you the answer, but I’ll guide you on how to move forward.

# Introduction
Hey! Harish here, I’ll take you through the steps I took to root the DC-8 vulnerable lab. This lab presented unique challenges. We’ll start with reconnaissance, sql injection vulnerability discovery, exploiting a form that allows php code, and finally exploiting exim4 to achieve privilege escalation to root access. Let’s dive in!

# Table of Contents
- [Getting started (Setup)](#getting-started-setup)

- [Reconnaissance](#Reconnaissance)

- [Exploitation](#Exploitation)

- [Privilege Escalation](#Privilege-Escalation)

- [Conclusion](#Conclusion)

# 1.Getting started (Setup)
As always we start by setting up the working environement and the network.

First we have to download the required machine "https://download.vulnhub.com/dc/DC-8.zip"

I setting up a Bridge Adapter Network for both target machine and Attacker machine.

My setup is the following :

Attacker Machine:

<img width="965" height="587" alt="1" src="https://github.com/user-attachments/assets/d8362018-2328-4438-b7b6-813f30898d76" />

Target Machine:

<img width="950" height="587" alt="2" src="https://github.com/user-attachments/assets/47c4ef2e-8909-49f8-867f-02482aa0e81f" />

First let’s run netdiscover on the network to detect the target IPv4 address.

```
sudo netdiscover -r 192.168.1.63
```

<img width="1920" height="1022" alt="1" src="https://github.com/user-attachments/assets/12b58f30-d364-4d68-9ad9-360d3386f2ba" />

And pick the PCS ip

# 2.Reconnaissance
# Nmap Scan:
To start with, I fired up an Nmap scan against the target’s IP address. Notably, port 22 (SSH) and port 80 (HTTP) are open. It’s also worth noting that Drupal 7 is running on the web application on port 80.
```
nmap 192.168.1.64
```

<img width="1920" height="1022" alt="2" src="https://github.com/user-attachments/assets/b3801f6e-69e6-41d3-90b5-8b57d78e03a5" />

Scan Another scan with -A (Agressive scan) for more  info
```
nmap -A 192.168.1.64
```

<img width="1920" height="1022" alt="3" src="https://github.com/user-attachments/assets/f0cb490d-b0c2-415e-bdc6-d81be8f4930f" />

# Web Enumeration
Upon accessing the web application, I was presented with a landing page. Notably, there was an advisory on the home page mentioning potential disruptions to the site while resolving a few outstanding issues. This could imply possible security misconfigurations that could be exploited.

<img width="1920" height="1022" alt="4" src="https://github.com/user-attachments/assets/32f4ec65-3dd2-48de-aa48-4b83f4e4e899" />



