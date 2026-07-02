---
title: AI Server hardware
---

# AI Specs based on Google AI
### Why the AMD Ryzen 9000-series X3D Benefits the R9700 More

#### 1. Maximizing Dual-GPU and Quad-GPU Topologies
The Radeon AI Pro R9700 features a blower-style dual-slot design explicitly optimized to be stacked in arrays of 2 to 4 cards. 
* **AMD X870E Workstation Platforms:** Pairing an X3D processor like the Ryzen 9 9950X3D or 9900X3D with a premium X870E motherboard provides native, clean routing for multi-GPU PCIe Gen 5.0 x16 lanes.
* **Intel Platform Limitation:** The Intel 270K Plus sits on the LGA1851 socket (Arrow Lake architecture), which is fundamentally a mainstream desktop platform. Splitting PCIe lanes across multiple high-bandwidth workstation cards on these motherboards often bottlenecks the secondary card down to Gen 4 or x4 speeds.

#### 2. AMD Smart Access Memory (SAM)
Using an all-AMD ecosystem unlocks Smart Access Memory (SAM). This allows a Ryzen 9000 chip to bypass standard Windows memory mapping bottlenecks and directly leverage the full 32 GB of high-speed GDDR6 VRAM on the R9700 simultaneously. This drastically speeds up large token context indexing when offloading data from the CPU to the GPU.

#### 3. Massive L3 Cache for Data Tokenization
Before the R9700 can process data or text-model inferences, the CPU must tokenize and batch the dataset. The stacked L3 V-Cache on chips like the Ryzen 7 9800X3D (104MB Cache) or 9950X3D (144MB Cache) holds massive dataset matrices directly on the processor. This reduces the latency of constantly querying system RAM, ensuring the GPU is never left waiting for data pipelines.

---

### When to Consider the Intel 270K Plus

The only scenario where the Intel Ultra 7 270K Plus makes sense is pure multi-threaded cost efficiency. 
* **The Core Advantage:** The 270K Plus packs 24 physical cores (8 P-Cores + 16 E-Cores). 
* **The Workload:** If your daily workflow involves heavy, non-gaming CPU tasks alongside your AI tasks—such as background 4K video encoding, massive ZIP compilations, or multi-threaded CPU rendering—the 270K Plus offers exceptional raw processing horsepower relative to its budget-oriented price tag.


# Donato Capitella's (modified) Build

## The Build Components

| Component | Name | Price |
| --- | --- | --- |
| GPUs | AMD Radeon AI Pro R9700 | 29.000.000 | 
| GPUs | AMD Radeon AI Pro R9700 | 29.000.000 | 
| CPU | AMD Ryzen 7 9800X3D | 8.730.000 |
| CPU Cooler | Arctic Liquid Freezer III 360 | 1.600.000 |
| Mobo | MSI MAG X870E TOMAHAWK WIFI | 6.200.000 |
| RAM | 64GB Crucial Pro DDR5 | 18.000.000 |
| NVME | Crucial T705 2TB | 6.160.000 |
| PSU | Seasonic VERTEX PX ATX 3.1 PX-1200 1200W | 5.100.000 |
| Case | MSI PC CASE ATX MAG FORGE 130A AIRFLOW | 700.000 |
| --- | --- | --- |
| Total | --- | 105.000.000  |

## The Original

GPUs: 2x AMD Radeon AI PRO R9700 (64GB Total VRAM)  
CPU: AMD Ryzen 9 9900X3D  
Motherboard: ASRock X870E Taichi  
RAM: 64GB Crucial Pro DDR5  
Storage: Crucial T710 2TB NVMe PCIe 5.0  
Power Supply: Corsair HX1200i (1200W)  
Case: Fractal Design Torrent (High Airflow)
