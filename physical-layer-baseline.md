# Physical Infrastructure Baseline

## Overview

This document describes the physical layer preparation performed before deploying VMware ESXi. The objective was to ensure optimal hardware performance, proper resource alignment, and stable platform behavior through validated hardware configuration and BIOS tuning.

---

## Objective

Prepare hosts with a consistent and performance optimized physical configuration to support reliable virtualization workloads and automated infrastructure operations.

---

## Hardware Topology Validation

### CPU Configuration

- Verified processor count across hosts
- Confirmed socket population
- Identified hosts with single and dual CPU configurations
- Ensured proper CPU detection in system firmware

### Memory Configuration

- Validated DIMM placement symmetry
- Installed memory in slots closest to CPU first
- Avoided unbalanced memory layout
- Confirmed memory channels populated correctly
- Ensured optimal NUMA alignment

### Result

Hosts configured with balanced memory topology to minimize remote memory access and reduce latency.

---

## BIOS Performance Configuration

### System Profile

Set to Performance to prevent CPU throttling and maintain consistent frequency.

### Processor Settings

- Virtualization Technology Enabled
- VT-d Enabled
- Hyper Threading Enabled
- Turbo Boost Enabled
- Logical Processor Idling Disabled
- C States Disabled

### Memory Settings

- Node Interleaving Disabled
- Memory Mode Optimizer

### Boot Mode

- UEFI Enabled

### Result

Configured BIOS to prioritize performance, reduce latency, and ensure stable CPU scheduling for virtualization workloads.

---

## Thermal and Power Configuration

### Thermal Profile

Set to Performance Optimized to prevent thermal throttling.

### Fan Settings

Fan Speed Offset set to Low to maintain adequate cooling without unnecessary noise.

### Power Management

CPU Power Management set to Maximum Performance.

### Result

Improved thermal stability and sustained CPU performance under load.

---

## Storage Preparation

### ESXi Boot Device

Configured RAID1 mirror for hypervisor redundancy.

### vSAN Disks

Configured disks as Non RAID / JBOD to allow direct disk access and proper health monitoring.

### Result

Resilient boot configuration with storage prepared for software defined storage operations.

---

## Validation Summary

- Hardware topology verified
- CPU and memory aligned for NUMA efficiency
- BIOS optimized for performance
- Thermal configuration validated
- Storage prepared for virtualization workloads

---

## Outcome

Hosts are prepared with a stable physical baseline supporting consistent performance, reduced latency, and reliable infrastructure operations prior to ESXi deployment.

---

## Author

Brahim Benhammou
