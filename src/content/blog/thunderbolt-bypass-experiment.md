---
title: 'Experimental Report: Bypassing Anti-Cheat with Thunderbolt Tunnels'
description: 'Investigating the efficacy of encapsulating DMA attack hardware within legitimate Thunderbolt 3/4 enclosures to evade PCIe topology scanning.'
pubDate: '2025-12-28'
heroImage: '../../assets/cover-thunderbolt.svg'
---

As internal PCIe slots become heavily scrutinized by kernel-level anti-cheats, researchers are looking externally. **Thunderbolt (TB)** technology, which essentially extends the PCIe bus over a cable, offers a unique opportunity for hardware obfuscation.

This report details our experiments using off-the-shelf Thunderbolt 3 enclosures to hide standard PCILeech hardware from system surveillance.

## The Theory: Topological Obfuscation

Traditional anti-cheat heuristics scan the **PCIe Configuration Space** of all devices connected to the CPU's Root Ports. They look for specific vendor IDs (Xilinx, Altera) or abnormal BAR (Base Address Register) sizes typical of FPGA development boards.

**The Thunderbolt Twist:**
When a device is connected via Thunderbolt, it doesn't appear directly on the root PCIe bus in the same way. It sits behind the **Intel/Apple Thunderbolt Controller**.
*   **Heuristic Blind Spot:** Some older anti-cheat modules were configured to ignore devices strictly within the Thunderbolt chain, assuming they were legitimate docks or eGPUs.
*   **Driver Masking:** The OS loads the Thunderbolt infrastructure drivers first, creating a "safe container" for the downstream device.

## The Experiment Setup

*   **Attack Hardware:** Squire DMA Board (Artix-7 FPGA).
*   **Enclosure:** Razer Core X (Thunderbolt 3 eGPU enclosure) & Generic NVMe TB3 enclosure.
*   **Host System:** Windows 11 (Secure Boot Enabled, VBS Enabled).

We flashed the FPGA with a standard "LeechCore" firmware and placed it inside the enclosure, replacing the GPU/SSD.

## Findings

### 1. Detection via Topology
Contrary to forum rumors, modern anti-cheats (Vanguard, EAC) **CAN** see through the Thunderbolt tunnel. The OS still enumerates the device at the end of the chain.
*   **Result:** If the FPGA firmware identifies itself as a generic "Xilinx Device," it is flagged immediately, regardless of the Thunderbolt wrapper.
*   **Bypass:** We had to apply the same sophisticated **Configuration Space Spoofing (DSN/ConfigBARs)** used on internal cards. The enclosure alone provides zero identity protection.

### 2. The Hot-Plug Vector
This is where Thunderbolt shines.
*   **Method:** We launched the target game *without* the device connected. Once the game (and anti-cheat) fully initialized, we hot-plugged the Thunderbolt cable.
*   **Observation:** The anti-cheat's initial scan was clean. The "Runtime Scan" frequency varies by provider. We successfully read memory for 45 minutes before a "Hardware Change Detected" flag triggered a kick (not a ban).

### 3. DMA Remapping (The Hard Counter)
Thunderbolt security settings in BIOS (Kernel DMA Protection) are the biggest enemy.
*   **Windows 10/11 Kernel DMA Protection:** If enabled, the OS strictly limits what memory regions a hot-plugged TB device can touch.
*   **Workaround:** We had to disable "Kernel DMA Protection" and "BitLocker" in the BIOS to gain full read/write access. This is a suspicious configuration that high-tier anti-cheats now check for.

## Conclusion

Thunderbolt enclosures are not a "magic bullet." They do not hide the *nature* of the device from the OS. However, they introduce a significant layer of physical flexibility (hot-plugging) that can be exploited to bypass initialization checks.

For a robust undetected setup, the Thunderbolt strategy must be combined with high-quality firmware spoofing. A raw FPGA in a fancy box is still just a raw FPGA.
