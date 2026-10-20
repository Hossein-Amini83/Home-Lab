# Day 01 — Initial Network Reconnaissance

## 1. Scenario

A small fictional company has asked me to perform initial reconnaissance of its internal network before a penetration test. The network documentation is incomplete, and the IT team has provided only the lab network range.

My task is to identify responsive hosts in the lab network and determine which TCP ports are accessible on one of the discovered servers.

## 2. Scope

I used `ip a` to inspect the network addresses assigned to the attacker machine (Kali Linux). In addition to the `loopback` interface, two network interfaces were identified:

| Interface | IPv4 address | Broadcast address | Network type |
|---|---|---|---|
| `eth0` | `192.168.56.102/24` | `192.168.56.255` | Lab Host-Only network |
| `eth1` | `10.0.3.15/24` | `10.0.3.255` | NAT, according to the VM configuration |

Usable address ranges for these `/24` subnets:

- `eth0`: `192.168.56.1`–`192.168.56.254`
- `eth1`: `10.0.3.1`–`10.0.3.254`

> These ranges are calculated from the `/24` masks; they do not mean that every address is assigned to an active host. This exercise is limited to the Host-Only lab network.

## 3. Objective

Identify the target host and document its open TCP ports and detectable services. Gaining access to the target or exploiting vulnerabilities is outside the scope of this exercise.

## 4. Tools Used

- Nmap
- Linux networking utilities, including `ip`

## 5. Reconnaissance

- `eth0`: interface connected to the lab Host-Only network
- `eth1`: NAT interface, according to the VM configuration
- Host-Only network range: `192.168.56.0/24`
- NAT network range: `10.0.3.0/24`

## 6. Host Discovery

**Command used:**

```bash
nmap -sn 192.168.56.0/24 -n
```

**Observed or identified hosts:**

1. `192.168.56.1` — likely the host interface or a Host-Only networking service; its exact role should be verified in the network configuration.
2. `192.168.56.102` — Kali Linux; the address was confirmed with `ip a`.
3. `192.168.56.101` — likely the Metasploitable 2 target.
4. `192.168.56.100` — associated with another VM that was powered off according to the VM status; the scan alone does not prove that it is powered off.

**Selected target:** Metasploitable 2 (`192.168.56.101`)

**Reason for selection:** The operating-system detection output from the following Nmap command was consistent with the expected target. OS detection alone does not conclusively establish a host's identity.

```bash
nmap -O 192.168.56.101
```

## 7. Port Scanning

**Command used:**

```bash
nmap -sV 192.168.56.101
```

This scan was used to examine selected TCP ports and identify likely services and versions. The table below reflects the recorded Nmap output.

| Port | State | Service | Reported version or details |
|---|---|---|---|
| `21/tcp` | Open | FTP | vsftpd 2.3.4 |
| `22/tcp` | Open | SSH | OpenSSH 4.7p1 Debian 8Ubuntu1 (protocol 2.0) |
| `23/tcp` | Open | Telnet | Linux telnetd |
| `25/tcp` | Open | SMTP | Postfix smtpd |
| `53/tcp` | Open | DNS / DOMAIN | ISC BIND 9.4.2 |
| `80/tcp` | Open | HTTP | Apache httpd 2.2.8 ((Ubuntu) DAV/2) |
| `111/tcp` | Open | RPCBind | 2 (RPC #100000) |
| `139/tcp` | Open | NetBIOS-SSN | Samba smbd 3.X–4.X (workgroup: WORKGROUP) |
| `445/tcp` | Open | SMB / NetBIOS-SSN | Samba smbd 3.X–4.X (workgroup: WORKGROUP) |
| `512/tcp` | Open | exec | netkit-rshd |
| `513/tcp` | Open | login | OpenBSD or Solaris rlogind (tentative identification) |
| `514/tcp` | Open | shell | netkit-rshd |
| `1099/tcp` | Open | RMI Registry | GNU Classpath grmiregistry |
| `1524/tcp` | Open | bindshell | Identified in the Nmap output as “Metasploitable root shell” |
| `2049/tcp` | Open | NFS | Version 2–4 (as recorded in the output) |
| `2121/tcp` | Open | FTP | ProFTPD 1.3.1 |
| `3306/tcp` | Open | MySQL | 5.0.51a-3ubuntu5 |
| `5432/tcp` | Open | PostgreSQL | DB 8.3.0–8.3.7 |
| `5900/tcp` | Open | VNC | Protocol 3.3 |
| `6000/tcp` | Open | X11 | Access denied |
| `6667/tcp` | Open | IRC | UnrealIRCd (verify the exact product name against the raw output) |
| `8009/tcp` | Open | AJP13 | Apache JServ (protocol v1.3) |
| `8180/tcp` | Open | HTTP / Tomcat | Apache Tomcat/Coyote JSP engine 1.1 |

> Service and version names reflect the information reported by Nmap and may be tentative in some cases. Before publishing, compare the table with the raw Nmap output. An open port or a reported version does not, by itself, prove a vulnerability.

## 8. Findings

- **Confirmed findings:** Host `192.168.56.101` responded on the lab network, and Nmap reported the TCP ports listed above as `open`.
- **Unverified assumptions:** Some older or network-accessible services may present security risks. No service vulnerability has been tested or confirmed yet.
- **Potential security concerns:** A relatively large number of services are accessible, including legacy remote-access services and database services. Their business necessity and network exposure should be reviewed in later stages.

## 9. Limitations

- This exercise focused on host discovery and TCP port scanning.
- UDP ports were not examined.
- Authentication, access controls, service configuration, and vulnerability exploitation were not tested.
- By default, `nmap -sV` does not scan every TCP port. These results should not be presented as a definitive list of all open ports on the system.
- OS and service-version detection can be tentative.

## 10. Lessons Learned

**What did I learn?**

I learned how to use network-interface information and IP addresses to distinguish the lab interface from the NAT interface. I also used Nmap to discover responsive hosts and identify detectable services.

**What problem did I encounter?**

Initially, I had multiple IP addresses and two network interfaces, and I was unsure which address belonged to which network or machine.

**How did I resolve it?**

I reviewed the `ip a` output, ran Nmap, and consulted documentation and learning resources to compare the network information with the VM configuration.

## 11. Conclusion

This exercise established the Host-Only lab network range and selected `192.168.56.101` as the likely Metasploitable 2 target. Nmap reported multiple open TCP ports and identifiable services. These results provide an initial view of the target's network attack surface, but further investigation is required to validate vulnerabilities and assess risk.
