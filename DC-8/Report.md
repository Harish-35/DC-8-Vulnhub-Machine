
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

<img width="965" height="587" alt="1" src="https://github.com/user-attachments/assets/d8362018-2328-4438-b7b6-813f30898d76" />

<img width="950" height="587" alt="2" src="https://github.com/user-attachments/assets/47c4ef2e-8909-49f8-867f-02482aa0e81f" />




