# VMware DevOps Infrastructure Platform Foundation

## Overview

This project documents the design and implementation of a segmented, highly available VMware infrastructure platform built to simulate real world enterprise architecture.

The environment serves as a foundation for applying DevOps automation to manage, operate, and optimize VMware infrastructure as a platform.

---

## Project Goal

My goal is to apply DevOps automation to manage and operate VMware infrastructure as a platform.

This lab provides a realistic environment where automation workflows, infrastructure as code, and platform engineering practices can be developed and validated.

---

# VMware Infrastructure Design

## Goal

Design a segmented and highly available VMware infrastructure platform that supports reliable operations, automation workflows, and realistic lab testing aligned with enterprise architecture practices.

## Purpose

Provide a stable foundation for testing infrastructure automation, high availability features, and workload deployment scenarios in a controlled lab environment.

---

## Physical Hardware Inventory

| Device | Model               | Role                        | Primary Network   |
| ------ | ------------------- | --------------------------- | ----------------- |
| Host 1 | Dell PowerEdge R640 | Production Node A           | 10Gb SFP+         |
| Host 2 | Dell PowerEdge R640 | Production Node B           | 10Gb SFP+         |
| Host 3 | Dell PowerEdge R630 | Management and Witness Node | 1Gb Ethernet      |
| Switch | Catalyst 3750X      | Layer 3 Switching           | 10Gb SFP+ and 1Gb |

## Reliability Strategy

Three node cluster

Witness quorum design

Enhanced vMotion Compatibility enabled

Traffic isolation using dedicated networks

High speed storage and migration paths


---
# Physical Infrastructure Baseline

## Objective

Prepare hosts with consistent performance optimized configuration for virtualization workloads.

## Hardware Validation

CPU topology verified

Memory symmetry validated

NUMA alignment confirmed

Balanced memory configuration

## BIOS Configuration

Performance profile enabled

Virtualization and VT-d enabled

Hyper Threading enabled

Turbo Boost enabled

C States disabled

Node interleaving disabled

UEFI boot enabled

## Thermal and Power

Performance optimized cooling

Maximum CPU performance mode

## Storage

Each host includes four 1TB drives  with no RAID to allow direct disk access for VMware vSAN to entralized storage for the workload cluster

## Outcome

Stable physical platform supporting consistent performance and low latency operations.

## Logical Network Design

| VLAN | Name       | Subnet         | Gateway     | Purpose               |
| ---- | ---------- | -------------- | ----------- | --------------------- |
| 10   | Management | 10.10.10.0/24  | 10.10.10.1  | ESXi vCenter AD DNS   |
| 20   | vMotion    | 10.10.20.0/24  | 10.10.20.1  | Live Migration        |
| 30   | vSAN       | 10.10.30.0/24  | 10.10.30.1  | Storage Traffic       |
| 100  | VM Network | 10.10.100.0/24 | 10.10.100.1 | Applications| servers| Workloads |
| 110  | DevOps     | 10.10.110.0/24 | 10.10.110.1 | Automation Tools      |

---

## Core Services

| Service          | IP          | Role                      |
| ---------------- | ----------- | ------------------------- |
| OPNsense         | 10.10.10.1  | Routing and Firewall      |
| Active Directory | 10.10.10.10 | Identity and DNS          |
| vCenter          | 10.10.10.20 | Infrastructure Management |

---


# Underlay Switch Baseline

## Purpose

Layer 3 underlay configuration for segmented VMware lab using 802.1Q trunks.

R640 hosts carry all VLANs

R630 carries Management plus VM Network plus DevOps

---

## VLAN Plan

VLAN 10 MANAGEMENT

VLAN 20 VMOTION

VLAN 30 VSAN

VLAN 100 VM NETWORK

VLAN 110 DEVOPS

---

## Jumbo Frames

Configured MTU 9000 to support high throughput vMotion and vSAN traffic reducing CPU overhead and improving performance.

VMware VMkernel MTU configured as well to support mtu 9000

---

## Network Verification

VLAN and trunk validation

Switchport configuration verification

Interface status validation

MAC table verification

---

# Skills Demonstrated

VMware platform architecture

Infrastructure design

High availability planning

Network segmentation

Hardware performance tuning

Layer 3 switching

Enterprise lab design

Automation ready infrastructure

---

# Tools and Technologies

VMware vSphere

ESXi

vCenter

Dell PowerEdge servers

Cisco Catalyst switching

OPNsense firewall

Networking fundamentals

Infrastructure documentation

---

# Intended Use

Infrastructure engineering portfolio

Platform engineering learning environment

Foundation for DevOps automation

Enterprise lab reference architecture

---

# Future Enhancements

PowerCLI automation

Terraform infrastructure as code

Configuration management

Monitoring and logging

Backup and disaster recovery

Automated deployment pipelines

---

# Author

Brahim Benhammou

VMware Infrastructure Dev-Ops Engineering Focus

---

# Project Status

Active and continuously evolving platform engineering project
