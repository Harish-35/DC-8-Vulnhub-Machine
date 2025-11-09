
# Description
DC-8 is another purposely built vulnerable lab designed to help gain experience in the world of penetration testing. This challenge is a bit of a hybrid between being an actual challenge and a “proof of concept” to determine whether two-factor authentication installed and configured on Linux can prevent the Linux server from being exploited.

The “proof of concept” portion of this challenge arose from a question about two-factor authentication and Linux on Twitter, coupled with a suggestion by @theart42. The ultimate goal of this challenge is to bypass two-factor authentication, gain root access, and read the one and only flag.

You probably wouldn’t even know that two-factor authentication was installed and configured unless you attempted to log in via SSH, but it’s definitely there and doing its job. Linux skills and familiarity with the Linux command line are essential, as is some experience with basic penetration testing tools. For beginners, Google can be of great assistance, but you can always tweet me at @DCAU7 for help. However, take note: I won’t give you the answer, but I’ll guide you on how to move forward.

# Introduction
Hey! Harish here, I’ll take you through the steps I took to root the DC-8 vulnerable lab. This lab presented unique challenges. We’ll start with reconnaissance, sql injection vulnerability discovery, exploiting a form that allows php code, and finally exploiting exim4 to achieve privilege escalation to root access. Let’s dive in!

# Table of Contents
- [1.Getting started (Setup)](#getting-started-setup)

- [2.Reconnaissance](#2.Reconnaissance)

- [3.Exploitation](#3.Exploitation)

- [4.Privilege Escalation](#4.Privilege-Escalation)

- [5.Conclusion](#5.Conclusion)

# 1.Setup



