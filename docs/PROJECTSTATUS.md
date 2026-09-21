# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-09-21

This document records the current release baselines, dependency relationships, and immediate engineering priorities for the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | State | Public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Active / stable** | **v2.0.0** | 2.x maintenance and downstream adoption |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Active / stable** | **v0.7.0** | v0.8 API/ABI cleanup and ECSS architecture/assurance phase |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | **v0.5.1** | Standardized release finalization, then timing/qualification and port work |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Active architecture / integration** | Packet/interface + routing architecture on `main`; no system release yet | Implement/reconcile MCU, Linux payload, camera, communications, FPGA, and end-to-end HIL |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Active / migrated** | CCSDSPack v2-compatible `main`; no GitHub release yet | Continue HIL hardening and prepare a versioned host-side baseline |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | **v1.0.0** | Post-1.0 hardening and feature work |

## Dependency Position

The foundational libraries are no longer the primary portfolio blocker. CCSDSPack v2.0.0, HardRT v0.5.1, and SpWKit v0.7.0 are released; active work is now concentrated on SpWKit's pre-1.0 standards/contract phase and EXN implementation/integration.

```text
CCSDSPack 2.0.0 [released]
├── SpWKit 0.7.0 [released]
│   ├── hardware-driver/DMA public contract [stable]
│   ├── backend equivalence + runtime semantics [stable]
│   ├── scoped ECSS-E-ST-50-12C traceability [released]
│   └── v0.8 contract/standards phase [next]
└── EXN
    ├── central packet interfaces [migrated]
    ├── payload-routing architecture [defined]
    ├── EXN-GS [migrated]
    └── component implementation + system HIL [remaining]

HardRT 0.5.1 [released]
└── available RTOS foundation for explicit downstream adoption
```

---

## CCSDSPack

**Current release:** **v2.0.0**, published 2026-08-31.

CCSDSPack remains the stable packet/API baseline for downstream projects. The release covers CCSDS Space Packet construction, serialization, bounded parsing, supported PUS-A/PUS-C secondary headers, CRC policy, numeric CUC time, stream/sequence handling, validation, hosted package consumption, and representative arm64/Cortex-M7 execution evidence.

### Current portfolio role

- EXN central interfaces validate against released CCSDSPack v2.0.0.
- EXN-GS consumes CCSDSPack through released package semantics.
- SpWKit uses CCSDSPack only in standalone interoperability evidence; `libspwkit` remains independent of it.

---

## SpWKit

**Current release:** **v0.7.0**, published 2026-09-21.

v0.7.0 builds on the v0.6 hardware-provider boundary and performance work by making the software-visible backend contract explicit and executable before the planned v0.8 cleanup phase.

### v0.7.0 baseline

The release provides or hardens:

- portable C11 runtime with optional header-only C++17 wrapper;
- process-local simulation, VSPW-TP/UDP, Linux DEVICE/VSPD, CUSE presentation, and the portable DRIVER backend;
- explicit same-handle serialization and distinct-handle concurrency semantics;
- complete-operation timeout semantics and canonical result/error behavior;
- resource and lifecycle rules for start/stop/reset/close;
- reset-safe zero-copy ownership epochs and stale-handle rejection;
- reusable backend-contract tests and versioned behavioral-equivalence evidence across SIMULATOR, UDP, DEVICE/VSPD, and DRIVER;
- scoped ECSS-E-ST-50-12C Rev.1 applicability/traceability with specifically enumerated positive software claims and explicit delegated/future/not-applicable boundaries;
- physical NUCLEO-H755ZI-Q Cortex-M7 DMA/cache/zero-copy evidence through the public driver boundary;
- accepted CCSDSPack v2.0.0 interoperability baseline;
- controlled release-performance comparison against immutable v0.6.1;
- retained VSPW-TP 4096-byte RX paired-overhead reduction from 60,281 to 25,032 invariant-counter ticks in the documented hosted environment;
- multi-architecture Debian and GHCR publication for `amd64`, `arm64`, `armhf`, and `riscv64`.

The public software claim stops before implementation-specific FPGA/controller/PHY/electrical behavior. Hosted profiling numbers are regression/reference evidence for their named environments, not physical SpaceWire performance specifications.

### Next phase: v0.8

v0.8 is the last planned phase in which intentional public API/ABI cleanup may occur before the v0.9 freeze.

Planned work includes:

- ECSS-E-ST-40 applicability and requirements-to-design-to-test traceability;
- ECSS-Q-ST-80 applicability/product-assurance gap analysis;
- any standards-driven API or architecture corrections while breaking changes are still permitted;
- disposition of deferred software-visible SpaceWire capabilities such as distributed interrupts, applicable standardized node-management parameters, and the SpaceWire MIB/service;
- continued ECSS-E-ST-50-12C traceability as the public software surface evolves.

The intended sequence is **v0.8 architecture/standards cleanup → v0.9 API freeze/assurance → v1.0 stable software contract**.

### Repository hygiene note

Immediately after the v0.7.0 release, `main` and `develop` are not yet fully reconciled and multiple temporary release/audit/performance branches remain. These should be reconciled/pruned before substantial v0.8 integration work accumulates.

---

## HardRT

**Current release:** **v0.5.1**, published 2026-09-08.

v0.5.1 remains the current stable RTOS line. It preserves the public v0.5 C/C++ API and Cortex-M scheduling contract while correcting hosted POSIX execution.

### Stable baseline

The v0.5 line provides:

- static task/kernel storage with runtime task creation and EXITED-slot reclamation;
- fixed-priority, global round-robin, and priority round-robin scheduling;
- semaphores, owner-tracked mutexes, message queues, 32-bit event flags, and per-task notifications;
- task-context and ISR producer paths with scheduler-aware wake behavior;
- allocation-free C++17 wrappers;
- hardened Cortex-M PendSV, BASEPRI, external-tick, FPU-context, and ISR wake contracts;
- NUCLEO-H755ZI-Q physical qualification with **13/13 functional contracts** and **38/38 benchmark cases**;
- scheduler-controlled pthread execution and asynchronous hosted preemption for CPU-bound POSIX tasks.

### Development state

`develop` is currently one commit ahead of `main` with standardized release-finalization infrastructure:

- native Linux amd64/arm64 POSIX release packaging;
- deterministic STM32 qualification packaging;
- draft-release staging/finalization;
- checksum verification;
- qualification-diff enforcement;
- controlled branch cleanup;
- corresponding documentation and CI validation.

The 0.5.1 release tracker remains open for retained physical-evidence attachment and cleanup of temporary release/fix branches.

---

## EXN

The central EXN repository now defines more than the packet contract. The September architecture update establishes the payload/service routing model that downstream implementations are expected to follow.

### Current architecture baseline

The current `main` baseline defines:

- CCSDSPack v2.0.0-aligned packet/interface authority;
- Ground Station / HIL, MCU control node, Linux payload platform, camera service, communications service, and FPGA processing roles;
- the OBC as the supervisory control point rather than a mandatory data-path hop;
- direct Camera ↔ FPGA, Camera ↔ Communications, and FPGA ↔ Communications payload-data paths where operationally appropriate;
- configurable routing of FPGA processing/classification results toward OBC, payload services, or ground-facing communications according to use case;
- Linux payload platform-management responsibilities such as watchdog/health/logging around independently managed services;
- dedicated ICDs for GS, MCU, Linux payload/communications, Pi camera, and FPGA processing interfaces.

Architecture changes are explicitly separated from wire-format changes: packet layouts remain governed by the shared central interface definitions.

### Remaining implementation work

1. implement/reconcile `exn-mcu-rtos` against the current MCU ICD and select its runtime/RTOS baseline explicitly;
2. modernize `exn-pi-cam` around the Linux payload service boundary and current packet contract;
3. implement `exn-fpga-ai` against the current processing/routing contract;
4. implement the communications endpoint/service and validate direct payload routing;
5. restore cross-component node-to-node simulation and representative HIL scenarios;
6. freeze compatible component revisions before publishing a coherent EXN system release.

There is currently **no published EXN GitHub release**.

---

## EXN-GS

EXN-GS remains the current migrated host-side ground/HIL baseline.

The current `main` provides:

- CCSDSPack v2 packet construction, parsing, framing, and routing;
- released-dependency fallback and isolated/versioned dependency builds;
- transport daemon separated from operator/application behavior;
- independent daemon IPC and physical/device-link state reporting;
- transport counters and operational daemon commands;
- hardened reconnect/device behavior;
- repeatable simulation/HIL build tooling;
- packet regressions plus daemon/simulator HIL smoke coverage in CI.

EXN-GS still pins HardRT v0.4.0 for its STM32 simulator. Any dependency upgrade should be explicit and regression-tested rather than silently following the latest HardRT tag.

There is currently **no published EXN-GS GitHub release**.

---

## Recommended Execution Order

1. **SpWKit:** reconcile post-v0.7 branches, then begin the v0.8 API/ABI + ECSS architecture phase.
2. **EXN:** turn the now-defined routing/service architecture into concrete component interfaces and implementations.
3. **EXN MCU:** select the runtime/RTOS baseline and implement against the current central ICD.
4. **EXN Linux payload:** modernize camera and communications services around the shared payload-service model.
5. **EXN FPGA:** implement processing and configurable result-routing behavior against the frozen architecture.
6. **EXN system:** restore end-to-end simulation/HIL across actual compatible component revisions.
7. **HardRT:** complete release-evidence/branch cleanup and continue qualification/port work without destabilizing the 0.5.1 baseline.
8. **EXN-GS:** evolve the host-side baseline only as required by explicit system integration contracts.

The main portfolio transition since the previous snapshot is substantial: **SpWKit has advanced from v0.6.0 performance-characterization work to the released v0.7.0 behavioral/ECSS contract, while EXN has advanced from packet migration into a concrete dynamic payload-routing architecture.**
