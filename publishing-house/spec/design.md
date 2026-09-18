# GPU Node Setup and AI Workloads on Red Hat OpenShift

<!-- This file is the design document for your lab or demo. -->
<!-- Fill in each section below, or run /rhdp-publishing-house to have the intake skill help. -->
<!-- Sections marked with [brackets] are placeholders — replace with real content. -->
<!-- The validation gate checks for all required sections before submission. -->

## Overview

This lab gives technical sales professionals hands-on experience setting up GPU infrastructure on Red Hat OpenShift and running AI workloads on it. Participants will install the NVIDIA GPU Operator and Node Feature Discovery Operator on a running OpenShift cluster, then use Red Hat OpenShift AI to deploy and execute a Jupyter notebook on a GPU-accelerated node.

## Target Audience

- **Role:** Technical sales (solutions architects, sales engineers, technical account managers)
- **Experience level:** Intermediate
- **What they already know:** Basic OpenShift navigation and operator concepts
- **What they don't know:** GPU node configuration, NVIDIA GPU Operator, Red Hat OpenShift AI workbench setup

## Prerequisites

- Basic familiarity with Red Hat OpenShift (navigating the console, installing operators)
- Can the lab validate these automatically? No — trust-based

## Learning Objectives

1. Install the NVIDIA GPU Operator and Node Feature Discovery Operator on Red Hat OpenShift
2. Configure a GPU-enabled worker node for AI workloads
3. Deploy and run a Jupyter notebook on a GPU-accelerated node using Red Hat OpenShift AI

## Content Type

Lab (hands-on)

## Products & Technologies

- Red Hat OpenShift 4.21
- NVIDIA GPU Operator
- Node Feature Discovery (NFD) Operator
- Red Hat OpenShift AI

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| 1 | Installing GPU Operators | 30 min |
| 2 | Running GPU-Accelerated Workloads | 30 min |
| — | **Total hands-on** | **1 hour** |
| — | Intro / presentation | ~5 min |
| — | **Total lab** | **~1 hour 5 min** |

## Difficulty Level

Intermediate

## Environment

**Learner view:** A running OpenShift 4.21 cluster on AWS with a GPU-capable worker node available but not yet GPU-enabled. Admin credentials are pre-provided. Participants work primarily through the OpenShift console — navigating OperatorHub to install operators, then switching to the OpenShift AI dashboard to launch a Jupyter workbench.

**Automation needed:** Yes — automation must provision an AWS GPU instance (e.g., g4dn or p3 family) as a worker node in the cluster, with any required driver dependencies pre-staged so participants start at the operator installation step.

## Infrastructure Requirements

- **Cloud provider:** AWS
- **Cluster type:** Multinode
- **OCP version:** 4.21
- **Topology:** Per-student
- **Sizing:** 3 control plane (8 vCPU, 32GB RAM); 2 standard workers (8 vCPU, 32GB RAM, 100GB disk); 1 GPU worker (g6.8xlarge — 1x NVIDIA L4)
- **Automation approach:** Ansible
- **AI/MaaS:** GPU — direct GPU node required. MaaS insufficient; lab objective is GPU infrastructure configuration using NVIDIA GPU Operator and NFD on real hardware.
- **External services:** nvcr.io, registry.redhat.io, registry.access.redhat.com
- **AAP version:** N/A
- **Non-GA products:** None (all products are GA)
