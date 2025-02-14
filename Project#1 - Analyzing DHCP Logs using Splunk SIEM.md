# **Analyzing DHCP Logs Using Splunk SIEM**

## 🚀 Introduction

**Dynamic Host Configuration Protocol (DHCP)** is responsible for dynamically assigning IP addresses to devices in a network. Monitoring **DHCP logs** is crucial for tracking network activity, troubleshooting connectivity issues, and detecting security threats such as unauthorized access and abnormal IP assignment patterns.

This project demonstrates how to use **Splunk SIEM** to analyze **DHCP log files**, extract relevant data, detect anomalies, and monitor network activity effectively.

---

## 📌 **Project Overview**
The goal of this project is to:

✅ Search and extract DHCP logs from **Splunk SIEM**  
✅ Extract key fields such as **src_ip, dst_ip, lease renewals**  
✅ Detect anomalies such as **frequent lease renewals or unauthorized IP allocations**  
✅ Monitor IP usage trends using **Splunk queries**  
✅ Identify potential **security risks** in DHCP assignments  

By following this guide, you will learn **how to ingest logs, extract relevant information, analyze trends, and detect suspicious activity** using Splunk.

---

## ⚙ **Prerequisites**
Before getting started, make sure you have:
- ✅ **Splunk Enterprise or Splunk Cloud** installed  
- ✅ A **Sample DHCP Log File** → [Download Sample DHCP File](https://github.com/0xAll3nC/Splunk-Projects/blob/main/Sample%20Files/dhcp.log.gz)  
- ✅ Basic knowledge of **Splunk's search processing language (SPL)**  
- ✅ Access to **Splunk's Search & Reporting app**  

---

## 📂 **Steps to Upload Sample DHCP Log File to Splunk SIEM**

Follow these steps to **ingest** your DHCP log file into Splunk:

### **1️⃣ Navigate to Splunk's Data Input Section**
- Open Splunk and go to **Settings → Add Data → Upload Files**  
- Select **your DHCP log file** (`dhcp.log.gz` or similar)  

### **2️⃣ Set Source Type and Index**
- Choose **Manual Input** and set **sourcetype=dhcplogs**  
- Select or create a new **index** (e.g., `dhcp_index`)  

### **3️⃣ Verify and Start Indexing**
- Click **Next** to preview the data  
- If everything looks correct, click **Start Indexing**  

Once uploaded, the logs are now **searchable in Splunk** using the `index` and `sourcetype`.

---

## 📊 **Steps to Analyze DHCP Logs in Splunk**

### **1️⃣ Search for DHCP Events**
To fetch DHCP logs from Splunk, use:

```spl
index=* sourcetype=dhcplogs
```

---

### **2️⃣ Extract Relevant Information**
To analyze logs effectively, extract key fields such as **src_ip (source IP) and dst_ip (destination IP)**.

- Used **Splunk's Interactive Field Extractor (IFX)** to extract `src_ip` and `dst_ip` without regex.
- Opened a sample log entry and clicked **"Extract Fields"**.
- Highlighted **IP addresses**, and Splunk automatically recognized patterns.
- Saved the extracted fields to be used in searches.

Example:

```spl
index=* sourcetype=dhcplogs | stats count by src_ip, dst_ip
```

---

### **3️⃣ Analyze Traffic Patterns - Check IP Address Assignments**
This step helps **understand IP allocations** and identify potential misconfigurations.

```spl
index=* sourcetype=dhcplogs | stats count by src_ip
```

```spl
index=* sourcetype=dhcplogs | stats count by src_ip, dst_ip
```

```spl
index=_* OR index=* sourcetype=dhcplogs | top limit=10 src_ip
```

💡 **Tip from Experts:**  
If an IP is assigned **too frequently**, it may indicate **IP conflicts** or **a device reconnecting repeatedly**.

---

### **4️⃣ Detect Anomalies in DHCP Logs**
Identifying **unusual patterns** such as unauthorized devices or unexpected spikes.

```spl
index=* sourcetype=dhcplogs | timechart span=1h count
```

To find unauthorized devices:

```spl
index=_* OR index=* sourcetype=dhcplogs | search NOT client_identifier="c8:60:00:1a:aa:77"
```

💡 **Tip from Experts:**  
If **unknown MAC addresses** appear, check them against your **authorized device list**.

---

### **5️⃣ Monitor IP Address Usage Over Time**
Tracking **how often an IP appears** in the logs to detect **network issues**.

```spl
index=* sourcetype=dhcplogs src_ip="192.168.202.76" | timechart span=2h count
```

To check **most frequently assigned IPs**:

```spl
index=* sourcetype=dhcplogs | top limit=10 src_ip
```

To detect frequent **IP lease renewals**:

```spl
index=* sourcetype=dhcplogs | stats count by src_ip, lease_renewal | where count > 1 AND lease_renewal="true"
```

💡 **Tip from Experts:**  
Set **Splunk alerts** for **excessive lease renewals**, which could indicate **misconfigured devices**.

---

### **6️⃣ Detect Traffic Deviations in DHCP Logs**
Looking for **sudden increases in DHCP requests** that may indicate attacks.

```spl
index=* sourcetype=dhcplogs | timechart span=1d count by src_ip
```

💡 **Tip from Experts:**  
Compare today’s logs with **previous weeks**. If DHCP activity suddenly **spikes**, investigate further.

---

### **7️⃣ Dashboard for DHCP Log Analysis**  
I created a **Splunk dashboard** to visually analyze DHCP logs using the following search query:  

```spl
index=* sourcetype=dhcplogs src_ip="192.168.202.76"
```

The dashboard helps in:  
✅ **Monitoring DHCP activity** for a specific `src_ip`  
✅ **Visualizing IP allocations and trends**  
✅ **Quickly detecting anomalies and spikes in DHCP logs**  

---

## 📊 **Results & Insights**
- ✅ **Extracted src_ip and dst_ip** using Splunk's **IFX tool**.
- ✅ Successfully identified **IP assignment trends**.
- ✅ Detected **unauthorized devices** trying to obtain IPs.
- ✅ Monitored **IP lease renewals** and identified **potential misconfigurations**.
- ✅ Found **sudden spikes in DHCP requests**, indicating **possible security threats**.

---

## 🛠 **Next Steps**
✔ Automate anomaly detection using **Splunk Alerts**  
✔ Build a **dashboard** for real-time DHCP activity monitoring  
✔ Expand analysis by integrating **firewall and intrusion detection logs**  

---

## 🎯 **Conclusion**
This project provided hands-on experience using **Splunk SIEM** for **log analysis and anomaly detection**. By leveraging **search queries, field extraction, and statistical analysis**, we successfully monitored **DHCP activity** and detected **potential security threats**.

---

## 📌 **References**
- 📖 [Splunk Documentation](https://docs.splunk.com/)
- 🔍 [Splunkbase Apps](https://splunkbase.splunk.com/)
- 🌐 [DHCP Protocol Overview](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)
