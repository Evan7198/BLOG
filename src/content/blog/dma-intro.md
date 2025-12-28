---
title: 'Introduction to DMA Attacks'
description: 'Understanding Direct Memory Access vulnerabilities and how hardware cheats operate.'
pubDate: 'Jul 22 2024'
heroImage: '../../assets/cover-shield.svg'
---

## What is DMA?

**Direct Memory Access (DMA)** is a feature of computer systems that allows certain hardware subsystems to access main system memory (RAM) independently of the central processing unit (CPU).

While this is crucial for performance (e.g., graphics cards, network cards), it introduces significant security risks.

### The Attack Vector

In the context of game security and "cheating," a DMA attack involves using a separate piece of hardware (like a PCIe screamer card) to read and write to the game's memory without the CPU—and often the operating system's anti-cheat software—being aware of it.

```c
// Pseudo-code example of reading memory
void read_memory(uint64_t address, void* buffer, size_t size) {
    // Hardware-level access bypasses OS protection
    dma_device->read(address, buffer, size);
}
```

### Why Anti-Cheat Struggles

Traditional anti-cheat runs on the CPU (Ring 0 or Ring 3). Since DMA hardware operates independently on the PCIe bus, it doesn't need to run code on the host CPU to access memory. This makes detection extremely difficult without:

1.  **IOMMU (Input-Output Memory Management Unit)**: Can restrict device access to memory.
2.  **Heuristics**: Detecting impossible player behavior.

## Conclusion

DMA represents the frontier of hardware-based exploitation. As software protections become more robust, attackers move down the stack to the hardware level.
