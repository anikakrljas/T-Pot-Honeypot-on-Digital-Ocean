### 🐝 T-Pot Honeypot Setup on Digital Ocean ☁️
Introduction

Welcome to my T-Pot Honeypot project! 👋

T-Pot is an all-in-one honeypot platform that combines multiple honeypot technologies to capture and analyze attacks in real-time. It’s perfect for research and security monitoring to understand attacker behaviors and techniques. 🔒

I decided to deploy T-Pot on Digital Ocean, a cloud computing platform that provides simple and scalable virtual machines (Droplets) to host applications. Digital Ocean makes it easy to set up a server quickly without worrying about hardware management. ⚡

# 🛠 Creating a Droplet
## 1. Go to Create Droplet

Log into your Digital Ocean account.
Click Create → Droplet.

## 2. Choose an Image

Select Ubuntu 24.04 LTS.
Ubuntu is a user-friendly Linux operating system widely used for servers.
<ul>
  <li><strong>24.04</strong> → Released in April 2024</li>
  <li><strong>LTS</strong> → Long Term Support (5 years of updates)</li>
  <li><strong>x64</strong> → 64-bit architecture, supports modern applications and more memory</li>
</ul>

![Snimak ekrana 2025-12-06 105721](https://github.com/user-attachments/assets/f2fd83f3-d96f-4e99-a403-a145bf6ffefb)

## 3. Choose a Region

Pick a region closest to you for better latency. 🌍
Some examples:
<ul>
  <li><strong>🇺🇸 New York</strong></li>
  <li><strong>🇩🇪 Frankfurt</strong></li>
  <li><strong>🇸🇬 Singapore</strong></li>
  <li><strong>🇦🇺 Sydney</strong></li>
</ul>

![Snimak ekrana 2025-12-06 140641](https://github.com/user-attachments/assets/b6ac6764-bd40-4eea-afd2-a950ca497acd)

## 4. Select CPU, Memory, and Disk

Choose the CPU and RAM based on your expected load.
Select disk type (SSD recommended) and size.
Optionally, enable automatic backups (recommended but not mandatory).

## 5. Authentication

Set up authentication:
Password for root user OR
SSH key for secure login
![Snimak ekrana 2025-12-06 140159](https://github.com/user-attachments/assets/6e658cd1-99c3-43be-9495-1d2136a84e80)

## 6. Create Droplet

Click Create Droplet and wait for the server to be provisioned.
You will receive the IP address to access your T-Pot honeypot. 🚀


# 🚨 Deploying T-Pot Honeypot via PowerShell (SSH)

This section documents the exact steps I used to deploy T-Pot, a multi-honeypot platform, on my server using PowerShell and SSH. Each command is explained in detail to help others fully understand the setup process.

## 1. 🔐 Connect to Your Server
ssh root@publicIPaddress

This command initiates a secure SSH connection from your local machine to the remote server using the root account. Replace publicIPaddress with your server’s actual public IP. From PowerShell, this opens an encrypted session where you can run administrative commands.

## 2. 📦 Update and Upgrade System Packages
apt-get update && apt-get upgrade -y

apt-get update refreshes the package index, ensuring your system knows about the latest available versions of all packages.
apt-get upgrade -y automatically installs all available updates.
This step ensures your server is fully patched before installing T-Pot.

## 3. 👤 Create a New User
adduser username

Creates a new user account on the system. Replace username with the name you want to assign. You’ll be prompted to set a password and optional user details.

## 4. 🔑 Grant Sudo Privileges
sudo usermod -aG sudo username

Adds the new user to the sudo group, giving them administrative privileges. This is a best practice so that you don’t continue using the root account for routine tasks.

## 5. 🔄 Switch to the New User
su username

Switches from the root user to the newly created user account. This ensures the installation and repository setup happen under a safer, non-root environment.

## 6. 📁 Navigate to the User’s Home Directory
cd /home/username

Moves into the home directory of the new user, where your cloned repository and installation files will be stored.

## 7. 📥 Clone Your Repository
git clone repos

Clones the repository containing the T-Pot CE files or your fork of the project. Replace repos with the actual Git URL.

## 8. 📂 Enter the T-Pot Directory
cd tpotce/

Navigates into the cloned repository folder where the T-Pot installation script is located.

## 9. ⚙️ Run the T-Pot Installation Script
./install.sh

Executes the official T-Pot installation script. The script will guide you through configuration options and set up multiple honeypot services, dashboards, and data pipelines.
Make sure the script is executable (chmod +x install.sh) if needed.

# 🔄 Changing the SSH Port During T-Pot Installation
During the T-Pot installation process, the setup wizard automatically changes the default SSH port from 22 to a randomly assigned high port number.
This is done for security reasons, because T-Pot exposes multiple honeypots on common ports, and keeping SSH on 22 would make the real system vulnerable or interfere with honeypot services.

![Snimak ekrana 2025-12-06 112610](https://github.com/user-attachments/assets/111bd295-fa72-46ad-a01b-2229d116a186)

🧰 T-Pot Deployment Profiles Explained

Below are the available installation modes you can choose from during the setup screen:

🐝 Hive

A full-scale, multi-honeypot environment with dashboards, Elastic stack, data pipelines, and logging. This is the complete T-Pot experience—ideal if you want visualization, analytics, and all honeypot services running.

📡 Sensor

A lightweight deployment focused on collecting attack traffic only, without dashboards or analytics.
Useful when you want to forward data to an external T-Pot Hive instance or SIEM.

🧪 LM (Log Monitor)

Runs honeypots and captures logs, but with reduced system-wide overhead.
A good option for mid-range hardware where you still want detailed telemetry.

📦 Mini

Very small footprint. Runs a limited set of honeypots with minimal computational requirements.
Perfect for low-power devices or small virtual machines.

📱 Mobile

A variant optimized for mobile or ARM devices (e.g., Raspberry Pi).
Includes honeypots and services tailored for lower power consumption.

🐢 Tarpit

A specialized mode that focuses on tarpitting attackers—deliberately slowing down connections, consuming attacker resources, and wasting their time.
This mode is minimalistic but very effective for bot deterrence.

# 🔄 Reboot After Installation
sudo reboot

Reboots the server so the new SSH configuration, services, and T-Pot components fully activate.

# 🔑 Connecting Using the New SSH Port
ssh -p sshportnumber root@publicIPaddress

-p sshportnumber specifies the new SSH port assigned by T-Pot (e.g., 64297).
You must now use this port every time you connect via SSH.

# 🌐 Accessing the T-Pot Web Interface
https://publicIPaddress:sshportnumber

T-Pot’s web UI (Cockpit + dashboards) also listens on the same high port that SSH was moved to (for example, https://yourIP:64297).
This port is chosen dynamically by the installer to avoid conflicts with the honeypots, many of which use standard ports (22, 80, 443, etc.) to mimic real services.

The high port—e.g., 64297—acts as a secure management port for both:

🔐 SSH administration

📊 Web dashboard interface

# 🔁 Restarting T-Pot Services
systemctl restart tpot

Restarts all T-Pot honeypot services, dashboards, and supporting components without rebooting the whole machine.
Useful after configuration updates or troubleshooting.

# T-Pot 

![Snimak ekrana 2025-12-06 123918](https://github.com/user-attachments/assets/31f8f62c-7835-4339-9c66-44bcef035a0b)

## 📊 T-Pot Honeypot – 24-Hour Attack Report

![Snimak ekrana 2025-12-07 130011](https://github.com/user-attachments/assets/6807c393-3275-4667-8091-737cd811c7db)

Time window: Last 24 hours
Total recorded events: ≈ 51,463 attacks
Your honeypot sensors captured a large amount of malicious traffic from multiple countries across the globe. Below is a breakdown of the observed activity by country, protocol, service, and general attack behavior.

# 🌍 1. Geographic Distribution of Attacks

Based on the “Top Countries by Hits” section in your dashboard:

Top 5 Attacking Countries

<ul>
  <li>
    🇺🇸 <strong>United States – 6079 hits</strong><br>
    Very active across multiple services, especially SSH and SMB.
  </li>
  <li>
    🇮🇱 <strong>Israel – 3037 hits</strong><br>
    Strong activity, likely automated scanning frameworks.
  </li>
  <li>
    🇳🇱 <strong>The Netherlands – 1932 hits</strong><br>
    Common hotspot for cloud-origin traffic.
  </li>
  <li>
    🇭🇰 <strong>Hong Kong – 1927 hits</strong><br>
    Significant scanning activity, possibly botnets.
  </li>
  <li>
    🇭🇺 <strong>Hungary – 1477 hits</strong><br>
    Local/regional scanning noted.
  </li>
</ul>

# 🔌 2. Services / Protocols Targeted

The color legend in the interface shows:

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Color</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Service</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🔴 Red</td>
      <td style="border: 1px solid #ddd; padding: 8px;">FTP</td>
      <td style="border: 1px solid #ddd; padding: 8px;">Likely brute force attempts</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟠 Orange</td>
      <td style="border: 1px solid #ddd; padding: 8px;">SSH</td>
      <td style="border: 1px solid #ddd; padding: 8px;">One of the most attacked services—login brute force, key probing</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟡 Yellow</td>
      <td style="border: 1px solid #ddd; padding: 8px;">TELNET</td>
      <td style="border: 1px solid #ddd; padding: 8px;">Very common with IoT malware</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟢 Green</td>
      <td style="border: 1px solid #ddd; padding: 8px;">EMAIL</td>
      <td style="border: 1px solid #ddd; padding: 8px;">SMTP spam attempts or probing</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">⚪ White</td>
      <td style="border: 1px solid #ddd; padding: 8px;">SQL</td>
      <td style="border: 1px solid #ddd; padding: 8px;">Database enumeration/injection attempts</td>
    </tr>
  </tbody>
</table>

# 🔍 3. Top Attacking IPs

Your dashboard lists the IPs with the highest number of hits:

<ul>
  <li>🇮🇱 <strong>141.226.93.223</strong> – 3036 hits</li>
  <li>🇭🇰 <strong>185.243.5.185</strong> – 1724 hits</li>
  <li>🇭🇺 <strong>31.46.245.29</strong> – 1476 hits</li>
  <li>🇺🇸 <strong>198.143.191.202</strong> – 1029 hits</li>
  <li>🇺🇸 <strong>148.72.169.42</strong> – 763 hits</li>
</ul>

Most of these are likely automated bots or compromised servers performing broad-spectrum scanning.

## 📘 T-Pot Honeypot – Kibana 24h Attack Report

This report summarizes all attacks recorded by the T-Pot honeypot environment during the last 24 hours, visualized through Kibana dashboards.
The goal is to provide a clear, readable, and insightful overview of attacker behavior, targeted services, ports, and credential attempts across all honeypot sensors.

![Snimak ekrana 2025-12-07 132216](https://github.com/user-attachments/assets/7945021d-edd1-44f0-b183-5a946fbc228e)


# 📊 1. Total Attacks Overview

During the last 24 hours, the honeypot recorded a total of:

🔥 55,000+ attacks

These attacks were distributed across multiple honeypot modules:

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Honeypot</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Hits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟥 Cowrie</td>
      <td style="border: 1px solid #ddd; padding: 8px;">~22k</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟧 Honeytrap</td>
      <td style="border: 1px solid #ddd; padding: 8px;">~17k</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟨 Dionaea</td>
      <td style="border: 1px solid #ddd; padding: 8px;">~10k</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟩 Sentrypeer</td>
      <td style="border: 1px solid #ddd; padding: 8px;">~4k</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">💚 Honeyaml</td>
      <td style="border: 1px solid #ddd; padding: 8px;">355</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟩 Adbhoney</td>
      <td style="border: 1px solid #ddd; padding: 8px;">228</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟦 ConPot</td>
      <td style="border: 1px solid #ddd; padding: 8px;">191</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🔵 Miniprint</td>
      <td style="border: 1px solid #ddd; padding: 8px;">147</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">🟥 Redishoneypot</td>
      <td style="border: 1px solid #ddd; padding: 8px;">134</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">💌 Mailoney</td>
      <td style="border: 1px solid #ddd; padding: 8px;">129</td>
    </tr>
  </tbody>
</table>

Cowrie, Honeytrap, and Dionaea remain the highest-interaction sensors and receive the most brute-force and malware-driven traffic.

# 📈 2. Attack Trends Over Time

The Kibana histogram shows:

<ul>
  <li>Continuous scanning activity across the entire 24h window</li>
  <li>Multiple sharp spikes reaching over 4,000 attacks per hour</li>
  <li>A stable background noise of low-frequency automated probes</li>
  <li>Unique source IPs remain lower than total attacks, proving most attacks are automated botnets 🤖</li>
</ul>

# 🛠️ 3. Attacks by Destination Port

Attackers heavily targeted services running on:

<ul>
  <li>445 – SMB file sharing (Windows exploitation attempts)</li>
  <li>5060 – SIP/VoIP scanning</li>
  <li>22 – SSH brute-force</li>
  <li>8728 – MikroTik routers</li>
  <li>3000 – Generic application ports / probing</li>
</ul>

![Snimak ekrana 2025-12-07 132240](https://github.com/user-attachments/assets/5f9e5c79-1225-4310-a890-3d38cac68951)

# 🔐 4. Credential Attacks (Tagcloud Analysis)
👤 Username Tagcloud Highlights

Most attempted usernames:

<ul>
  <li>root</li>
  <li>admin</li>
  <li>ubuntu</li>
  <li>test / test1 / test2</li>
  <li>postgres</li>
  <li>nginx, backup, docker, oracle</li>
  <li>guest, user, developer</li>
</ul>

Attackers overwhelmingly target default system accounts and common admin names.

# 🔑 Password Tagcloud Highlights

Most attempted passwords:

<ul>
  <li>123456 (extremely dominant)</li>
  <li>password, password1</li>
  <li>admin, admin123</li>
  <li>12345, 1234, 654321</li>
  <li>qwerty, qwerty123</li>
  <li>root, root123</li>
  <li>letmein, welcome</li>
  <li>Empty passwords (“”)</li>
  <li>Variations like P@ssw0rd, 123qwe, 123abc</li>
</ul>

🔎 This clearly indicates automated brute-force tools using giant default password dictionaries.

# ⚙️ 7. Security Observations
✔ High rate of automated botnet traffic

SSH (Cowrie), SMB, and SIP ports show continuous brute-force waves.

✔ Global distribution of attacks

Large clusters from US + Europe with frequent single IP hits from Asia suggest mixed scanning sources.

✔ Default credentials remain the #1 target

Brute-force attempts rely heavily on well-known weak passwords.

✔ Industrial/IoT scanning detected

Ports 8728, 5060, and ConPot hits show attackers probing routers, SIP servers, and ICS devices.

