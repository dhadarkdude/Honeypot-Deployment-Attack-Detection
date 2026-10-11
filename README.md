# Cowrie SSH Honeypot — Deployment, Attack Simulation & Detection

## Overview
Deployed a Cowrie SSH honeypot in an isolated lab environment, simulated an
external attack against it using Nmap and a Metasploit SSH brute-force module, and verified detection through both the honeypot's own logs and a centralized Kibana dashboard.

##Environment: Kali Linux, Cowrie honeypot (port 2222), Nmap, Metasploit,
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
- Nmap scan results
- Metasploit SSH brute-force session output
- Cowrie log entry showing successful login and executed commands
- Kibana documents confirming the same login event and command input

## Takeaway
Honeypots are a low-cost, high-signal way to study real attacker behavior without putting production systems at risk, and they're a practical demonstration of detection engineering: capturing, logging, and centralizing evidence of an intrusion attempt.
