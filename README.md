#🐝 T-Pot Honeypot Setup on Digital Ocean ☁️
Introduction

Welcome to my T-Pot Honeypot project! 👋

T-Pot is an all-in-one honeypot platform that combines multiple honeypot technologies to capture and analyze attacks in real-time. It’s perfect for research and security monitoring to understand attacker behaviors and techniques. 🔒

I decided to deploy T-Pot on Digital Ocean, a cloud computing platform that provides simple and scalable virtual machines (Droplets) to host applications. Digital Ocean makes it easy to set up a server quickly without worrying about hardware management. ⚡

#🛠 Creating a Droplet
1. Go to Create Droplet

Log into your Digital Ocean account.

Click Create → Droplet.

You’ll see the configuration page for your virtual machine.
![Snimak ekrana 2025-12-06 135714](https://github.com/user-attachments/assets/ef216b89-1672-4066-afd2-25a17c2fdb1b)

2. Choose an Image

Select Ubuntu 24.04 LTS.

Ubuntu is a user-friendly Linux operating system widely used for servers.

Explanation of the version:

24.04 → Released in April 2024

LTS → Long Term Support (5 years of updates)

x64 → 64-bit architecture, supports modern applications and more memory

![Snimak ekrana 2025-12-06 105721](https://github.com/user-attachments/assets/f2fd83f3-d96f-4e99-a403-a145bf6ffefb)

3. Choose a Region

Pick a region closest to you for better latency. 🌍

Some examples:

🇺🇸 New York

🇩🇪 Frankfurt

🇸🇬 Singapore

🇦🇺 Sydney

![Snimak ekrana 2025-12-06 140641](https://github.com/user-attachments/assets/b6ac6764-bd40-4eea-afd2-a950ca497acd)

4. Select CPU, Memory, and Disk

Choose the CPU and RAM based on your expected load.

Select disk type (SSD recommended) and size.

Optionally, enable automatic backups (recommended but not mandatory).

5. Authentication

Set up authentication:

Password for root user OR

SSH key for secure login

![Snimak ekrana 2025-12-06 140159](https://github.com/user-attachments/assets/6e658cd1-99c3-43be-9495-1d2136a84e80)

6. Create Droplet

Click Create Droplet and wait for the server to be provisioned.
You will receive the IP address to access your T-Pot honeypot. 🚀


