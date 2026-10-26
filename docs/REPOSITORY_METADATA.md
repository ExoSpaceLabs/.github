# ExoSpaceLabs Repository Metadata

**Snapshot:** 2026-10-26

This document is the canonical review baseline for GitHub repository descriptions and topics across ExoSpaceLabs.

Repository topics should describe the **project domain, use case, protocol, platform family, or product role**. Avoid language-version/toolchain tags unless they are genuinely central to discovery. Topics such as `c11`, `cpp17`, `python`, `react`, `vite`, `docker-compose`, or `posix` are implementation details and should normally stay in the README rather than repository metadata.

Descriptions should explain what the project **is and does**, not list its entire implementation stack.

## Public repositories

### CCSDSPack

**Description**

> C11/C++17 CCSDS Space Packet library with PUS-A/PUS-C, CUC time, validation, segmentation/reassembly, and embedded/hosted integration.

**Topics**

`ccsds`, `space-packet`, `ecss`, `pus`, `telecommand`, `telemetry`, `cuc-time`, `satellite-communications`, `spacecraft-software`, `packet-validation`, `protocol-library`

**Remove / avoid**

`arm`, `bare-metal`, `cpp17`, `embedded`, `space`, `satellite`, `stm32`, `comms`, `mission-control`

### HardRT

**Description**

> Small static-allocation hard-real-time RTOS for Cortex-M with deterministic scheduling, bounded synchronization, periodic timing, and reproducible qualification.

**Topics**

`rtos`, `hard-real-time`, `real-time-kernel`, `embedded-systems`, `cortex-m`, `scheduler`, `deterministic-systems`, `static-allocation`, `priority-inheritance`, `real-time-systems`, `stm32`

**Remove / avoid**

`freertos-alternative`, `baremetal`, `baremetal-kernel`, `posix`, `operating-system`, `rtos-library`

### SpWKit

**Description**

> Portable SpaceWire software toolkit for simulation, HIL, distributed testing, embedded systems, and hardware-backed links through a stable driver contract.

**Topics**

`spacewire`, `ecss`, `spacecraft-communications`, `spacecraft-networking`, `spacecraft-software`, `hardware-in-the-loop`, `simulation`, `distributed-testing`, `embedded-systems`, `device-driver`, `space-systems`

**Remove / avoid**

`c`, `c11`, `cpp17`, `linux`, `dma`, `zero-copy`, `rtos`, `ccsds`

### EXN

**Description**

> Modular spacecraft-avionics demonstrator and ICD authority for an STM32 OBC, Raspberry Pi payload/communications services, FPGA processing, and Ground/HIL.

**Topics**

`spacecraft-avionics`, `satellite-avionics`, `earth-observation`, `ccsds`, `pus`, `spacecraft-software`, `hardware-in-the-loop`, `onboard-computer`, `payload-processing`, `ground-station`, `stm32`, `raspberry-pi`, `fpga`

### EXN-GS

**Description**

> Ground-station and HIL environment for EXN, implementing CCSDS/PUS-C command, telemetry, simulation, and Serial/TCP spacecraft links.

**Topics**

`ground-station`, `satellite-ground-station`, `hardware-in-the-loop`, `ccsds`, `pus`, `telecommand`, `telemetry`, `satellite-communications`, `spacecraft-operations`, `simulation`, `exn`

### WorldSat Monitor

**Description**

> Self-hosted satellite and constellation situational-awareness platform with SGP4 propagation, orbital-source health, persistent monitoring, and interactive 3D Earth visualization.

**Topics**

`satellite-tracking`, `space-situational-awareness`, `orbital-mechanics`, `orbit-propagation`, `sgp4`, `celestrak`, `constellation-monitoring`, `3d-globe`, `mission-visualization`, `satellite-operations`

**Remove / avoid**

`docker`, `docker-compose`, `nextjs`, `postgresql`, `python`, `react`, `typescript`, `vite`, `aerospace`, `space`

## Private repositories

### SpWKit FPGA

**Description**

> Private FPGA/HDL implementation of the SpWKit SpaceWire hardware provider, including project-owned RTL, KV260 integration, LVDS PHY adaptation, and HIL.

**Topics**

`spacewire`, `fpga`, `rtl`, `hdl`, `kv260`, `kria`, `spacewire-controller`, `spacecraft-communications`, `hardware-in-the-loop`, `lvds`, `xilinx`, `vivado`

**Important correction**

The current description, “private commercial IP and driver for the SpaceWire library”, is inaccurate. The production policy explicitly requires project-owned RTL and rejects commercial/encrypted SpaceWire RTL as an essential dependency.

### EXN Pi

**Description**

> Raspberry Pi 5 implementation of EXN PI-CAM, PI-COMMS, and PI-MGMT services for imaging, Ground communications, payload routing, and platform supervision.

**Topics**

`exn`, `raspberry-pi`, `spacecraft-payload`, `satellite-communications`, `earth-observation`, `camera`, `ccsds`, `pus`, `spacecraft-software`, `payload-routing`, `platform-management`

### EXN MCU RTOS

**Description**

> STM32H755 EXN on-board computer using HardRT, DAS, and CCSDSPack for spacecraft control, PUS-C routing, time, health supervision, and payload orchestration.

**Topics**

`exn`, `stm32h755`, `onboard-computer`, `spacecraft-obc`, `satellite-avionics`, `hard-real-time`, `ccsds`, `pus`, `spacecraft-software`, `fault-management`, `embedded-systems`

### EXN FPGA AI

**Description**

> KV260 EXN payload-processing node for image preprocessing, AI inference, postprocessing, payload-object transfer, and OBC-controlled processing jobs.

**Topics**

`exn`, `kv260`, `kria`, `fpga`, `payload-processing`, `edge-ai`, `earth-observation`, `spacecraft-payload`, `image-processing`, `spacecraft-software`

## Organization repositories

### .github

**Description**

> Organization profile, portfolio status, and shared public GitHub configuration for ExoSpaceLabs.

Topics are unnecessary for this special repository.

### .github-private

**Description**

> Private ExoSpaceLabs organization profile and internal repository-status documentation.

Topics are unnecessary for this special repository.

## Topic policy

Prefer roughly **6–12 strong topics** per project. A good topic should answer one of these questions:

- What domain is this project in?
- What standard/protocol does it implement?
- What product/system role does it play?
- What hardware/platform family is central to the project?
- What would someone reasonably search GitHub for to discover it?

Do not use repository topics as a dependency manifest. Compiler language versions, build systems, frontend frameworks, databases, container tools, individual optimization techniques, and incidental platforms belong in project documentation unless they are defining characteristics of the project.
