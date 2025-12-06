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
Explanation:
This command initiates a secure SSH connection from your local machine to the remote server using the root account. Replace publicIPaddress with your server’s actual public IP. From PowerShell, this opens an encrypted session where you can run administrative commands.

## 2. 📦 Update and Upgrade System Packages
apt-get update && apt-get upgrade -y
Explanation:
apt-get update refreshes the package index, ensuring your system knows about the latest available versions of all packages.
apt-get upgrade -y automatically installs all available updates.
This step ensures your server is fully patched before installing T-Pot.

## 3. 👤 Create a New User
adduser username
Explanation:
Creates a new user account on the system. Replace username with the name you want to assign. You’ll be prompted to set a password and optional user details.

## 4. 🔑 Grant Sudo Privileges
sudo usermod -aG sudo username
Explanation:
Adds the new user to the sudo group, giving them administrative privileges. This is a best practice so that you don’t continue using the root account for routine tasks.

## 5. 🔄 Switch to the New User
su username
Explanation:
Switches from the root user to the newly created user account. This ensures the installation and repository setup happen under a safer, non-root environment.

## 6. 📁 Navigate to the User’s Home Directory
cd /home/username
Explanation:
Moves into the home directory of the new user, where your cloned repository and installation files will be stored.

## 7. 📥 Clone Your Repository
git clone repos
Explanation:
Clones the repository containing the T-Pot CE files or your fork of the project. Replace repos with the actual Git URL.

## 8. 📂 Enter the T-Pot Directory
cd tpotce/
Explanation:
Navigates into the cloned repository folder where the T-Pot installation script is located.

## 9. ⚙️ Run the T-Pot Installation Script
./install.sh
Explanation:
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
Explanation:
Reboots the server so the new SSH configuration, services, and T-Pot components fully activate.

# 🔑 Connecting Using the New SSH Port
ssh -p sshportnumber root@publicIPaddress
Explanation:
-p sshportnumber specifies the new SSH port assigned by T-Pot (e.g., 64297).
You must now use this port every time you connect via SSH.

# 🌐 Accessing the T-Pot Web Interface
https://publicIPaddress:sshportnumber
Explanation:
T-Pot’s web UI (Cockpit + dashboards) also listens on the same high port that SSH was moved to (for example, https://yourIP:64297).
This port is chosen dynamically by the installer to avoid conflicts with the honeypots, many of which use standard ports (22, 80, 443, etc.) to mimic real services.

The high port—e.g., 64297—acts as a secure management port for both:

🔐 SSH administration

📊 Web dashboard interface

# 🔁 Restarting T-Pot Services
systemctl restart tpot
Explanation:
Restarts all T-Pot honeypot services, dashboards, and supporting components without rebooting the whole machine.
Useful after configuration updates or troubleshooting.
