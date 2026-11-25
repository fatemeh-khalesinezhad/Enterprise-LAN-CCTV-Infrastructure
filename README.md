<div align="center">

# Modern Corporate Enterprise LAN & CCTV Infrastructure

**A standards-compliant engineering design integrating a 40-PC Wired/Wireless Hybrid LAN with an 8-camera PoE IP surveillance system — engineered per TVTO Grade 12 curriculum and TIA/IEEE/ISO industrial standards.**

[![TVTO](https://img.shields.io/badge/Standard-TVTO%20Grade%2012-blue?style=flat-square)](https://www.tvtc.ir/)
[![Modules](https://img.shields.io/badge/Modules-3%20%2B%205-orange?style=flat-square)]()
[![Cat6](https://img.shields.io/badge/Cat6-%E2%89%A490m-green?style=flat-square)]()
[![PoE](https://img.shields.io/badge/PoE-IEEE%20802.3at-yellow?style=flat-square)]()
[![Codec](https://img.shields.io/badge/H.265-2MP%20%7C%2015fps-purple?style=flat-square)]()
[![RouterOS](https://img.shields.io/badge/MikroTik-RouterOS-red?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)](./LICENSE)
[![Status](https://img.shields.io/badge/Status-Design%20Complete-brightgreen?style=flat-square)]()

</div>

## Table of Contents
- [1. Executive Summary](#1-executive-summary)
- [2. Project Objectives](#2-project-objectives)
- [3. System Architecture](#3-system-architecture)
- [4. Technical Specifications](#4-technical-specifications)
- [5. Software & Toolchain](#5-software--toolchain)
- [6. Standards & Compliance](#6-standards--compliance)
- [7. Engineering Calculations](#7-engineering-calculations)
- [8. CCTV Design & Deployment Workflow](#8-cctv-design--deployment-workflow)
- [9. Routing & Inter-VLAN Configuration](#9-routing--inter-vlan-configuration)
- [10. Network Security Design](#10-network-security-design)
- [11. Repository Structure](#11-repository-structure)
- [12. Bill of Materials (LOM)](#12-bill-of-materials-lom)
- [13. Validation & Testing Plan](#13-validation--testing-plan)
- [14. Author & Acknowledgments](#14-author--acknowledgments)

---

## 1. Executive Summary
This repository contains the **complete engineering design of a Modern Corporate Enterprise's integrated IT infrastructure** — a structured-cabling Local Area Network (LAN) serving 40 wired workstations across an open workspace and two executive offices, plus a **Smart Meeting Room** equipped with wireless laptops. 

The core premise is **physical resource sharing** between LAN and CCTV: a single 48-port PoE+ switch, a single structured-cabling backbone, and a single secure server rack. This reduces capital expenditure and centralizes power, management, and physical security. 

This design integrates the theoretical foundations of **Module 3** (Routing/Network Setup) and **Module 5** (Network Hardware/CCTV) from the TVTO textbook *E-Commerce and Network Security*.

| Metric | Value |
|---|---|
| Longest Cat6 permanent link | **≈ 35 m** (≪ 90 m TVTO limit) |
| Storage required (30-day H.265) | **5.96 TB** (1x 6TB Surveillance HDD) |
| Design bandwidth (Main + Sub) | **40 Mbps** |
| PoE load | **56 W** of 240 W budget (23%) |
| Total Nodes | 40 Wired PCs + 2 APs + 8 IP Cameras |

---

## 2. Project Objectives
| # | Objective | Success Metric |
|---|---|---|
| **O1** | Design a Hybrid Star-Topology LAN | All wired PCs on VLAN 10; Laptops connected via APs |
| **O2** | Enforce 90 m Cat6 permanent-link rule | Every drop ≤ 90 m — verified in Visio |
| **O3** | Design 8-camera IP CCTV with zero blind spots | 100% coverage of hallway, entrances, and server room |
| **O4** | Calculate NVR storage for 30-day retention | Disk Calculator output matches manual within 5% |
| **O5** | Calculate network bandwidth | Design load ≤ 30% of switch uplink capacity |
| **O6** | Configure inter-VLAN routing (Module 3) | Data and CCTV VLANs strictly isolated |

---

## 3. System Architecture

### 3.1 Physical Layout & Dimensions
All architectural blueprints were drafted in MS Visio at a strict **1:100 scale** (1cm on paper = 1m in reality), based on real-world baseline measurements.
*   **Main Hallway:** 1500 cm × 310 cm (Ceiling: 290 cm)
*   **Open Workspace:** 850 cm × 700 cm (Houses 25 wired PCs + 1 AP)
*   **Server Room:** 400 cm × 350 cm (Secure 90cm door, 42U Rack)
*   **Executive Offices (x2):** 600 cm × 550 cm each (Houses 5 wired PCs each; 100cm glass doors)
*   **Smart Meeting Room (Office 3):** 600 cm × 550 cm (Boardroom table with 6 laptops connecting wirelessly via a local AP).

### 3.2 Logical Segmentation (VLANs)
| VLAN | Name | Subnet | Gateway | Purpose |
|---|---|---|---|---|
| 10 | CORP_DATA | 192.168.10.0/24 | 192.168.10.1 | All 40 Workstations & Wi-Fi Laptops |
| 20 | CCTV_MGMT | 192.168.20.0/24 | 192.168.20.1 | NVR & IP Cameras |

> **Why /24?** Provides 254 usable IPs for the data VLAN, allowing for seamless future expansion (e.g., wireless APs, printers) without re-subnetting.

---

## 4. Technical Specifications

### 4.1 Network Hardware
| Component | Specification | Rationale |
|---|---|---|
| **Router** | MikroTik RouterOS (RB4011iGS+RM class) | Inter-VLAN routing, firewall, DHCP |
| **Core Switch** | 48-port Gigabit PoE+ (802.3at), ≥ 380W budget | 40 Wired PCs + 8 Cameras + 2 APs |
| **Wireless AP** | 802.11ac/ax Dual-Band PoE | Provides seamless Wi-Fi for laptops in Workspace & Meeting Room |
| **NVR** | 8-channel H.265+ | Capacity for 8 cameras |

### 4.2 CCTV Hardware
| Type | Qty | Resolution | Lens | Mount | Placement |
|---|---|---|---|---|---|
| **Dome** | 4 | 2 MP 1920×1080 | 2.8 mm | Ceiling (290 cm) | Hallway (x3), Server Room (x1) |
| **Bullet** | 4 | 2 MP 1920×1080 | 4 mm | 250 cm | Workspace & Office Entrances |

---

## 5. Engineering Calculations

### 5.1 Cable Length Verification (TVTO p. 33)
**Rule:** Cat6 permanent link must not exceed 90 m.
*   **Farthest drop:** Server Room to the furthest PC in the bottom-left office.
*   **Calculation:** Horizontal distance ≈ 23m. Vertical drop ≈ 5.8m. Slack = 20%. Total = **~35 meters**.
*   **Result:** ✅ **35 m ≪ 90 m — PASS.** No mid-switch is required.

### 5.2 Storage Calculation (TVTO p. 199)
**Inputs:** 8 cameras × 2 MP × H.265 × 15 fps × 30 days.
```text
Step 1 — Main-stream bitrate: 2.0 Mbps per camera
Step 2 — Total ingest: 8 cameras × 2.0 Mbps = 16 Mbps
Step 3 — Daily data volume: 16 Mbps × 86,400 s = 1,382,400 Mb/day
          1,382,400 ÷ 8 = 172,800 MB/day (≈ 172.8 GB/day)
Step 4 — 30-day raw storage: 172,800 MB × 30 = 5,184,000 MB (≈ 5.18 TB)
Step 5 — Add 15% engineering headroom: 5.18 TB × 1.15 = 5.957 TB
Step 6 — HDD selection: 1 × 6 TB Surveillance-grade HDD (WD Purple / SkyHawk)

Result: ✅ 5.96 TB required → 1 × 6 TB HDD selected.

5.3 Bandwidth Calculation (TVTO p. 198)
text
Main Stream:  8 cameras × 4.0 Mbps = 32.0 Mbps (recording)
Sub Stream:   8 cameras × 1.0 Mbps =  8.0 Mbps (remote viewing)
Subtotal:                            40.0 Mbps
+ 20% engineering headroom:          48.0 Mbps
Result: **48 Mbps design load — well within the 1 Gbps switch capacity.**

6. CCTV Design & Deployment Workflow
6.1 Camera Placement (8 Cameras)
Hallway West: Dome 2.8mm (Corridor overview)

Hallway Center: Dome 2.8mm (Overlap coverage)

Hallway East: Dome 2.8mm (Corridor overview)

Server Room: Dome 2.8mm (Rack-access monitoring)

Workspace Double Door: Bullet 4mm (Face capture)

Office 1 Door: Bullet 4mm (Face capture)

Office 2 Door: Bullet 4mm (Face capture)

Office 3 (Meeting Room) Door: Bullet 4mm (Face capture)

6.2 Deployment Workflow
SADP: Discover cameras, assign static IPs (192.168.20.11-18), activate, and disable UPnP.

iVMS-4200: Add cameras to NVR. Configure Main Stream (2MP, H.265, 15fps) for 24/7 recording, and Sub Stream (D1, 10fps) for live view.

IP Video System Design Tool: (TVTO p. 197) Generate FOV (Field of View) screenshots. Dome set to 2.8mm wide-angle, Bullets set to 4mm for depth.

Hik-Connect: Bind NVR for remote Sub-Stream viewing, enabling 2FA.

7. Bill of Materials (LOM)
(Real-world pricing based on Iranian market, updated for 1405–1406 / 2026–2027)

Category	Item	Qty	Unit Cost (Toman)	Total (Toman)
Passive	Cat6 UTP Cable (305m Box)	2	29,861,110	59,722,220
Passive	Trunking (PVC, 100x50mm, 2m)	15	1,570,000	23,550,000
Passive	Duct (PVC Flex, 40mm, 25m)	4	3,500,000	14,000,000
Passive	RJ45 Cat6 Connectors & Keystones	40	200,000	8,000,000
Passive	48-Port Patch Panel (Cat6)	1	3,820,000	3,820,000
Passive	42U Server Rack	1	26,366,000	26,366,000
Active	MikroTik RouterOS (RB4011iGS+RM)	1	63,000,000	63,000,000
Active	48-Port Gigabit PoE+ Switch	1	352,800,000	352,800,000
Active	Wireless Access Point (802.11ax)	2	33,999,000	67,998,000
Active	8-Ch NVR (H.265)	1	21,714,000	21,714,000
Active	6TB Surveillance HDD	1	58,630,000	58,630,000
Active	2MP Dome Cameras	4	3,390,000	13,560,000
Active	2MP Bullet Cameras	4	8,999,000	35,996,000
Labor	Cabling, Rack Assembly, CCTV Config	1	25,000,000	25,000,000
—	TOTAL			774,156,220

8. Validation & Testing Plan
☑ Visio 1:1 scale verified with exact dimensions.

☑ All Cat6 drops documented ≤ 90 m (Max: 35m).

☑ 8 FOV screenshots generated in IP Video Tool.

☑ Disk Calculator output matches manual within 5%.

☑ Bandwidth calculation cross-checked (Main + Sub).

☑ PoE budget verified (56W / 240W).

□ Physical deployment (Pending).

9. Author & Acknowledgments
Author: [Your Full Name]
Grade: 12 — Network & Software
Standard: TVTO (Iranian Technical and Vocational Training Organization)
Year: 1405–1406 (2026–2027)

This project was designed as a high-end Capstone Portfolio piece for GKS / CSC / MEXT university applications, bridging conceptual enterprise visualization with rigorous engineering calculations.