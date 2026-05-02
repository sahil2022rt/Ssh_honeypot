<div align="center">

<img src="assets/images/honeypy-logo-white.png" alt="HoneyPy Logo" width="300"/>

# 🍯 HoneyPy

**A Python-based honeypot framework for capturing attacker credentials, commands, and behavior in real-time.**

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0.3-black?style=for-the-badge&logo=flask)](https://flask.palletsprojects.com/)
[![Paramiko](https://img.shields.io/badge/Paramiko-3.4.0-green?style=for-the-badge)](https://www.paramiko.org/)
[![Dash](https://img.shields.io/badge/Dash-2.17.1-orange?style=for-the-badge)](https://dash.plotly.com/)
[![License](https://img.shields.io/badge/License-MIT-red?style=for-the-badge)](LICENSE)

</div>

---

## 📖 Overview

HoneyPy is a lightweight, modular honeypot tool built in Python that simulates both SSH and HTTP (WordPress) servers to **lure, log, and analyze attackers**. It captures login credentials, commands executed during sessions, and geographic data — all visualized through a sleek real-time dashboard.

Whether you're a cybersecurity researcher, student, or blue teamer, HoneyPy gives you hands-on insight into how attackers behave in the wild.

---

## ✨ Features

- 🔐 **SSH Honeypot** — Emulates a corporate SSH jumpbox; logs all login attempts and shell commands
- 🌐 **HTTP Honeypot** — Mimics a WordPress admin login page to capture credential stuffing attacks
- 🪤 **Tarpit Mode** — Slows attackers down by sending an endless SSH banner (anti-scanner)
- 📊 **Live Analytics Dashboard** — Plotly/Dash-powered dashboard with Top 10 IPs, usernames, passwords, and commands
- 🌍 **IP Geolocation** — Maps attacker IPs to countries via the CleanTalk API
- 📁 **Rotating Log Files** — Structured logs for credentials and commands with automatic rotation

---

## 🖼️ Screenshots

<div align="center">

### 📋 HTTP Audit Log
Captures timestamped login attempts with IP, username, and password.

![HTTP Audit Log](<img width="1152" height="864" alt="WhatsApp Image 2026-05-02 at 5 45 21 PM" src="https://github.com/user-attachments/assets/79c9b333-6696-4289-aee3-c25488fdbf90" />
)

### 🖥️ SSH Honeypot in Action
SSH server listening and accepting connections, logging all activity.

![SSH Honeypot Running](<img width="1600" height="1200" alt="WhatsApp Image 2026-05-02 at 5 45 22 PM" src="https://github.com/user-attachments/assets/30dfbeac-2417-4daf-920b-e4f312b1eb97" />
)(<img width="1599" height="899" alt="WhatsApp Image 2026-05-02 at 5 45 24 PM" src="https://github.com/user-attachments/assets/5fc4373e-395d-4802-8a0b-d12cd89e15d5" />
)

### 🔑 Server Key Generation
RSA host key used to authenticate the fake SSH server.

![Server Key](<img width="1600" height="1200" alt="WhatsApp Image 2026-05-02 at 5 45 22 PM" src="https://github.com/user-attachments/assets/cf565c8c-0cd2-49e8-b0d4-7ef2f70f261a" />
)
<img width="1600" height="1200" alt="WhatsApp Image 2026-05-02 at 5 45 23 PM" src="https://github.com/user-attachments/assets/db453a37-1401-47e9-b4b2-523f7d6e56f1" />

</div>

---

## 🏗️ Project Structure

```
ssh_honeypy/
├── honeypy.py                 # Main entry point (CLI)
├── ssh_honeypot.py            # SSH honeypot logic (Paramiko)
├── web_honeypot.py            # HTTP WordPress honeypot (Flask)
├── web_app.py                 # Analytics dashboard (Dash)
├── dashboard_data_parser.py   # Log file parsers & data utilities
├── requirements.txt           # Python dependencies
├── public.env                 # Environment config (country lookup toggle)
└── ssh_honeypy/
    ├── static/
    │   └── server.key         # RSA host key
    ├── log_files/
    │   ├── creds_audits.log   # Captured credentials
    │   ├── cmd_audits.log     # Captured commands
    │   └── http_audit.log     # HTTP login attempts
    └── templates/
        └── wp-admin.html      # Fake WordPress login page
```

---

## ⚙️ Installation

### 1. Clone the Repository

```bash
(https://github.com/sahil2022rt/Ssh_honeypot.git)
cd honeypot
```

### 2. Create a Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate       # Linux/macOS
.venv\Scripts\activate          # Windows
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Generate an SSH Host Key

```bash
ssh-keygen -t rsa -b 2048 -f ssh_honeypy/static/server.key
```

### 5. Configure Environment

Edit `public.env` to toggle country code lookups:

```env
COUNTRY=False   # Set to True to enable IP geolocation (slower)
```

---

## 🚀 Usage

### Run the SSH Honeypot

```bash
# Basic (accepts any credentials)
python honeypy.py -a 0.0.0.0 -p 2222 -s

# With specific credentials (only logs, accepts correct combo)
python honeypy.py -a 0.0.0.0 -p 2222 -s -u admin -w password123

# With Tarpit mode (slows down scanners)
python honeypy.py -a 0.0.0.0 -p 2222 -s -t
```

### Run the HTTP (WordPress) Honeypot

```bash
# Default credentials (admin / deeboodah)
python honeypy.py -a 0.0.0.0 -p 8080 -wh

# Custom credentials
python honeypy.py -a 0.0.0.0 -p 8080 -wh -u myadmin -w mypassword
```

### Launch the Analytics Dashboard

```bash
python web_app.py
# Open browser at: http://localhost:8050
```

---

## 🧩 CLI Arguments

| Argument | Short | Description |
|---|---|---|
| `--address` | `-a` | IP address to bind the honeypot |
| `--port` | `-p` | Port to listen on |
| `--username` | `-u` | Expected username (optional) |
| `--password` | `-w` | Expected password (optional) |
| `--ssh` | `-s` | Run SSH honeypot mode |
| `--http` | `-wh` | Run HTTP WordPress honeypot mode |
| `--tarpit` | `-t` | Enable tarpit (endless SSH banner) |

---

## 📊 Dashboard Preview

The dashboard (powered by Plotly Dash + Bootstrap Cyborg theme) visualizes:

- **Top 10 Attacker IPs**
- **Top 10 Usernames Attempted**
- **Top 10 Passwords Attempted**
- **Top 10 Commands Executed**
- **Country Code Distribution** *(optional, requires CleanTalk API)*
- **Raw Intelligence Data Tables**

---

## 📦 Dependencies

| Package | Version | Purpose |
|---|---|---|
| `paramiko` | 3.4.0 | SSH server emulation |
| `flask` | 3.0.3 | HTTP honeypot web server |
| `dash` | 2.17.1 | Analytics dashboard |
| `plotly` | 5.23.0 | Data visualization |
| `pandas` | 2.2.2 | Log data processing |
| `dash-bootstrap-components` | 1.6.0 | Dashboard UI components |
| `python-dotenv` | 1.0.1 | Environment variable management |
| `requests` | 2.32.3 | CleanTalk geolocation API calls |

---

## ⚠️ Disclaimer

> **HoneyPy is intended for educational and research purposes only.**
> Deploy only on systems and networks you own or have explicit written permission to monitor.
> Running a honeypot on unauthorized networks may violate local laws and regulations.
> The authors assume no liability for misuse.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Made with ❤️ for the cybersecurity community

⭐ **Star this repo if you found it useful!** ⭐

</div>
