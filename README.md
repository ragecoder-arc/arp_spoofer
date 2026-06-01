# 🕵️ ARP Spoofer

A Python-based **Man-in-the-Middle (MITM) attack tool** built with Scapy that performs **ARP poisoning** on a target device and gateway — intercepting all network traffic between them. Includes **automatic ARP table restoration** on exit.

> ⚠️ **Disclaimer:** This tool is strictly for **educational and ethical use only** in controlled lab environments. Using this on unauthorized networks is **illegal** and can result in criminal charges. Only use on networks you own or have written permission to test.

---

## 📸 Preview

```
$ sudo python3 arp_spoofer.py

[+] Packets sent: 2
[+] Packets sent: 4
[+] Packets sent: 6
[+] Packets sent: 8
^C
[+] Detected CTRL + C ... Resetting ARP tables... Please wait.
```

---

## ✨ Features

- 🎯 **ARP Poisoning** — spoofs both target and gateway simultaneously
- 🔄 **Two-way MITM** — intercepts traffic in both directions
- 🛡️ **Auto Restore** — resets ARP tables on `CTRL+C` (no permanent damage)
- 📦 **Packet Counter** — shows live count of sent packets
- ⏱️ **Continuous Loop** — keeps spoofing every 2 seconds until stopped
- 🔕 **Silent Mode** — `verbose=False` for clean output

---

## 🛠️ Requirements

| Requirement | Detail |
|-------------|--------|
| OS | Linux (Kali recommended) |
| Python | 3.x |
| Library | `scapy` |
| Privileges | Must run as **root** (`sudo`) |

### Install Scapy

```bash
pip install scapy
```

### Enable IP Forwarding (Important!)

```bash
# Without this, target loses internet — they'll notice!
echo 1 > /proc/sys/net/ipv4/ip_forward
```

---

## 📁 Project Structure

```
arp-spoofer/
│
└── arp_spoofer.py    # Main spoofer script
```

---

## ▶️ How to Run

```bash
sudo python3 arp_spoofer.py
```

To stop and **restore ARP tables:**
```
CTRL + C
```
> ✅ Script automatically restores both target and gateway ARP tables on exit

---

## 🔧 Change Target & Gateway

Edit **lines 33–34** in the script:

```python
target_ip  = "10.252.183.155"   # ← Victim's IP
gateway_ip = "10.252.183.209"   # ← Router/Gateway IP
```

**How to find these:**
```bash
ip route show          # Find gateway IP
arp -a                 # Find devices on network
ifconfig               # Find your own IP
```

---

## 🧠 How ARP Spoofing Works

```
┌─────────────────────────────────────────────────────────────┐
│                    Normal Network                           │
│   Target ──────────────────────────────── Gateway          │
│                     (direct traffic)                        │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                  After ARP Spoofing                         │
│   Target ──────── Attacker ──────────── Gateway            │
│            ↑                      ↑                         │
│     "Gateway is me"         "Target is me"                  │
│     (told to target)        (told to gateway)               │
└─────────────────────────────────────────────────────────────┘
```

**Step by step:**
1. `get_mac()` — finds real MAC of target and gateway via ARP
2. `spoof()` — sends fake ARP replies every 2 seconds
3. Target thinks **attacker = gateway**, gateway thinks **attacker = target**
4. All traffic flows through attacker's machine *(MITM)*
5. On `CTRL+C`, `restore()` sends correct ARP info to fix tables

---

## ✅ Expected Output

**While Running:**
```
[+] Packets sent: 2
[+] Packets sent: 4
[+] Packets sent: 6
```

**On CTRL+C:**
```
[+] Detected CTRL + C ... Resetting ARP tables... Please wait.
```

---

## ⚠️ Important Notes

- 🔐 **Root required** — raw packets need root privileges
- 📶 **Enable IP Forwarding** — or target loses internet (suspicious!)
- 🔁 **ARP restore on exit** — script is safe, cleans up automatically
- 🧪 **Lab use only** — test in a VM/controlled environment
- 🐧 **Linux only** — Windows doesn't support Scapy's raw sockets well

---

## 🚀 Future Improvements

- [ ] Accept target & gateway as command-line arguments (`-t`, `-g`)
- [ ] Auto-detect gateway IP
- [ ] Add packet sniffer integration (capture HTTP traffic)
- [ ] DNS spoofing support
- [ ] Colorized terminal output with Rich library

---

## 🤝 Contributing

Pull requests are welcome! Open an issue for major changes first.

---

## 👨‍💻 Author

Built with 🐍 Python + 📡 Scapy for ethical hacking education.
