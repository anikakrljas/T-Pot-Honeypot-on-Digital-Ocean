# 🐝 T-Pot Honeypot Setup on Digital Ocean ☁️

Welcome to my T-Pot Honeypot project! 

T-Pot is an all-in-one honeypot platform that combines multiple honeypot technologies to capture and analyze attacks in real-time. It’s perfect for research and security monitoring to understand attacker behaviors and techniques. 

I decided to deploy T-Pot on <a href="https://www.digitalocean.com/">Digital Ocean</a>, a cloud computing platform that provides simple and scalable virtual machines (Droplets) to host applications. Digital Ocean makes it easy to set up a server quickly without worrying about hardware management. 

# 🛠 Creating a Droplet
## 1. Go to Create Droplet

Log into your Digital Ocean account.
Click **Create** → **Droplet**.

## 2. Choose an Image

Select **Ubuntu 24.04 LTS**.
Ubuntu is a user-friendly Linux operating system widely used for servers.
* **24.04** → Released in April 2024
* **LTS** → Long Term Support (5 years of updates)
* **x64** → 64-bit architecture, supports modern applications and more memory

![ISO image](https://github.com/user-attachments/assets/f2fd83f3-d96f-4e99-a403-a145bf6ffefb)

## 3. Choose a Region

Pick a region closest to you for better latency. 
Some examples:
* 🇺🇸 **New York**
* 🇩🇪 **Frankfurt**
* 🇸🇬 **Singapore**
* 🇦🇺 **Sydney**

![Choose Region](https://github.com/user-attachments/assets/b6ac6764-bd40-4eea-afd2-a950ca497acd)

## 4. Select CPU, Memory, and Disk

Choose the CPU and RAM based on your expected load.
Select disk type (SSD recommended) and size.
Optionally, enable automatic backups (recommended but not mandatory).

## 5. Authentication

Set up authentication:
* **Password** for root user OR
* **SSH key** for secure login
![Password](https://github.com/user-attachments/assets/6e658cd1-99c3-43be-9495-1d2136a84e80)

## 6. Create Droplet

Click **Create Droplet** and wait for the server to be provisioned.
You will receive the IP address to access your T-Pot honeypot. 

In the **Graphs** section of the droplet created on Digital Ocean Cloud, you can monitor bandwidth, CPU usage, and disk I/O.

![Droplet graphs](https://github.com/user-attachments/assets/dadd742d-aa71-41ac-898e-e5503c2cf425)


# 🚨 Deploying T-Pot Honeypot via PowerShell (SSH)

This section documents the exact steps I used to deploy T-Pot, a multi-honeypot platform, on my server using PowerShell and SSH. Each command is explained in detail to help others fully understand the setup process.

## 1. 🔐 Connect to Your Server
```ssh root@publicIPaddress```

This command initiates a secure SSH connection from your local machine to the remote server using the root account. Replace publicIPaddress with your server’s actual public IP. From PowerShell, this opens an encrypted session where you can run administrative commands.

## 2. 📦 Update and Upgrade System Packages
```apt-get update && apt-get upgrade -y```

apt-get update refreshes the package index, ensuring your system knows about the latest available versions of all packages.
apt-get upgrade -y automatically installs all available updates.
This step ensures your server is fully patched before installing T-Pot.

## 3. 👤 Create a New User
```adduser username```

Creates a new user account on the system. Replace username with the name you want to assign. You’ll be prompted to set a password and optional user details.

## 4. 🔑 Grant Sudo Privileges
```sudo usermod -aG sudo username```

Adds the new user to the sudo group, giving them administrative privileges. This is a best practice so that you don’t continue using the root account for routine tasks.

## 5. 🔄 Switch to the New User
```su username```

Switches from the root user to the newly created user account. This ensures the installation and repository setup happen under a safer, non-root environment.

## 6. 📁 Navigate to the User’s Home Directory
```cd /home/username```

Moves into the home directory of the new user, where your cloned repository and installation files will be stored.

## 7. 📥 Clone Your Repository
```git clone repository```

Clones the repository containing the T-Pot CE files or your fork of the project. Replace repository with the actual Git URL.

## 8. 📂 Enter the T-Pot Directory
```cd tpotce/```

Navigates into the cloned repository folder where the T-Pot installation script is located.

## 9. ⚙️ Run the T-Pot Installation Script
```./install.sh```

Executes the official T-Pot installation script. The script will guide you through configuration options and set up multiple honeypot services, dashboards, and data pipelines.
Make sure the script is executable (chmod +x install.sh) if needed.

**🔄 Changing the SSH Port During T-Pot Installation**

During the T-Pot installation process, the setup wizard automatically changes the default SSH port from 22 to a randomly assigned high port number.
This is done for security reasons, because T-Pot exposes multiple honeypots on common ports, and keeping SSH on 22 would make the real system vulnerable or interfere with honeypot services.

![T-Pot types](https://github.com/user-attachments/assets/111bd295-fa72-46ad-a01b-2229d116a186)

## 🧰 T-Pot Deployment Profiles 

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

## 10. 🔄 Reboot After Installation
```sudo reboot```

Reboots the server so the new SSH configuration, services, and T-Pot components fully activate.

## 11. 🔑 Connecting Using the New SSH Port
```ssh -p sshportnumber root@publicIPaddress```

-p sshportnumber specifies the new SSH port assigned by T-Pot (e.g., 64297).
You must now use this port every time you connect via SSH.

## 12. 🌐 Accessing the T-Pot Web Interface
https://publicIPaddress:sshportnumber

T-Pot’s web UI (Cockpit + dashboards) also listens on the same high port that SSH was moved to (for example, https://yourIP:64297).
This port is chosen dynamically by the installer to avoid conflicts with the honeypots, many of which use standard ports (22, 80, 443, etc.) to mimic real services.

The high port e.g., 64297 acts as a secure management port for both:

🔐 SSH administration

📊 Web dashboard interface

## 13. 🔁 Restarting T-Pot Services
```systemctl restart tpot```

Restarts all T-Pot honeypot services, dashboards, and supporting components without rebooting the whole machine.
Useful after configuration updates or troubleshooting.

# T-Pot Honeypot

![T-Pot Honeypot](https://github.com/user-attachments/assets/31f8f62c-7835-4339-9c66-44bcef035a0b)

## 📊 T-Pot Honeypot – 24-Hour Attack Report

![Attack Map](https://github.com/user-attachments/assets/6807c393-3275-4667-8091-737cd811c7db)

Time window: Last 24 hours
Total recorded events: ≈ 51,463 attacks
Your honeypot sensors captured a large amount of malicious traffic from multiple countries across the globe. Below is a breakdown of the observed activity by country, protocol, service, and general attack behavior.

### 🌍 1. Geographic Distribution of Attacks

Based on the “Top Countries by Hits” section in your dashboard:

Top 5 Attacking Countries

* 🇺🇸 **United States – 6079 hits**
  Very active across multiple services, especially SSH and SMB.
* 🇮🇱 **Israel – 3037 hits**
  Strong activity, likely automated scanning frameworks.
* 🇳🇱 **The Netherlands – 1932 hits**
  Common hotspot for cloud-origin traffic.
* 🇭🇰 **Hong Kong – 1927 hits**
  Significant scanning activity, possibly botnets.
* 🇭🇺 **Hungary – 1477 hits**
  Local/regional scanning noted.

### 🔌 2. Services / Protocols Targeted

The color legend in the interface shows:

| Color | Service | Description |
| :--- | :--- | :--- |
| 🔴 Red | FTP | Likely brute force attempts |
| 🟠 Orange | SSH | One of the most attacked services—login brute force, key probing |
| 🟡 Yellow | TELNET | Very common with IoT malware |
| 🟢 Green | EMAIL | SMTP spam attempts or probing |
| ⚪ White | SQL | Database enumeration/injection attempts |

### 🔍 3. Top Attacking IPs

Your dashboard lists the IPs with the highest number of hits:

* 🇮🇱 **141.226.93.223** – 3036 hits
* 🇭🇰 **185.243.5.185** – 1724 hits
* 🇭🇺 **31.46.245.29** – 1476 hits
* 🇺🇸 **198.143.191.202** – 1029 hits
* 🇺🇸 **148.72.169.42** – 763 hits

Most of these are likely automated bots or compromised servers performing broad-spectrum scanning.

## 📘 T-Pot Honeypot – Kibana 24h Attack Report

This report summarizes all attacks recorded by the T-Pot honeypot environment during the last 24 hours, visualized through Kibana dashboards.
The goal is to provide a clear, readable, and insightful overview of attacker behavior, targeted services, ports, and credential attempts across all honeypot sensors.

![Kibana](https://github.com/user-attachments/assets/7945021d-edd1-44f0-b183-5a946fbc228e)


### 📊 1. Total Attacks Overview

During the last 24 hours, the honeypot recorded a total of:

🔥 50,000+ attacks

These attacks were distributed across multiple honeypot modules:

| Honeypot | Hits |
| :--- | :--- |
| 🟥 Cowrie | ~22k |
| 🟧 Honeytrap | ~17k |
| 🟨 Dionaea | ~10k |
| 🟩 Sentrypeer | ~4k |
| 💚 Honeyaml | 355 |
| 🟩 Adbhoney | 228 |
| 🟦 ConPot | 191 |
| 🔵 Miniprint | 147 |
| 🟥 Redishoneypot | 134 |
| 💌 Mailoney | 129 |

Cowrie, Honeytrap, and Dionaea remain the highest-interaction sensors and receive the most brute-force and malware-driven traffic.

### 📈 2. Attack Trends Over Time

The Kibana histogram shows:

* Continuous scanning activity across the entire 24h window
* Multiple sharp spikes reaching over 4,000 attacks per hour
* A stable background noise of low-frequency automated probes
* Unique source IPs remain lower than total attacks, proving most attacks are automated botnets 🤖

### 🛠️ 3. Attacks by Destination Port

Attackers heavily targeted services running on:

* 445 – SMB file sharing (Windows exploitation attempts)
* 5060 – SIP/VoIP scanning
* 22 – SSH brute-force
* 8728 – MikroTik routers
* 3000 – Generic application ports / probing

### 🔐 4. Credential Attacks (Tagcloud Analysis)

![User/Password attacks](https://github.com/user-attachments/assets/5f9e5c79-1225-4310-a890-3d38cac68951)

👤 Username Tagcloud Highlights

Most attempted usernames:

* root
* admin
* ubuntu
* test / test1 / test2
* postgres
* nginx, backup, docker, oracle
* guest, user, developer

Attackers overwhelmingly target default system accounts and common admin names.

🔑 Password Tagcloud Highlights

Most attempted passwords:

* 123456 (extremely dominant)
* password, password1
* admin, admin123
* 12345, 1234, 654321
* qwerty, qwerty123
* root, root123
* letmein, welcome
* Empty passwords (“”)
* Variations like P@ssw0rd, 123qwe, 123abc

🔎 This clearly indicates automated brute-force tools using giant default password dictionaries.

### ⚙️ 5. Security Observations
✔ High rate of automated botnet traffic

SSH (Cowrie), SMB, and SIP ports show continuous brute-force waves.

✔ Global distribution of attacks

Large clusters from US + Europe with frequent single IP hits from Asia suggest mixed scanning sources.

✔ Default credentials remain the #1 target

Brute-force attempts rely heavily on well-known weak passwords.

✔ Industrial/IoT scanning detected

Ports 8728, 5060, and ConPot hits show attackers probing routers, SIP servers, and ICS devices.

## 🔹 CyberChef: The Cyber Swiss Army Knife 🔹

CyberChef, often called the “Cyber Swiss Army Knife” 🛠️, is a web-based tool developed by GCHQ for performing a wide variety of data analysis, manipulation, and encryption tasks. It’s designed for both beginners and professionals in cybersecurity, digital forensics, and data analysis. The tool allows users to process data through an intuitive drag-and-drop interface, using “recipes” that chain together different operations.

![CyberChef](https://github.com/user-attachments/assets/74c5bf01-f6c4-427c-b4c3-1a555fdc9180)

Key Features:
* 🔐 **Data Encoding & Decoding:** Convert between Base64, Hex, URL encoding, and more.
* 🛡️ **Encryption & Decryption:** Support for AES, XOR, ROT13, and other cryptographic algorithms.
* 📊 **Hashing & Checksums:** Calculate MD5, SHA-1, SHA-256, and other hashes.
* 🔍 **Data Extraction:** Extract useful information from logs, emails, or network packets.
* ⚡ **Analysis & Transformation:** Perform pattern matching, regex searches, or byte-level operations.

Why It’s Useful in Cybersecurity:
1. **Incident Response:** Quickly decode and analyze suspicious files, emails, or network traffic.
2. **Malware Analysis:** Inspect payloads, decrypt strings, or reverse engineer obfuscated code.
3. **Forensics:** Extract hidden information from files or logs to understand an attack or breach.
4. **Rapid Prototyping:** Create custom “recipes” to automate repetitive analysis tasks.
5. **Education & Training:** Visual interface helps beginners learn cryptography and encoding concepts easily.

Why It Stands Out:
* ✨ **Intuitive Interface:** No need to write scripts; drag and drop operations in a logical sequence.
* 🧰 **All-in-One:** Combines multiple cybersecurity tasks in a single tool.
* 🌐 **Open Source:** Freely available and continuously improved by the community.

CyberChef is a versatile tool for anyone working in cybersecurity 💻. Its ability to quickly manipulate, decode, and analyze data makes it an essential part of threat hunting, digital forensics, and malware analysis. With CyberChef, complex tasks become simple, making it a “must-have” tool for security professionals and enthusiasts alike 🚀.

## 🕷️ SpiderFoot: The Automated OSINT Tool 🕷️

SpiderFoot is an open-source intelligence (OSINT) automation tool that helps security professionals gather information about IPs, domains, email addresses, names, and more 🌐. It automates the collection and correlation of data from hundreds of sources, making reconnaissance faster, easier, and more accurate. SpiderFoot is designed for both beginners and experts in cybersecurity, threat intelligence, and digital forensics.

![SpiderFoot](https://github.com/user-attachments/assets/1b1ff939-87fc-4d44-a2c4-f20384c49716)

Key Features:

* 🔎 **Automated Reconnaissance:** Scan targets to gather intelligence from open sources automatically.
* 🌐 **Wide Source Integration:** Pull data from public databases, DNS records, social media, breach data, and more.
* 📊 **Data Correlation:** Link related data points to discover hidden relationships.
* ⚡ **Flexible Scanning:** Scan IPs, domains, emails, names, or ASNs with minimal manual setup.
* 🛡️ **Threat Intelligence:** Identify potential vulnerabilities, leaks, or suspicious activity early.

Why It’s Useful in Cybersecurity:

1. **Recon & Footprinting:** Map out an organization’s digital footprint efficiently.
2. **Threat Hunting:** Detect potential attack surfaces or compromised assets.
3. **Security Assessment:** Identify exposed data or weak points before attackers do.
4. **Research & Education:** Learn OSINT techniques and gather real-world data safely.
5. **Automation:** Save time with automated scanning and reporting, reducing human error.

Why It Stands Out:

* ✨ **Highly Automated:** Minimal manual input required to gather massive amounts of intelligence.
* 🧰 **All-in-One OSINT Tool:** Combines hundreds of sources into a single platform.
* 🌐 **Open Source:** Continuously improved by the community and fully customizable.

SpiderFoot is an essential tool for cybersecurity professionals 💻, OSINT investigators, and threat intelligence analysts. Its ability to automatically collect and correlate vast amounts of data helps identify vulnerabilities, track threat actors, and strengthen digital defenses 🛡️. Whether you’re a beginner or an expert, SpiderFoot simplifies reconnaissance and makes intelligence gathering efficient and effective 🚀.

## 📊 Elasticvue: Elasticsearch Management Made Easy 📊

Elasticvue is a powerful web-based tool for managing and visualizing Elasticsearch clusters 🔍. It provides a clean and intuitive interface for querying, monitoring, and administering Elasticsearch data. Elasticvue is designed for developers, system administrators, and security professionals who work with Elasticsearch on a daily basis.

![Elasticvue](https://github.com/user-attachments/assets/dc99b11d-a4b4-417b-94c6-a2f5a2ff82c0)

Key Features:

* 🗃️ **Cluster Overview:** Get real-time statistics about your Elasticsearch nodes, indices, and cluster health.
* 🔎 **Data Exploration:** Browse indices, documents, and mappings with an easy-to-use interface.
* ⚡ **Querying & Filtering:** Run Elasticsearch queries, filters, and aggregations quickly and visually.
* 🛠️ **Index Management:** Create, delete, or update indices and mappings directly from the interface.
* 📊 **Visualization:** Visualize document counts, field distributions, and cluster metrics.

Why It’s Useful in Cybersecurity:

1. **Log Analysis:** Quickly analyze logs and events for suspicious activity or incidents.
2. **Monitoring:** Keep track of Elasticsearch cluster health and performance to avoid failures.
3. **Threat Hunting:** Query and correlate security logs to identify potential attacks.
4. **Automation & Admin:** Manage indices, mappings, and settings without using command-line tools.
5. **Learning & Debugging:** Explore Elasticsearch queries and understand data structures easily.

Why It Stands Out:

* ✨ **Intuitive Interface:** Visual and interactive UI makes Elasticsearch management easy.
* 🧰 **All-in-One Tool:** Combines querying, monitoring, and cluster management in a single platform.
* 🌐 **Open Source:** Freely available, actively maintained, and community-driven.

Elasticvue is an essential tool for anyone working with Elasticsearch 💻. Its clean interface, powerful querying capabilities, and monitoring tools make it ideal for developers, sysadmins, and security analysts alike 🛡️. Whether analyzing logs, managing clusters, or hunting for threats, Elasticvue simplifies the process and enhances productivity 🚀.
