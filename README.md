# Cowrie SSH Honeypot — Deployment, Attack Simulation & Detection

## Overview
Deployed a Cowrie SSH honeypot in an isolated lab environment, simulated an
external attack against it using Nmap and a Metasploit SSH brute-force module, and verified detection through both the honeypot's own logs and a centralized Kibana dashboard.

**Environment:** Kali Linux, Cowrie honeypot (port 2222), Nmap, Metasploit,
Wireshark, ELK Stack (Elasticsearch, Logstash, Kibana) via Filebeat.

>> All activity was conducted in a self-contained lab against infrastructure I own and control. No external or production systems were involved.

## What I Did
1. Deployed Cowrie, an SSH/Telnet honeypot that emulates a vulnerable  server and logs every connection, login attempt, and command an attacker runs.
2. Ran Nmap reconnaissance against the honeypot to enumerate the exposed service before attacking it.
3. Used Metasploit's `auxiliary/scanner/ssh/ssh_login` module to bruteforce the honeypot's SSH login.
4. Captured the full attacker session in Cowrie's logs, including the credentials used and every command executed post-login (`whoami`, `help`, etc).
5. Shipped Cowrie's logs into an ELK stack via Filebeat and confirmed the same activity was independently visible in Kibana, a second, defender-side source of truth.

## Key Finding
A honeypot's value isn't just capturing the attack, it's proving the attack is visible from the defender's seat, not just the attacker's. Cross-referencing the Metasploit terminal output against Cowrie's log and the Kibana dashboard confirmed the same login and command sequence in all three places.

## Evidence
- Honeypot startup and live log tail
  <img width="753" height="401" alt="Evidence 1" src="https://github.com/user-attachments/assets/3c6d0874-8207-41b1-8d1a-87f31fec05b8" />
- Nmap scan results
  <img width="1280" height="720" alt="Evidence 2" src="https://github.com/user-attachments/assets/1d36331d-559e-4c5b-9d27-907d65babf5c" />
- Metasploit SSH brute-force session output
  <img width="1280" height="720" alt="Evidence 3" src="https://github.com/user-attachments/assets/0b123163-3405-475e-8ff7-05ea9661f799" />
- Cowrie log entry showing successful login and executed commands
  <img width="1280" height="720" alt="Evidence 4" src="https://github.com/user-attachments/assets/b67624f2-c6bc-42a3-89d7-20f840932eea" />
- Kibana documents confirming the same login event and command input
  <img width="1280" height="720" alt="Evidence 5" src="https://github.com/user-attachments/assets/c5a5bc6c-72a5-4888-8efd-d79337cb4df0" />

## Takeaway
Honeypots are a low-cost, high-signal way to study real attacker behavior without putting production systems at risk, and they're a practical demonstration of detection engineering: capturing, logging, and centralizing evidence of an intrusion attempt.
