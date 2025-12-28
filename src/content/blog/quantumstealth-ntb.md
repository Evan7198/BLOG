---
title: 'EProject: Leveraging NTB for Absolute Host Isolation'
description: 'Technical report on using PCIe Non-Transparent Bridging (NTB) to create undetectable, physically isolated attack environments.'
pubDate: '2025-12-28'
heroImage: '../../assets/cover-bridge.svg'
---

In the escalating arms race between security researchers and anti-cheat developers, the hardware itself is evolving. While traditional DMA cards act as simple endpoints, a new paradigm is emerging from the server room: **Non-Transparent Bridging (NTB)**.

This report analyzes the "EProject" approach, which weaponizes NTB technology to achieve what we call **Absolute Host Isolation**.

## What is NTB?

In standard PCIe architecture, a system has one Root Complex (the CPU/Chipset) that controls all endpoints. You cannot simply connect two computers with a PCIe cable because both try to be the "master."

**Non-Transparent Bridge (NTB)** is a technology originally designed for high-availability storage arrays. It allows two independent computers (two Root Complexes) to connect via PCIe. It creates a "window" in the memory address space of System A that translates directly to the memory of System B.

## The EProject Architecture

The EProject implementation utilizes this bridge to fundamentally change how external access works.

### 1. True Dual-Host Topology
Unlike a standard DMA card that is a "device" plugged into the "host," NTB links two peers.
*   **Host A (Game PC):** Sees a generic PCI Bridge device. It does not see a "Leech" card or a generic FPGA endpoint. It sees a standard system component often used in high-end workstations.
*   **Host B (Attack PC):** Maps the physical memory of Host A into its own virtual address space via the NTB Bar.

### 2. The Isolation Advantage
The primary vector for detecting DMA cards is analyzing the PCIe Configuration Space (Device ID, Vendor ID, Config Header) and measuring TLP (Transaction Layer Packet) latency.

With NTB:
*   **Signature Obfuscation:** The bridge chip itself (e.g., from PLX or Avago) presents a legitimate, signed hardware ID commonly found in servers. It doesn't need to "spoof" a Network Card; it *is* a legitimate Bridge.
*   **Software Gap:** There is absolutely zero unauthorized code running on Host A. The reading logic, the parsing, and the decision-making happen on Host B, physically separated by the PCIe link.

## Breaking the Detection Logic

Current anti-cheats look for:
1.  **Suspicious PCIe Devices:** NTB bridges are whitelisted in many enterprise/workstation environments.
2.  **Abnormal Memory Access:** Because NTB is designed for high-throughput memory mirroring, its access patterns can be tuned to mimic legitimate system interconnect traffic (like CXL or NVLink).

## Implementation Challenges

While powerful, EProject/NTB is not without hurdles:
*   **Hardware Cost:** Requires specialized motherboards or riser cards that support NTB bifurcation.
*   **Driver Complexity:** Setting up the memory windows (BAR setup) requires precise BIOS configuration, often demanding modified UEFI firmware.

## Conclusion

EProject represents the industrialization of game hacking. By repurposing enterprise-grade connectivity protocols, researchers have created a method of access that is structurally invisible to the host operating system. It moves the battleground from "detecting the device" to "analyzing memory access heuristics" exclusively.
