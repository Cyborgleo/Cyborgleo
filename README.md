# 🛡️ SOC Home Lab with AI-Assisted Alert Triage

> A self-built Security Operations Center lab that collects logs from Windows and Linux endpoints, detects simulated attacks mapped to MITRE ATT&CK, and uses a machine-learning / LLM layer to prioritise alerts and cut analyst noise.

![Status](https://img.shields.io/badge/status-active-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE-ATT%26CK-red)

**Author:** Liyon Liju · 📧 [liyonliju1@gmail.com](mailto:liyonliju1@gmail.com) · [LinkedIn](https://linkedin.com/in/YOUR-HANDLE)

---

## 👤 About the Author

I'm **Liyon Liju**, a cybersecurity enthusiast from Kerala, India, with a strong foundation in:

- **Networking**: protocols, TCP/IP, subnetting, firewalls, traffic analysis
- **Operating Systems**: Windows and Linux administration, logging and hardening
- **Cybersecurity fundamentals**: threat detection, vulnerability assessment, incident response
- **AI**: applying machine learning to security problems

This project brings those skills together in a hands-on, reproducible lab.

---

## 📌 Table of Contents

1. [Problem Statement](#-problem-statement)
2. [Key Results](#-key-results)
3. [Architecture](#-architecture)
4. [Tech Stack](#-tech-stack)
5. [Features](#-features)
6. [Detection Coverage (MITRE ATT&CK)](#-detection-coverage-mitre-attck)
7. [AI Triage Module](#-ai-triage-module)
8. [Setup and Installation](#-setup-and-installation)
9. [Usage](#-usage)
10. [Attack Simulation and Validation](#-attack-simulation-and-validation)
11. [Screenshots](#-screenshots)
12. [Lessons Learned](#-lessons-learned)
13. [Roadmap](#-roadmap)
14. [Disclaimer](#-disclaimer)
15. [License](#-license)

---

## 🎯 Problem Statement

SOC analysts face thousands of alerts per day, and most are false positives. This project builds an end-to-end detection pipeline and adds an AI layer that scores and ranks alerts so analysts can focus on the ones that matter.

**Goals**

- Build a realistic, reproducible detection lab using free and open-source tools
- Write and validate detection rules against real attack techniques
- Measure whether AI-assisted triage reduces alert noise without missing true positives

---

## 📊 Key Results

> ⚠️ Replace every placeholder below with your **real, measured** numbers. Never publish figures you did not measure.

| Metric | Result |
|---|---|
| Custom detection rules written | **[X]** |
| MITRE ATT&CK techniques covered | **[X]** across **[X]** tactics |
| Attacks simulated and detected | **[X] / [Y]** ([Z]% detection rate) |
| False-positive reduction with AI triage | **[X]%** |
| AI model precision / recall | **[0.XX] / [0.XX]** |
| Mean time to triage (before → after) | **[X min] → [Y min]** |

---

## 🏗️ Architecture

```
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│ Windows 10/11│   │ Ubuntu Server│   │ Kali (Attack)│
│ Sysmon+Agent │   │  Wazuh Agent │   │  Atomic Red  │
└──────┬───────┘   └──────┬───────┘   └──────┬───────┘
       │  logs            │  logs            │ simulated attacks
       └──────────┬───────┘                  │
                  ▼                          │
        ┌───────────────────┐                │
        │  Wazuh Manager /  │◄───────────────┘
        │  SIEM (Elastic)   │
        └─────────┬─────────┘
                  │ alerts (JSON)
                  ▼
        ┌───────────────────┐
        │  AI Triage Module │  ← feature extraction + ML/LLM scoring
        │  (Python)         │
        └─────────┬─────────┘
                  ▼
        ┌───────────────────┐
        │ Dashboard / Report│  ← prioritised alerts + explanations
        └───────────────────┘
```

*(Replace with a real diagram exported from draw.io or Excalidraw and saved in `/docs/architecture.png`.)*

---

## 🧰 Tech Stack

| Layer | Tools |
|---|---|
| SIEM / EDR | Wazuh, Elastic Stack (or Splunk Free) |
| Endpoint telemetry | Sysmon, Auditd |
| Attack simulation | Atomic Red Team, Kali Linux, Metasploit |
| AI / ML | Python, scikit-learn, pandas *(add LLM API if used)* |
| Datasets | CICIDS2017 / UNSW-NB15 *(list what you actually used)* |
| Virtualisation | VirtualBox / VMware, pfSense (network segmentation) |
| Frameworks | MITRE ATT&CK, NIST SP 800-61 |

---

## ✨ Features

- **Centralised logging** from Windows and Linux endpoints
- **Custom detection rules** for credential access, persistence, lateral movement and more
- **ATT&CK-mapped alerts** with tactic and technique IDs
- **AI-based alert scoring** to rank alerts by likelihood of being malicious
- **Explainable output**: each score shows the top contributing features
- **Reproducible lab**: setup scripts and configs included

---

## 🗺️ Detection Coverage (MITRE ATT&CK)

| Tactic | Technique | ID | Rule File | Validated |
|---|---|---|---|---|
| Credential Access | OS Credential Dumping | T1003 | `rules/t1003_lsass.xml` | ✅ |
| Persistence | Scheduled Task | T1053 | `rules/t1053_schtasks.xml` | ✅ |
| Execution | PowerShell | T1059.001 | `rules/t1059_powershell.xml` | ✅ |
| Discovery | Account Discovery | T1087 | `rules/t1087_discovery.xml` | ⬜ |

*(Edit this table to match the rules you actually built.)*

---

## 🤖 AI Triage Module

**Approach:** Alerts exported from the SIEM are converted into features (rule level, frequency, source/destination, time of day, process lineage, etc.) and scored by a classifier trained on labelled data.

**Model:** `[Random Forest / XGBoost / LLM-based classifier]`

**Evaluation**

| Metric | Value |
|---|---|
| Accuracy | [0.XX] |
| Precision | [0.XX] |
| Recall | [0.XX] |
| F1-score | [0.XX] |
| False-positive rate | [0.XX] |

**Limitations:** Trained on public datasets plus lab-generated data, so results may not transfer to production environments. See [Lessons Learned](#-lessons-learned).

---

## ⚙️ Setup and Installation

### Prerequisites

- 16 GB RAM recommended, 100 GB free disk
- VirtualBox or VMware
- Python 3.10+
- Git

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/YOUR-USERNAME/soc-ai-triage-lab.git
cd soc-ai-triage-lab

# 2. Create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Deploy the lab (see /docs/lab-setup.md for full VM instructions)
```

Full lab build guide: [`docs/lab-setup.md`](docs/lab-setup.md)

---

## ▶️ Usage

```bash
# Export alerts from the SIEM
python src/export_alerts.py --since 24h --out data/alerts.json

# Train the triage model
python src/train.py --data data/labelled_alerts.csv

# Score and rank new alerts
python src/triage.py --input data/alerts.json --top 20
```

**Sample output**

```
Rank  Score  Rule                         Technique   Host
1     0.97   LSASS memory access          T1003       WIN10-01
2     0.91   Encoded PowerShell command   T1059.001   WIN10-01
3     0.84   New scheduled task created   T1053       WIN10-01
```

---

## 🧪 Attack Simulation and Validation

Detections were validated using Atomic Red Team tests and manual attacks from Kali.

```powershell
Invoke-AtomicTest T1003 -TestNumbers 1
```

Each test is documented in [`docs/validation.md`](docs/validation.md) with: technique, command run, expected alert, actual alert, and pass/fail.

---

## 🖼️ Screenshots

| SIEM Dashboard | Alert Detail | AI Ranking |
|---|---|---|
| ![dashboard](docs/img/dashboard.png) | ![alert](docs/img/alert.png) | ![ranking](docs/img/ranking.png) |

---

## 💡 Lessons Learned

- **[Example]** Default rules produced excessive noise. Tuning by [method] cut false positives by [X]%.
- **[Example]** Class imbalance skewed the model. Fixed with [SMOTE / class weights / resampling].
- **[Example]** Log volume from Sysmon required a tuned config to stay manageable.

*(Write 3 to 5 honest lessons from your own build. Reviewers value this section highly.)*

---

## 🚀 Roadmap

- [ ] Add SOAR-style automated response (e.g., block IP, isolate host)
- [ ] Integrate threat intelligence feeds (MISP / AbuseIPDB)
- [ ] Extend to cloud logs (AWS CloudTrail)
- [ ] Add adversarial testing of the triage model

---

## ⚠️ Disclaimer

This project is for **educational and defensive purposes only**. All attacks were run inside an isolated lab on systems I own. Do not use these techniques against systems you do not have explicit permission to test.

---

## 📄 License

Distributed under the MIT License. See [`LICENSE`](LICENSE) for details.

---

## 🙋 Contact

**Liyon Liju**  
📧 [liyonliju1@gmail.com](mailto:liyonliju1@gmail.com) · 💼 [LinkedIn](https://linkedin.com/in/YOUR-HANDLE)

⭐ If you found this project useful, please star the repo!
