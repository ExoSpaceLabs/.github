# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-10-26

This document records the current release baselines, active development lines, qualification state, dependency relationships, and immediate engineering priorities across the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | Public baseline | Active development state | Immediate next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **v2.1.0** | v2.2 validation-ownership refactor active; develop is ahead of main | Complete C11-as-single-protocol-authority audit across C++ boundaries |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **0.6.0** | Released and qualified; main and develop aligned | Begin 0.7 timing/interference qualification and analytical-bound work |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **v0.7.0** | End-to-end transport campaign active; develop substantially ahead of release main | Resolve ACK/retry/performance anomalies, finish qualification, then v0.8 readiness freeze |
| [EXN](https://github.com/ExoSpaceLabs/exn) | No system release | Central PUS-C architecture and complete component-to-component ICD authority defined | Implement and validate P0 Ground ↔ Pi communications ↔ STM32 OBC path |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | No release yet | PUS-C Ground/HIL baseline implemented | Validate against real Pi/OBC path and keep mission semantics synchronized with EXN |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **v1.1.0** | Post-v1.1 UI resilience/mobile work on develop; Sites deployment PR active | Browser/WebGL E2E qualification and release-grade visual validation |

## Executive Position

The portfolio has moved materially since the previous snapshot:

- **CCSDSPack v2.1.0 is now published**, not a release candidate. The C11 core is the released architecture; v2.2 is tightening the remaining C++/C protocol-validation boundary.
- **HardRT 0.6.0 is now published**, with the full STM32H755 qualification already completed. The project has moved past 0.6 release closure into the 0.7 hard-real-time timing/interference phase.
- **SpWKit v0.7.0 remains the stable release**, but active development is now centered on end-to-end transport diagnostics and performance qualification rather than basic functionality.
- **EXN has clarified interface ownership**: EXN owns every mission-visible component-to-component contract, including interfaces between co-located Pi services. Only true implementation-internal contracts remain in component repositories.
- **WorldSat Monitor v1.1.0 remains the stable application baseline**, while develop has accumulated post-release UI/mobile/recovery validation and the serverless Sites path remains an active PR.
- **SpWKit FPGA has advanced from repository/setup planning into real KV260 bring-up**, with a standard-VHDL blink path and safer JTAG probing now present, while visible/measurable GPIO validation and the actual SpaceWire controller/RTL remain open.

---

## CCSDSPack

**Current public release:** **v2.1.0**, published 2026-10-25.

### Released v2.1 baseline

v2.1 establishes:

- authoritative C11 protocol core;
- compatible C++17 ownership/convenience layer;
- Space Packet header/wire primitives;
- CRC16 and endian helpers;
- PUS-A/PUS-C TC/TM codecs;
- numeric CUC time;
- allocation-free packet views;
- structured validation;
- segmentation/reassembly;
- stream walking;
- pointer-native parsing;
- caller-buffer packet finalization/encoding;
- C-only and C++ installed-package consumers.

Release qualification included native Raspberry Pi 5/arm64 acceptance and NUCLEO-H755ZI-Q/Cortex-M7 acceptance.

### v2.2 active work

The development branch is now focused on a stricter architectural rule:

> every protocol/wire rule has one authority: the C11 core.

Current work includes:

- removing duplicated C++ prevalidation before C codecs;
- routing primary-header numeric/range validation through C11;
- auditing PUS setters and TM decode boundaries;
- preserving C++ object-state/ownership invariants without duplicating protocol constants;
- adding C-only/C++ parity and negative/error-vector tests;
- retaining performance evidence where validation-path changes affect cost.

Open v2.2 PRs currently include the primary-header validation refactor and PUS TM validation-boundary cleanup.

The released v2.1.0 behavior is not being rewritten by this work.

---

## HardRT

**Current public release:** **0.6.0**, published 2026-10-25.

### 0.6 released capability

0.6 adds the synchronization/timing semantics that were previously under qualification:

- common absolute-tick timeout/deadline engine;
- timed semaphore, queue, mutex, event and notification operations;
- bounded waiter timeout/expiry handling;
- statically bounded transitive priority inheritance under fixed-priority policies;
- stable-phase periodic release via hrt_delay_until();
- explicit deadline-miss semantics that preserve nominal phase;
- documented distinction between structural bounds, measured timing evidence and universal WCET claims.

### Qualification state

The qualified NUCLEO-H755ZI-Q campaign passed:

- **16/16 functional contracts**;
- **52/52 benchmark metrics**;
- **39 build/flash images**;
- no failed/skipped release-gate cases.

The release is now published and main / develop are aligned.

### Qualification evidence policy

The repository now keeps concise qualification summaries as the durable release record rather than storing complete raw logs from every deterministic run. Detailed diagnostics are regenerated on demand from the canonical qualification scripts when required.

### Next phase: 0.7

The next substantive engineering work is deeper hard-real-time qualification:

- critical-section and bounded-operation characterization;
- scheduler/context-switch decomposition;
- ISR-to-task timing decomposition;
- IPC handoff/copy-cost analysis;
- periodic release jitter and interference characterization;
- machine-readable timing evidence;
- conversion of measured behavior into configuration-specific analytical bounds where justified.

Pre-1 API hardening, architecture-neutral stack storage, diagnostic namespacing and final configuration/API freeze remain later roadmap work.

Open 0.6 tracker issues may remain as historical/cleanup trackers; they no longer mean the 0.6 release is unpublished.

---

## SpWKit

**Current public release:** **v0.7.0**, published 2026-09-21.

### Stable software contract

The release provides:

- C11 runtime plus optional C++17 wrapper;
- deterministic simulator;
- VSPW transport semantics;
- UDP and RAW-Ethernet-capable provider architecture;
- Linux DEVICE/VSPD support;
- public DRIVER backend contract;
- lifecycle, timeout, error, resource and zero-copy ownership semantics;
- reusable backend-contract/equivalence validation;
- scoped ECSS-E-ST-50-12C software traceability;
- STM32H755 DMA/cache/provider-boundary evidence.

### Physical functional status

Functional hardware testing passes across:

- PC ↔ Raspberry Pi 5;
- PC ↔ STM32H755;
- Raspberry Pi 5 ↔ STM32H755.

The tested STM32 runs verified payload integrity without final application mismatch, MCU fault or unrecovered link failure.

### Active performance investigation

Performance interpretation is still not qualified.

Current development includes:

- in-memory CPU profiling;
- TX-slot diagnostics;
- transport/carrier accounting;
- ACK/retry observability;
- comparison of PC/Pi host behavior against STM32 clock profiles;
- repeatable benchmark orchestration.

The active investigation must determine whether the observed throughput anomalies are dominated by:

- ACK timeout/retransmission behavior;
- AF_PACKET/host scheduling;
- STM32 polling/MAC-DMA behavior;
- fragmentation/control overhead;
- or an intrinsic sequential reliable-echo ceiling.

No timeout/default change should be treated as a fix until that root cause is measured.

### Branch/release note

The development line is substantially ahead of v0.7.0 main and remains the active pre-v0.8 engineering line. Release-branch reconciliation/governance is still part of the pre-1 cleanup.

---

## EXN

EXN is the **single authority for system architecture and mission-visible component-to-component ICDs**.

### Interface ownership

The current rule is:

- EXN owns interfaces between logical components, regardless of physical deployment.
- PI-CAM ↔ PI-COMMS is therefore an EXN-owned component interface even when both run on one Raspberry Pi.
- Internal CM7↔CM4 IPC, KV260 PS↔PL implementation, Linux process layouts, driver internals and Ground daemon/UI internals stay in their component repositories.

EXN owns APIDs, PUS services/subservices, payload layouts, routing semantics, HK contracts, events and recovery protocols.

### Current architecture

Mission interfaces use CCSDS/PUS Revision C.

Logical components include:

- OBC / STM32H755 — spacecraft authority;
- PI-CAM — camera/product component;
- PI-COMMS — Ground-link component;
- PI-MGMT — Pi supervision/recovery component;
- FPGA — image/AI processing component;
- Ground/HIL.

The current bench topology is:

```text
Ground ↔ Pi          : Wi-Fi
Pi ↔ STM32 OBC       : 3.3 V UART + COBS/CCSDS/PUS-C
STM32 ↔ KV260        : USB CDC-ACM control, optional by deployment
Pi ↔ KV260           : direct Gigabit Ethernet for payload objects
```

### P0 integration path

The first complete system path remains:

```text
Ground / EXN-GS
      ↓
PI-COMMS / Antenna
      ↓
UART / COBS / CCSDS / PUS-C
      ↓
STM32H755 OBC / CM7 / HardRT
```

The system must be commandable and observable without requiring the FPGA.

### Current implementation gap

The architecture and central ICD are ahead of the component implementations. Remaining system work is concentrated in:

- real OBC bootstrap/runtime;
- real Pi communications/router path;
- C11 CCSDSPack/PUS-C endpoint implementation;
- time/HK/supervision;
- optional USB/KV260 control;
- payload object paths;
- complete node-to-node HIL.

---

## EXN-GS

EXN-GS implements the Ground/HIL side of the central EXN protocol.

Current capabilities include:

- PUS-C TC/TM construction/parsing;
- CRC-protected CCSDS framing;
- Service 1 verification;
- CUC telemetry timestamps;
- System HK snapshot requests/replies;
- central endpoint/service constants synchronized from EXN;
- transport daemon separated from operator/application logic;
- FTXUI operator client and CLI;
- Serial/TCP transports;
- STM32 simulator and HIL smoke coverage.

The repository must continue to consume mission semantics from EXN rather than defining independent protocol variants.

The simulator still pins HardRT 0.4.0; upgrading that dependency should remain an explicit regression-tested change rather than following upstream automatically.

---

## EXN Component Repositories

### exn-pi-cam

Current state:

- Camera, Antenna/PI-COMMS and Platform Management are defined as separate logical components on one Pi;
- their mission-visible component interfaces are owned centrally by EXN;
- local process/transport implementation details remain in this repository;
- platform sensor acquisition/normalization responsibility is defined;
- P0 remains the Ground ↔ PI-COMMS ↔ OBC path.

Implementation still pending includes:

- Wi-Fi Ground-facing PI-COMMS service;
- independently supervised exn-linkd UART/COBS/PUS-C router;
- Pi health/restart behavior;
- later direct KV260 Ethernet object transfer.

### exn-mcu-rtos

The OBC architecture is defined around:

- CM7 + HardRT;
- DAS hardware abstraction;
- released C11 CCSDSPack/PUS-C;
- UART/COBS/CCSDS framing toward the Pi;
- 64-bit time authority;
- endpoint supervision/System HK;
- board health/watchdog/reset cause;
- optional CM4 asynchronous worker;
- feature-gated USB CDC-ACM link toward KV260.

The repository remains implementation-heavy. Immediate blockers are the CM7 bootstrap, PUS-C gateway, Pi UART path and the P0 real Ground↔Pi↔OBC loop.

### exn-fpga-ai

The repository owns FPGA implementation internals such as:

- KV260 PS/PL design;
- local job-engine behavior;
- sensor acquisition internals.

Mission-visible FPGA interfaces remain defined by EXN.

Immediate implementation work includes:

- PS/Linux USB CDC-ACM OBC control endpoint;
- direct Ethernet Service 220 payload-object path;
- reference hardware HK;
- later PL acceleration/inference implementation.

---

## WorldSat Monitor

**Current application baseline:** **v1.1.0**.

### v1.1 baseline

v1.1 includes:

- bounded CelesTrak retry/backoff;
- catalog caching and stale fallback;
- orbital-source provenance;
- source/element age and freshness;
- provider health/attention state;
- deterministic provider-priority/fallback policy;
- provider-capabilities API;
- persistent satellite/group/provider/propagation state;
- Docker Compose deployment.

### Active post-v1.1 development

The development branch is ahead of release main and now includes additional UI qualification work such as:

- mobile control usability;
- visible/recoverable API error paths;
- diagnostics for failure states.

Open work still includes:

- real browser Playwright E2E against production-built Compose;
- actual WebGL/MapLibre rendered-scene validation;
- screenshot/pixel-difference evidence for visual behavior;
- release pull-only browser smoke against published images;
- later Space-Track/multi-provider work.

A separate open PR provides a serverless ChatGPT Sites backend/deployment path while preserving the normal Docker Compose architecture.

---

## SpWKit FPGA

The private SpWKit FPGA repository is the implementation-specific hardware companion to public SpWKit.

### Current bring-up state

The repository has progressed beyond architecture-only setup:

- KV260/Vivado bring-up documentation is active;
- a minimal blink design has been cleaned up and standardized on VHDL;
- superseded SystemVerilog blink RTL has been removed;
- JTAG probing now includes a guard against freezing a live Linux system;
- Rev-A LVDS adapter design remains available and DRC-clean.

The first FPGA validation issue remains open because the complete reproducible bring-up and visible/measurable GPIO proof is not yet closed.

### Remaining implementation

Still pending:

- validated GPIO/ILA workflow;
- LVDS TX/RX electrical I/O;
- serializer/deserializer building blocks;
- project-owned SpaceWire link-state/controller RTL;
- packet datapath/buffering;
- PS↔PL integration;
- low-level SpWKit provider/driver integration;
- physical SpaceWire HIL.

This repository remains intentionally separate from generic SpWKit software-release readiness.

---

## Current Active Pull Requests

At this snapshot the main active engineering PRs are:

- **CCSDSPack:** v2.2 C11 validation-ownership refactors;
- **SpWKit:** end-to-end transport performance campaign;
- **WorldSat Monitor:** serverless ChatGPT Sites deployment path preserving Compose.

HardRT has no equivalent active release PR because 0.6.0 is already published and main/develop are aligned.

---

## Current Execution Order

1. **SpWKit:** complete diagnostics/root-cause work for the ACK/retry performance anomaly, then requalify the affected transport path.
2. **EXN MCU:** implement CM7 HardRT+DAS + CCSDSPack v2.1 baseline and bring up the Pi UART/PUS-C path.
3. **EXN Pi:** implement PI-COMMS/exn-linkd and independently supervised P0 Ground↔Pi↔OBC routing.
4. **EXN-GS:** validate the existing PUS-C Ground/HIL stack against the real Pi/OBC path.
5. **CCSDSPack:** continue v2.2 single-authority validation cleanup without destabilizing released v2.1.
6. **HardRT:** begin 0.7 timing/interference qualification and analytical-bound work on top of the released 0.6 contract.
7. **SpWKit FPGA:** close reproducible KV260/GPIO bring-up, then advance into LVDS and owned SpaceWire RTL.
8. **WorldSat Monitor:** finish browser/WebGL E2E qualification while integrating the alternative Sites deployment cleanly.
9. **EXN FPGA AI:** implement the optional control/object endpoints after the P0 spacecraft-control path is operational.

## Dependency Position

```text
CCSDSPack
├── v2.1.0 released
└── v2.2 boundary/validation refactor active

HardRT
├── 0.6.0 released
├── main == develop
└── 0.7 timing/interference qualification next

SpWKit
├── v0.7.0 released
├── physical functionality qualified
└── performance/root-cause qualification active

EXN
├── central PUS-C architecture + complete component ICD ownership
├── EXN-GS implementation baseline
├── Pi P0 communications implementation pending
├── MCU/OBC implementation pending
└── optional FPGA integration pending

WorldSat Monitor
├── v1.1.0 baseline
├── post-release UI validation on develop
└── browser/WebGL + Sites work active
```

The portfolio is now split cleanly between **released foundations**, **measurement/qualification work**, and **EXN component implementation**. Those categories should remain explicit so an open roadmap issue is not mistaken for an unreleased or missing core feature.
