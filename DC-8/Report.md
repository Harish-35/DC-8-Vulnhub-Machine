
# Description
DC-8 is another purposely built vulnerable lab designed to help gain experience in the world of penetration testing. This challenge is a bit of a hybrid between being an actual challenge and a “proof of concept” to determine whether two-factor authentication installed and configured on Linux can prevent the Linux server from being exploited.

The “proof of concept” portion of this challenge arose from a question about two-factor authentication and Linux on Twitter, coupled with a suggestion by @theart42. The ultimate goal of this challenge is to bypass two-factor authentication, gain root access, and read the one and only flag.

You probably wouldn’t even know that two-factor authentication was installed and configured unless you attempted to log in via SSH, but it’s definitely there and doing its job. Linux skills and familiarity with the Linux command line are essential, as is some experience with basic penetration testing tools. For beginners, Google can be of great assistance, but you can always tweet me at @DCAU7 for help. However, take note: I won’t give you the answer, but I’ll guide you on how to move forward.

# Introduction
Hey! Harish here, I’ll take you through the steps I took to root the DC-8 vulnerable lab. This lab presented unique challenges. We’ll start with reconnaissance, sql injection vulnerability discovery, exploiting a form that allows php code, and finally exploiting exim4 to achieve privilege escalation to root access. Let’s dive in!

# Table of Contents
- [Getting started (Setup)](#1.getting-started-setup)

- [Reconnaissance](#2.Reconnaissance)

- [Exploitation](#3.Exploitation)

- [Privilege Escalation](#4.Privilege-Escalation)

- [Conclusion](#5.Conclusion)

# 1.Getting started (Setup)
As always we start by setting up the working environement and the network.

First we have to download the required machine "https://download.vulnhub.com/dc/DC-8.zip"

I setting up a Bridge Adapter Network for both target machine and Attacker machine.

My setup is the following :

Attacker Machine:

<img width="965" height="587" alt="1" src="https://github.com/user-attachments/assets/d8362018-2328-4438-b7b6-813f30898d76" />
#
Target Machine:

<img width="950" height="587" alt="2" src="https://github.com/user-attachments/assets/47c4ef2e-8909-49f8-867f-02482aa0e81f" />
#
First let’s run netdiscover on the network to detect the target IPv4 address.

```
sudo netdiscover -r 192.168.1.63
```

<img width="1920" height="1022" alt="1" src="https://github.com/user-attachments/assets/12b58f30-d364-4d68-9ad9-360d3386f2ba" />
#
And pick the PCS ip

# 2.Reconnaissance
# Nmap Scan:
To start with, I fired up an Nmap scan against the target’s IP address. Notably, port 22 (SSH) and port 80 (HTTP) are open. It’s also worth noting that Drupal 7 is running on the web application on port 80.
```
nmap 192.168.1.64
```

<img width="1920" height="1022" alt="2" src="https://github.com/user-attachments/assets/b3801f6e-69e6-41d3-90b5-8b57d78e03a5" />
#
Scan Another scan with -A (Agressive scan) for more  info
```
nmap -A 192.168.1.64
```

<img width="1920" height="1022" alt="3" src="https://github.com/user-attachments/assets/f0cb490d-b0c2-415e-bdc6-d81be8f4930f" />

# Web Enumeration
Upon accessing the web application, I was presented with a landing page. Notably, there was an advisory on the home page mentioning potential disruptions to the site while resolving a few outstanding issues. This could imply possible security misconfigurations that could be exploited.

<img width="1920" height="1022" alt="4" src="https://github.com/user-attachments/assets/32f4ec65-3dd2-48de-aa48-4b83f4e4e899" />
#
After trying each options in the site, i see some different response and found the chance for try the "SQLi" in URL.

<img width="1920" height="1022" alt="5" src="https://github.com/user-attachments/assets/cad4357e-142a-498e-9129-4d4c8649fe42" />

# 3.Exploitation

# SQLMap

So i try the "SQL injection" using "sqlmap tool". Using command
```
sudo sqlmap -u http://192.168.1.64/?nid=1 --dbs --batch --risk 3 --level 5
```
And we find some Databases

<img width="1920" height="1022" alt="6" src="https://github.com/user-attachments/assets/ec6641e2-6e34-4dab-838c-9520608c6a20" />
#
Using the "d7db" Db for next step
```
sudo sqlmap -u http://192.168.1.64/?nid=1 -D d7db  --tables --batch --risk 3 --level 5
```

<img width="1920" height="1022" alt="8" src="https://github.com/user-attachments/assets/7971a9d1-a1a6-4edc-b78a-bb93d87d161d" />
#
And get lot of tables, i pick the "users" table.

```
sudo sqlmap -u http://192.168.1.64/?nid=1 -D d7db  -T users --batch --risk 3 --level 5
```

<img width="1920" height="1022" alt="9" src="https://github.com/user-attachments/assets/1680187b-eead-428b-8a4f-12ce1ec4fad2" />
#
Finally, I dumped the users table, revealing two user credentials, including names, email addresses, and password hashes.

<img width="1920" height="1022" alt="10" src="https://github.com/user-attachments/assets/27a72961-41ca-4c03-9502-e63374d21b49" />

# Cracking the Hashes
I saved the hashes in a text file called john.txt.
Use the "John The Ripper" tool for crack the password.

<img width="1920" height="1022" alt="11" src="https://github.com/user-attachments/assets/21e930cf-383c-4c9e-bc9f-73bc52c46fc2" />
#
Using John’s credentials, I logged in to the web application.

<img width="1920" height="1022" alt="12" src="https://github.com/user-attachments/assets/fe63b02f-d544-49fe-b224-ad7eec853f01" />

# Exploiting the Webform for Remote Code Execution
After logging in, I revisited the “Contact Us” page and found a Webform tab. I modified the form’s confirmation message to include a PHP reverse shell code, which would execute upon form submission.

<img width="1920" height="1022" alt="14" src="https://github.com/user-attachments/assets/26018860-71dd-4b14-af93-843f08d5d2d6" />

<img width="1920" height="1022" alt="15" src="https://github.com/user-attachments/assets/8b5be1a3-5796-475f-9f03-228a8fea1c87" />
#
After i copy PHP reverse shell code from github, paste it into input field and change the text format to PHP code.

<img width="1920" height="1022" alt="18" src="https://github.com/user-attachments/assets/df43c67f-4145-4c02-8fdf-0f328d0169b6" />
#
Now change the ip=192.168.1.63(Attacker ip) and port= 8889 in the PHP code.

<img width="1920" height="1022" alt="19" src="https://github.com/user-attachments/assets/10e0f2a5-b125-4345-9185-f1de037d8ba7" />
#
I then set up a Netcat listener on my machine to catch the reverse shell connection.

```
nc -lvnp 8886
```

<img width="1920" height="1022" alt="20" src="https://github.com/user-attachments/assets/9b783f91-52c1-46c2-b20e-ccfdd99d367e" />

# 4.Privilege Escalation
With the listener set up, I filled out the Contact Us form with arbitrary data. Upon submission, the page stalled—a good sign that my payload was executing. The listener picked up the connection, providing me a foothold on the system.




