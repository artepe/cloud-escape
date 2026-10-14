# ☁️ Cloud Escape

**Where do your smart-home devices connect? Find out without sending your data to anyone.**

Cloud Escape is a privacy-first, offline IoT traffic auditor. It reads captured network traffic and presents a clear, interactive dashboard of **observed devices, DNS queries, public IP connections, destination ports and domain/vendor hints**.

![Status](https://img.shields.io/badge/status-v0.1_MVP-00bea4) ![Python](https://img.shields.io/badge/python-3.10%2B-blue) ![License](https://img.shields.io/badge/license-MIT-purple)

## Screens / demo

No network connection or dependencies needed to explore a synthetic demo:

```bash
python3 -m cloud_escape demo -o demo.html
open demo.html   # macOS; Linux: xdg-open demo.html
```

## Analyze your own capture

```bash
# From the repository folder (requires Python 3.10+)
python3 -m cloud_escape neighbors
python3 -m cloud_escape analyze my-home.pcapng --output report.json --html report.html
open report.html

# Or host a saved JSON report on the local machine
python3 -m cloud_escape serve report.json
# Browser: http://127.0.0.1:8787
```

Optional installation: `python3 -m pip install .` then use `cloud-escape` rather than `python3 -m cloud_escape`.

### Where does the capture come from?

* Capture **at your own router** or a monitored/mirrored switch port to observe multiple IoT devices. Wireshark/tshark captures from your Mac's normal Wi-Fi interface generally **do not see all traffic between other devices**.
* Wireshark: start capture on the appropriate interface, interact with one of your IoT devices, then save as `.pcap` or `.pcapng`.
* The program does not intercept traffic, send packets, block services or contact vendor APIs. `neighbors` only reads the existing ARP table.

## What v0.1 actually supports

- ✅ Offline PCAP and PCAPNG reading (Ethernet, Linux cooked v1, raw IP)
- ✅ IPv4/IPv6 basic packet destination observations
- ✅ DNS queries / DNS A and AAAA answers; best-effort TLS ClientHello SNI (single-segment only)
- ✅ Self-contained browser dashboard, search, JSON export and synthetic demo
- ✅ No accounts, no external API, no root privileges for report viewing
- ✅ Unit tests
- ⚠️ DNS-over-HTTPS, encrypted TLS client hello, VPNs and fragmented packets may hide destination names
- ⚠️ Seeing a server connection **does not prove the device depends on the cloud**. Testing that requires a controlled offline trial and verifying device functionality.
- ⚠️ PCAP/PCAPNG with unsupported link types are skipped and reported; non-IPv4/IPv6 and extension header cases are out of scope.
- ⚠️ A single observed IP is not a device identity; a router, NAT or capture point may hide multiple devices.

## Roadmap

- [ ] Optional guided local-only/offline behavioral test with user approval and rollback
- [ ] Vendor fingerprint rules from reviewed community contributions
- [ ] DNS-over-time graphs, multicast local-service discovery, Home Assistant integration
- [ ] Safer anonymized report export (redaction of private identifiers)
- [ ] HTTPS metadata, IPv6 extensions and broader capture link type support

## Privacy / security

Packet captures can contain passwords, personal identifiers, device addresses and private conversations. **Never upload unredacted captures to GitHub issues.** Run only against equipment and networks you own or are authorized to examine. Report files remain on your machine. The read-only analysis makes no claims of automatic cloud removal.

## Run tests

```bash
python3 -m unittest discover -s tests -v
```

MIT licensed. Community contributions welcome. v0.1 is an honest, working MVP, not a universal cloud-bypass exploit.

### One-click macOS demo

After extracting the ZIP, double-click `START_DEMO_MAC.command` (or right-click → Open if macOS asks for confirmation). It runs an offline demo and opens the HTML file in your browser. No admin rights required.