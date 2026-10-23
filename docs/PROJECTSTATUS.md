# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-10-23

This document records the current release baselines, development state, qualification evidence, dependency relationships, and immediate engineering priorities across the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | Public baseline | Current development state | Immediate next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **v2.0.0** | v2.1 C11-core migration complete; Pi 5 and STM32H755 acceptance passed | Final release PR, promotion, tag and publication |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **v0.7.0** | Functional physical RAW-Ethernet HIL passes; performance qualification remains open | Diagnose ACK/retry anomalies, finish end-to-end transport campaign, then v0.8 readiness freeze |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **v0.5.1** | v0.6 bounded synchronization/timing features implemented; full H755 campaign passed on qualified baseline | Complete release audit, qualification-equivalence controls and 0.6 release closure |
| [EXN](https://github.com/ExoSpaceLabs/exn) | No system release | PUS-C inter-device architecture established; P0 OBC↔Pi path prioritized | Implement and validate real Ground↔Pi↔OBC loop |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | No release yet | Ground/HIL migrated to central PUS-C profile | Continue central-ICD alignment and validate against real OBC/Pi path |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **v1.1.0** | Provider resilience/freshness/provenance released | Browser/WebGL E2E validation, then multi-provider expansion |

## Executive Position

The reusable software foundations are no longer the primary portfolio blocker.

- **CCSDSPack v2.1** is essentially release-complete from an implementation and physical-acceptance standpoint.
- **HardRT v0.6** has the intended bounded synchronization/timing features implemented and a full physical H755 qualification run on the accepted baseline; remaining work is primarily release control and the distinction between measured evidence and later 1.0 analytical guarantees.
- **SpWKit v0.7** has a mature software-visible contract and physical functional evidence, but its end-to-end transport performance still requires root-cause work before the next contract/readiness freeze.
- **EXN** has moved from architecture churn into implementation. P0 is deliberately the OBC + Pi Antenna path; FPGA support remains modular.
- **WorldSat Monitor v1.1.0** is released and has moved into browser-level validation and future provider expansion.

The important distinction is now **software feature completeness vs qualification/release completeness**. Treating every open umbrella issue as an unfinished implementation would badly misrepresent the state of the projects.

---

## CCSDSPack

**Current public release:** **v2.0.0**, published 2026-08-31.

**Current development milestone:** **v2.1.0 release candidate**.

### v2.1 implementation state

The v2.1 line introduces an authoritative C11 protocol core while retaining the existing C++17 ownership/convenience API.

Completed implementation includes:

- C11 result/status and raw-buffer primitives;
- primary Space Packet header encode/decode and bounded packet inspection;
- CRC16 and byte-order primitives;
- numeric CUC support;
- zero-copy/non-owning packet views;
- PUS-A/PUS-C TC and TM codecs;
- allocation-free structured validation primitives;
- segmentation planning;
- receive-side reassembly;
- bounded packet-stream cursor;
- pointer-native packet parsing;
- generic and PUS-specific validator delegation;
- allocation-free packet finalization/caller-buffer encoding;
- installed C API and C-only consumer/package support;
- retained C++17 source compatibility for the v2 API surface.

### Qualification state

The qualified v2.1 candidate has passed:

- hosted Linux/Windows regression and package gates;
- C-only build/consumer validation;
- sanitizer/fuzz coverage;
- native Raspberry Pi 5 / arm64 acceptance;
- NUCLEO-H755ZI-Q / Cortex-M7 physical acceptance using the DAS/OpenOCD harness.

This means v2.1 is **not waiting on missing protocol implementation or missing hardware execution**.

### Remaining release work

- final review of the qualified release candidate;
- verify release-governance checks and build-input qualification equivalence;
- promote the accepted candidate to the release branch/main according to the approved workflow;
- tag and publish v2.1.0;
- retain exact qualification/package/hardware evidence.

Open CFDP/Python/sync-pattern work is future scope and does not redefine v2.1 completion.

---

## HardRT

**Current public release:** **0.5.1**, published 2026-09-08.

**Current development milestone:** **0.6.0**.

### v0.6 implementation state

The 0.6 release theme is bounded synchronization and timing semantics.

Implemented on the development line:

- common wrap-safe timeout/deadline engine;
- semaphore and queue timeout variants;
- timed mutex locking;
- deterministic wake-vs-timeout semantics;
- bounded waiter unlink/expiry behavior;
- bounded/transitive mutex priority inheritance;
- explicit base/effective priority handling;
- stable-phase periodic timing through `hrt_delay_until()`;
- absolute-release semantics that do not accumulate execution/preemption drift;
- observable deterministic deadline-miss behavior;
- integration of the new timing/synchronization semantics with the existing bounded sleeper/deadline structures.

### Qualification state

A complete NUCLEO-H755ZI-Q campaign has already passed on the accepted 0.6 development baseline:

- **16/16 functional contracts passed**;
- **52/52 benchmark metrics passed**;
- **39 firmware/build images** exercised;
- no failures/skips recorded in the full unfiltered run.

The key remaining release concern is that later version/release-workflow changes must not silently invalidate the qualified build-input boundary.

### Remaining 0.6 work

- authoritative root `VERSION` and consistent generated/package versioning;
- 0.6 release notes and documentation reconciliation;
- qualification-aware automatic tagging/release staging;
- build-input equivalence checks between physically qualified candidate and promoted release SHA;
- retain explicit evidence linking source, dependencies, build inputs and physical qualification;
- close any remaining 0.6 characterization/documentation items without overstating observed timing maxima as universal WCET.

### Beyond 0.6

HardRT is **not 1.0-complete**. Later milestones intentionally retain work such as:

- deeper critical-section/interference analysis and configuration-specific analytical bounds;
- scheduler/ISR/task timing decomposition;
- machine-readable timing evidence;
- architecture-neutral task stack API;
- public diagnostic namespace cleanup;
- further static-analysis/API hardening;
- final 1.0 configuration/API freeze and qualification contract.

So the correct status is:

> **0.6 feature implementation: essentially complete. 0.6 release closure: pending. 1.0 hard-real-time qualification/hardening: still active future work.**

---

## SpWKit

**Current public release:** **v0.7.0**, published 2026-09-21.

### Stable software contract

v0.7 establishes:

- authoritative C11 runtime with optional C++17 wrapper;
- process-local simulator;
- VSPW transport semantics;
- UDP and RAW-Ethernet-capable transport-provider architecture;
- Linux DEVICE/VSPD integration;
- public DRIVER backend contract;
- lifecycle, timeout, error and resource semantics;
- zero-copy ownership epochs and DMA/cache integration rules;
- backend behavioral-equivalence tests;
- scoped ECSS-E-ST-50-12C software traceability;
- installed package/container evidence across supported hosted targets;
- physical STM32H755 DMA/cache/driver-boundary validation.

A specific FPGA/HDL implementation is **not** a dependency of the generic SpWKit 1.0 software contract.

### Physical functional status

Current hardware campaign evidence shows functional PASS across:

- PC ↔ Raspberry Pi 5 RAW Ethernet;
- PC ↔ STM32H755 RAW Ethernet;
- Raspberry Pi 5 ↔ STM32H755 RAW Ethernet.

The campaign observed no final payload mismatch or unrecovered functional failure in the qualified runs.

### Performance status

Performance is **not yet qualified**.

The current investigation tracks large throughput/latency variation across host and STM32 clock combinations. The leading but unproven hypothesis involves VSPW ACK/retry timing and whole-message retransmission behavior.

Required work includes:

- low-overhead counters for original TX, retransmissions, DATA/ACK/control frames, timeouts and retry exhaustion;
- matched/unmatched/late ACK accounting;
- duplicate/reassembly statistics;
- bounded ACK-age/turnaround histograms;
- controlled repeated PC/Pi/STM32 tests;
- clear separation between intrinsic sequential request/echo limits and pathological retry/scheduling behavior;
- targeted implementation correction only after the responsible layer is identified;
- full requalification after any justified fix.

This is genuine software/diagnostic/performance work, not merely missing physical SpaceWire PHY validation.

### Toward v1.0

Remaining pre-1 work also includes release governance and final API/ABI freeze activities. The private FPGA implementation may mature in parallel without blocking the generic software contract.

---

## EXN

EXN is the **inter-device system architecture and mission-interface authority**, not the repository for every internal design decision.

### Current architecture baseline

The current central contract establishes:

- PUS Revision C as the mission packet/service profile;
- CCSDSPack v2.x as the packet-library baseline;
- STM32H755 OBC as spacecraft supervisory authority;
- Raspberry Pi 5 as a Linux payload platform hosting logically separate Antenna, Camera and Platform Management roles;
- KV260 as an optional FPGA/AI processing node;
- Ground/HIL as the external command/telemetry/operator environment;
- direct payload-data paths where routing through the OBC would be wasteful;
- optional FPGA deployment rather than making USB/FPGA presence mandatory for a valid OBC configuration.

### P0 system path

The highest-priority integration path is:

```text
Ground / EXN-GS
      |
     Wi-Fi
      |
Pi Antenna service
      |
 UART / COBS / CCSDS / PUS-C
      |
STM32H755 OBC / CM7 / HardRT
```

The system must be commandable and observable through this path without requiring the FPGA.

### OBC CM7/CM4 model

- CM7 remains the sole spacecraft-control authority.
- CM4 may be used for bounded asynchronous/background processing.
- CM4 execution is a component-internal implementation detail, not a second mission-visible OBC authority.
- CM7↔CM4 details belong in `exn-mcu-rtos`, not in the central EXN ICD.

### Interface ownership rule

Central EXN documentation now deliberately keeps only **inter-device mission-visible contracts**.

Examples that belong in component repositories:

- CM7↔CM4 internal contract;
- KV260 PS↔PL architecture;
- Linux service supervision internals;
- hardware driver selection;
- sensor acquisition implementation;
- local scheduling details.

This keeps the ICD usable instead of turning it into a landfill for every internal register and thread.

---

## EXN-GS

EXN-GS is the current Ground/HIL implementation of the central mission protocol.

### Current state

The latest baseline includes:

- PUS-C TC/TM construction and parsing;
- fixed PUS-C source/destination identities;
- PUS acknowledgement flags;
- Service 1 request verification;
- CUC telemetry timestamps;
- central service/interface constants synchronized from EXN;
- recovery-aware System HK snapshot handling;
- Camera configuration interface alignment;
- daemon/UI/CLI architecture;
- Serial/TCP transport;
- STM32 simulator and HIL coverage.

The repository explicitly treats EXN as the mission-protocol authority.

### Dependency note

The simulator still pins **HardRT 0.4.0**. Upstream HardRT is newer, but the EXN-GS dependency should only move when that simulator/runtime upgrade is explicitly validated.

---

## EXN Component Repositories

### exn-pi-cam

The repository has evolved from a camera-only legacy baseline into the implementation home for the shared Raspberry Pi payload platform.

Defined roles:

- **Antenna:** Ground-link ingress/egress and OBC gateway;
- **Camera:** image acquisition/product handling;
- **Platform Management:** supervision, health, logging and recovery support.

Immediate implementation priority is the P0 Wi-Fi Ground ↔ Pi Antenna ↔ OBC UART path. Direct KV260 Ethernet object transfer and Service 220 routing follow later.

### exn-mcu-rtos

The OBC design is now defined around:

- CM7 + HardRT;
- DAS hardware abstraction;
- C11 CCSDSPack/PUS-C handling;
- bounded UART/COBS/CCSDS framing toward the Pi;
- 64-bit time authority;
- endpoint supervision/System HK;
- calibrated board health;
- optional CM4 asynchronous worker;
- optional USB/KV260 control endpoint.

The repository remains implementation-heavy work, not a finished OBC.

### exn-fpga-ai

The FPGA repository now owns:

- KV260 PS/PL internal contracts;
- generic processing-service architecture;
- component-local job-engine/sensor-acquisition design.

Immediate implementation work:

- PS/Linux USB CDC-ACM OBC control endpoint;
- direct Ethernet Service 220 object path;
- reference hardware HK;
- later PL acceleration/inference execution behind the component-owned contract.

---

## WorldSat Monitor

**Current release:** **v1.1.0**.

v1.1 adds:

- bounded CelesTrak retry/backoff;
- catalog caching and stale-cache fallback;
- local catalog search during provider outage;
- orbital-source provenance;
- source/element age and freshness visibility;
- provider health/attention state;
- ordered per-satellite provider-priority policy;
- deterministic fallback to stored orbital elements;
- provider-capabilities API.

Current follow-up priorities:

- browser end-to-end automation;
- actual WebGL/render validation rather than DOM-only confidence;
- Space-Track/multi-provider work under its dedicated issue;
- continued scaling/visualization work only after validation coverage is credible.

---

## SpWKit FPGA

The private `spwkit-fpga` repository is the implementation-specific hardware companion to public SpWKit.

### Ownership boundary

```text
spwkit [public]
├── application API
├── simulator / transport abstractions
├── DRIVER contract
├── lifecycle/error/timeout/resource semantics
├── zero-copy / DMA ownership model
└── software conformance + qualification
         |
         v
spwkit-fpga [private]
├── project-owned SpaceWire RTL
├── controller/link state machine
├── packet datapath / buffering
├── control/status registers
├── KV260 platform integration
├── low-level driver
├── Rev-A LVDS adapter
└── physical SpaceWire HIL
```

### Current state

- architecture/IP-ownership policy defined;
- production path excludes commercial/encrypted protocol IP as an essential dependency;
- Rev-A LVDS adapter is designed and DRC-clean;
- KV260 bring-up/development-environment work is active;
- owned SpaceWire RTL/controller implementation remains pending;
- physical SpaceWire HIL follows after fabric/controller bring-up.

This work is intentionally separate from the generic SpWKit software release contract.

---

## Current Execution Order

1. **CCSDSPack:** close v2.1 release governance and publish the already-qualified implementation.
2. **HardRT:** finish the 0.6 release audit and qualification-safe tagging/promotion flow.
3. **SpWKit:** add observability, reproduce the ACK/retry anomaly, identify the responsible layer, then perform the smallest justified correction and requalification.
4. **EXN Pi:** implement the P0 Antenna gateway and independent platform supervision.
5. **EXN MCU:** build the CM7 HardRT+DAS+C11 CCSDSPack bootstrap and close the real Ground↔Pi↔OBC PUS-C loop.
6. **EXN-GS:** validate the current PUS-C implementation against the real P0 integration path.
7. **SpWKit FPGA:** continue KV260/RTL/controller bring-up without coupling it to generic SpWKit software release readiness.
8. **EXN FPGA AI:** implement optional control/object-transfer endpoints after the P0 spacecraft-control path is operational.
9. **WorldSat Monitor:** close actual browser/WebGL E2E validation before materially broadening UI complexity.

## Dependency Position

```text
CCSDSPack
├── v2.0.0 released
└── v2.1 qualified candidate
    ├── HardRT/EXN future C11 consumers
    └── downstream packet/interface baseline

HardRT
├── 0.5.1 released
└── 0.6 implementation + H755 qualification complete
    └── release closure active

SpWKit
├── v0.7.0 released
├── functional hardware-provider evidence
└── end-to-end transport performance qualification active

EXN
├── central PUS-C ICD
├── EXN-GS implementation
├── Pi P0 Antenna path
├── MCU/OBC implementation
└── optional FPGA integration
```

The portfolio is now in the comparatively healthy stage where most of the difficult questions are about **qualification boundaries, measured behavior and integration order**, rather than whether the basic architecture exists at all.
