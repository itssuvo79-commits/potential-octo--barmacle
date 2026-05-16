# Professional Python Port Scanner Project

## Project Structure

```text
port-scanner/
│
├── scanner.py
├── requirements.txt
├── README.md
└── screenshots/
```

---

# scanner.py

```python
import socket
import threading
from queue import Queue
from datetime import datetime

# =============================
# Configuration
# =============================
THREADS = 100
queue = Queue()
open_ports = []

print("=" * 60)
print("        PROFESSIONAL PYTHON PORT SCANNER")
print("=" * 60)

# =============================
# User Input
# =============================
target = input("Enter Target IP or Domain: ")

try:
    target_ip = socket.gethostbyname(target)
except socket.gaierror:
    print("[!] Invalid Hostname")
    exit()

start_port = int(input("Enter Start Port: "))
end_port = int(input("Enter End Port: "))

print(f"\n[+] Scanning Target: {target}")
print(f"[+] IP Address: {target_ip}")
print(f"[+] Port Range: {start_port}-{end_port}")
print(f"[+] Scan Started: {datetime.now()}\n")

# =============================
# Port Scan Function
# =============================
def scan_port(port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)

        result = sock.connect_ex((target_ip, port))

        if result == 0:
            try:
                banner = sock.recv(1024).decode().strip()
            except:
                banner = "No Banner"

            print(f"[OPEN] Port {port} | {banner}")
            open_ports.append(port)

        sock.close()

    except Exception:
        pass

# =============================
# Thread Worker
# =============================
def worker():
    while not queue.empty():
        port = queue.get()
        scan_port(port)
        queue.task_done()

# =============================
# Add Ports to Queue
# =============================
for port in range(start_port, end_port + 1):
    queue.put(port)

# =============================
# Start Threads
# =============================
for _ in range(THREADS):
    thread = threading.Thread(target=worker)
    thread.daemon = True
    thread.start()

queue.join()

# =============================
# Save Results
# =============================
with open("scan_results.txt", "w") as file:
    file.write(f"Target: {target}\n")
    file.write(f"IP: {target_ip}\n")
    file.write(f"Open Ports: {open_ports}\n")

print("\n" + "=" * 60)
print("Scan Completed")
print(f"Open Ports Found: {len(open_ports)}")
print("Results saved in scan_results.txt")
print("=" * 60)
```

---

# requirements.txt

```text
No external libraries required
Python 3.x only
```

---

# README.md

```markdown
# Python Port Scanner

A professional multithreaded TCP Port Scanner built using Python.

## Features

- TCP Port Scanning
- Multithreading for faster scanning
- Banner Grabbing
- Timeout Handling
- Save Scan Results
- Linux and Windows Compatible
- Beginner Friendly Cybersecurity Project

---

## Technologies Used

- Python
- Socket Programming
- Threading
- Networking Concepts

---

## Installation

Clone the repository:

```bash
git clone https://github.com/yourusername/port-scanner.git
cd port-scanner
```

Run the scanner:

```bash
python scanner.py
```

---

## Example

```bash
Enter Target IP or Domain: scanme.nmap.org
Enter Start Port: 1
Enter End Port: 1000
```

---

## Educational Purpose

This project was created for learning cybersecurity, networking, and socket programming concepts.

Use only on systems you own or have permission to test.

---

## Skills Demonstrated

- TCP/IP Networking
- Cybersecurity Fundamentals
- Python Programming
- Multithreading
- Reconnaissance Techniques
- Linux Environment Usage

---

## Author

Trishanjit Biswas
```

---

# How to Run

## Step 1 — Install Python

Download Python:
https://www.python.org/downloads/

While installing, enable:

```text
Add Python to PATH
```

---

## Step 2 — Open Terminal

Windows:

```bash
cmd
```

Linux:

```bash
terminal
```

---

## Step 3 — Run Project

```bash
python scanner.py
```

---

# GitHub Upload Steps

## Create Repository

Go to:

https://github.com/new

Repository Name:

```text
port-scanner
```

---

## Upload Using Git

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/port-scanner.git
git push -u origin main
```

---

# Resume Description

Developed a multithreaded TCP Port Scanner using Python sockets to identify open ports and network services on target systems. Implemented timeout handling, banner grabbing, and concurrent scanning for improved performance. Tested in a Kali Linux virtual lab environment to strengthen networking and cybersecurity fundamentals.

---

# Future Improvements

You can later add:

- GUI Interface
- UDP Scanning
- Service Detection
- OS Detection
- Export PDF Reports
- Vulnerability Detection
- Nmap Integration

