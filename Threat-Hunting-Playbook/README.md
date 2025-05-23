# Threat Hunting - Login Attempt Investigation using Splunk

## Overview
In this exercise, I performed a threat hunting activity on Apache web server logs using Splunk. The goal was to detect suspicious login attempts that may indicate brute-force attacks or credential stuffing.

---

## Objective
Identify abnormal login behavior by extracting IP addresses hitting the `/login` endpoint and visualizing their frequency and timing.

---

## Tools Used
- **Splunk Enterprise**
- **Regex for field extraction**
- **Timechart, Column Chart Visualization**

---

## Visual Evidence

### 1. IP Address Frequency Count
Shows the number of times each IP attempted to access the `/login` page.

![IP Frequency Count](./ASP1.png)

---

### 2. Timechart of Login Attempts
Displays when each login attempt occurred, grouped by IP.

![Timechart of Attempts](./ASP2.png)

---

### 3. Column Chart Visualization
Graphical display to identify spikes and attack patterns quickly.

![Column Chart](./ASP3.png)

---

## Interpretation

- IP `95.55.51.230` made **9 login attempts** — potential brute-force behavior.
- The timechart reveals repeated attempts from the same IPs in short intervals.
- Visualizations provide strong indicators of **suspicious login activity**.

---

## Outcome

- **Findings escalated** to the incident response team.
- **Recommendation:** Block high-frequency IPs, isolate affected hosts, and perform memory analysis.

---

## Skills Demonstrated

- ✅ Splunk Query Building
- ✅ Threat Detection via Pattern Recognition
- ✅ Data Visualization & Log Interpretation

## SPL Commands Used

```spl
index=* source="apache.access.log" "/login"
| rex field=_raw "(?<client_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
| stats count by client_ip
| sort - count

index=* source="apache.access.log" "/login"
| rex field=_raw "(?<client_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
| timechart count by client_ip

