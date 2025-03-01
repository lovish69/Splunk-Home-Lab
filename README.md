# Splunk-Home-Lab

📌 Setting Up Splunk for Failed Login & Brute Force Detection

📖 Prerequisites

Install Splunk on your system 

Ensure your logs (Windows Event Logs, Syslogs, etc.) are ingested into Splunk

Access to Splunk Search & Reporting app

![Screenshot (757)](https://github.com/user-attachments/assets/f5660eeb-808a-47ba-9ebd-363fb3a5af0c)

⚙️ Setting Up Splunk for Failed Login Monitoring

1️⃣ Configure Log Inputs

Go to Settings > Data Inputs

Select Add Data

Choose the appropriate source (e.g., Windows Event Logs, Syslog, JSON, etc.)

![Screenshot (759)](https://github.com/user-attachments/assets/9fdefea8-c419-4cbd-893d-6a88baccdad1)

🔍 SPL Queries for Failed Logins & Brute Force Detection

1️⃣ Detecting Failed Logins

```bash
index=main source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, IpAddress
| sort -count
```
![Screenshot (762)](https://github.com/user-attachments/assets/06a3ba4e-6244-44a4-aa7c-eae24b4470b9)

✔ What it does?

Searches for Windows failed login attempts (Event Code 4625)

Counts failed logins per user and IP address

Sorts results to show the most frequent offenders


2️⃣ Brute Force Attack Detection (Multiple Failed Logins)

```bash
index=main source="WinEventLog:Security" EventCode=4625
| bucket _time span=5m
| stats count by Account_Name, IpAddress, _time
| where count > 5
```

✔ What it does?

Groups failed login attempts within 5 minutes

Flags accounts/IPs that have failed more than 5 times in that period


![Screenshot (763)](https://github.com/user-attachments/assets/b7da95d0-cfa7-4e37-a5b1-5d3e4158f084)

--Windows Event IDs for Authentication Monitoring

```plaintext
4624 – Successful login
4625 – Failed login
4648 – A login attempt was made with explicit credentials
4675 – A privileged account was used to log in
4776 – Domain controller attempted to validate credentials
4768 – Kerberos authentication ticket (TGT) requested
4769 – Service ticket requested (for lateral movement detection)
4771 – Kerberos pre-authentication failed (could indicate brute-force attacks)
```

📊 Creating a Dashboard in Splunk

1️⃣ Navigate to Dashboards in Splunk UI

2️⃣ Click Create New Dashboard

3️⃣ Add Panels with the queries above

4️⃣ Use bar charts, tables, and line graphs for visualization

5️⃣ Save and set alerts if needed



