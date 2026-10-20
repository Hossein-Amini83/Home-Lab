# Home Lab Pentest Write-ups

Practical penetration testing / red team exercises and write-ups from my personal home lab, documenting my progress from zero to professional level.

🇮🇷 [نسخه فارسی](README.fa.md)

## About

This repository is a running log of hands-on penetration testing exercises I perform in an isolated home lab, along with a structured write-up for each one. The goal is to build real, demonstrable skills in network security and red teaming, one exercise at a time.

## Lab Environment

- **Attacker machine:** Kali Linux
- **Targets:**
  - Metasploitable 2 (Linux)
  - Windows Server 2025 (Server Core)
  - Windows 10
- **Network:** Host-only, isolated virtual network (`192.168.56.0/24`)

All testing is performed exclusively against machines I own and control, in an isolated lab with no internet-facing exposure.

## Current Skills / Tools

- Linux (daily user)
- Basic networking (IP addressing, subnetting, core protocols)
- Nmap
- Wireshark
- John the Ripper
- Hydra

*(Updated as new tools/skills are learned.)*

## Repository Structure

```
.
├── README.md
├── README.fa.md
└── writeups/
    ├── day-01-<topic>/
    │   └── README.md
    ├── day-02-<topic>/
    │   └── README.md
    └── ...
```

## Write-up Template

Each write-up follows this structure:

- **Title**
- **Scenario**
- **Objective**
- **Target machine**
- **Tools used**
- **Key commands**
- **Vulnerability discovered**
- **Lessons learned**

## Index of Write-ups

| # | Title | Target | Date |
|---|-------|--------|------|
| 01 | _TBD_ | _TBD_ | _TBD_ |

## Disclaimer

All exercises are conducted in a private, isolated lab environment against systems I own. This repository is for educational purposes only.
