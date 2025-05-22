# Brute-Force-alert-detection
 (Snort + pfSense + Nginx)

This project demonstrates how to detect brute-force login attempts using a custom Intrusion Detection System (IDS) setup involving **pfSense**, **Snort**, **Nginx**, and a PHP-based login form.

---

## 🧠 Objective

Simulate a brute-force attack on a login form and detect it using a **custom Snort rule** in **pfSense**. All done in a **local virtual lab**.

---

## 🛠️ Lab Environment

- **Firewall**: pfSense (3 Adapters: NAT, Internal Network, Host-only)
- **Web Server**: Ubuntu VM running Nginx with PHP login form
- **Attacker**: Same Ubuntu VM using `curl` to simulate brute-force
- **IDS**: Snort installed on pfSense
- **Network Segments**:
  - `192.168.1.1/24` (LAN - web server)
  - `10.0.2.0/24` (NAT for internet)
  - `192.168.200.1/24` (Host-only or monitoring)

---

## 🌐 Network Diagram

*(Insert screenshot or draw a simple diagram using draw.io and save it as `network-diagram.png`)*

---

## 🔐 Web Login Lab

PHP + HTML form hosted on Nginx:
- **URL**: `http://192.168.1.10/login-lab/index.php`
- Accepts `username` and `password` via `POST` request

---

## 🚨 Custom Snort Rule

```snort
alert tcp any any -> 192.168.1.10 80 (msg:"Brute-Force POST Detected"; flow:to_server,established; content:"POST"; http_method; threshold:type threshold, track by_src, count 5, seconds 10; sid:1000002; rev:1;)
