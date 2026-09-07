# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-09-07

This document records the current release baseline, dependency relationships, and immediate engineering priorities for the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | State | Public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Active / stable** | **v2.0.0** | 2.x maintenance and downstream adoption |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Stable maintenance + active development** | **v0.5.1** | Complete v0.6 hardware-driver evidence, proprietary FPGA boundary, and release audit |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | **v0.5.0** | Post-0.5 maintenance, timing qualification, and downstream adoption |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Active integration modernization** | Architecture/interface baseline on `main`; no system release yet | Reconcile MCU/Pi/FPGA components and restore coherent system HIL |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Active / migrated** | CCSDSPack v2-compatible `main`; no GitHub release yet | Continue HIL hardening and prepare a versioned host-side baseline |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | **v1.0.0** | Post-1.0 hardening and feature work |

## Dependency Position

The packet-library migration is no longer the portfolio blocker. CCSDSPack v2.0.0 is released and has already been adopted by the central EXN interface baseline and EXN-GS. HardRT v0.5.0 is also released, moving the real-time foundation from release qualification into downstream adoption.

```text
CCSDSPack 2.0.0 [released]
├── SpWKit v0.6 [development]
│   ├── installed-package CCSDS/PUS transport integration implemented
│   ├── public driver/DMA contract implemented
│   ├── planned private spwkit-fpga implementation boundary
│   └── no runtime dependency from libspwkit to CCSDSPack
└── EXN
    ├── central ICD/interfaces [migrated]
    ├── EXN-GS [migrated]
    └── MCU / Pi / FPGA component reconciliation [remaining]

HardRT 0.5.0 [released]
└── candidate RTOS foundation for future EXN MCU integration

SpWKit adoption by EXN remains a separate transport decision.
```

---

## CCSDSPack

**Current release:** **v2.0.0**, published 2026-08-31.

CCSDSPack is the stable packet/API baseline for downstream projects. The current release covers CCSDS Space Packet construction, serialization, bounded parsing, supported PUS-A/PUS-C secondary headers, CRC policy, numeric CUC time, stream/sequence handling, validation, hosted package consumption, and representative arm64/Cortex-M7 execution evidence.

### Current portfolio role

- EXN central interfaces now validate directly against released CCSDSPack v2.0.0.
- EXN-GS now consumes CCSDSPack through released package semantics rather than developer-local paths.
- SpWKit uses CCSDSPack only in standalone interoperability evidence; `libspwkit` remains independent of it.

---

## SpWKit

**Current stable release:** **v0.5.1**, published 2026-09-02.

v0.5.1 is the stable maintenance line while v0.6 development establishes the reusable hardware-integration boundary. The stable line provides:

- portable C11 runtime with optional C++17 wrapper;
- process-local SpaceWire simulation;
- distributed VSPW-TP/UDP on POSIX and native Windows/Winsock;
- Linux VSPD, `vspwd`, `spwctl`, `spwmon`, and CUSE `/dev/vspwX` presentation;
- installed-package consumers and multi-architecture packages;
- caller-owned/no-heap integration paths and zero-copy ownership semantics where advertised;
- HardRT POSIX/Cortex-M7 integration evidence.

The long-lived repository branches remain `main` and `develop`. `develop` carries the active v0.6 hardware-integration line.

### v0.6 development

Implemented v0.6 work includes:

- public `SPW_BACKEND_DRIVER` callback/configuration contract;
- preservation of lifecycle, packet EOP/EEP, time-code, readiness, statistics, timeout, and error semantics through the driver backend;
- DMA-capable driver storage mapped onto the existing zero-copy `spw_buffer_t` ownership API;
- deterministic host reference/mock driver coverage;
- no-heap and freestanding/RTOS-friendly driver validation;
- standalone CCSDSPack PUS-C TC/TM transport integration;
- immutable CCSDSPack v2.0.0 integration pin and exact-tag verification in CI;
- exact byte-preservation tests across UDP and Linux DEVICE/VSPD paths;
- deployment-shaped two-node Docker Compose CCSDS-over-UDP integration;
- STM32H755 DMA integration firmware scaffolding;
- broad documentation and CI/release-policy reconciliation.

### Public/private FPGA boundary

The public `spwkit` repository owns the reusable software contract: driver callbacks, packet/link semantics, DMA ownership rules, simulation, reference drivers, and generic HIL acceptance criteria.

The proprietary hardware implementation is intentionally separated into the planned private **`spwkit-fpga`** project. That project is intended to own implementation-specific material such as RTL/HDL architecture, SpaceWire logic, DMA engines/descriptors, register/address maps, interrupt wiring, clock/reset topology, board projects, constraints, and hardware-specific driver integration.

This boundary keeps the public software API sufficient for third-party or proprietary hardware implementations without exposing implementation internals.

### Remaining v0.6 gates

1. **#90:** explicitly accept the pinned CCSDSPack v2.0.0 baseline and close the integration fixture acceptance gate.
2. **#119:** execute and record physical STM32H755 DMA/cache ownership evidence. Compile/link coverage alone is not sufficient.
3. **#113:** finish the public proprietary-safe FPGA/driver boundary and generic HIL acceptance criteria, aligned with the `spwkit-fpga` separation.
4. Perform the final v0.6 audit, merge `develop` to `main`, tag **v0.6.0**, and verify release publication.

### Explicit non-claims

- RMAP is not currently implemented.
- The public SpWKit repository does not contain the proprietary FPGA/HDL SpaceWire implementation.
- Current software/MCU evidence does not constitute physical SpaceWire electrical/PHY interoperability.

---

## HardRT

**Current release:** **v0.5.0**, published 2026-09-06.

HardRT v0.5.0 completes the release line that previously remained under qualification. `main` and `develop` are aligned on the same release commit, with temporary feature/release branches removed.

### v0.5.0 baseline

The release adds and validates:

- statically allocated 32-bit event flags with wait-any/wait-all and clear-on-exit semantics;
- per-task 32-bit notifications with set-bits, overwrite, no-overwrite, and saturating-increment actions;
- ISR-safe event/notification producer paths with scheduler-aware wake decisions;
- allocation-free C++17 wrappers for the new synchronization surface;
- scheduler and lifecycle correctness hardening, including explicit kernel states and runtime task reclamation rules;
- deterministic hosted event/notification stress under priority, global round-robin, and priority round-robin policies;
- strengthened Cortex-M behavior for PendSV, critical sections, external tick ownership, FPU context preservation, and ISR wake paths;
- STM32H755 functional and DWT timing/profiling evidence;
- documentation drift checks, Doxygen validation, and explicit pre-1.0 compatibility policy;
- published POSIX, Cortex-M, bundle, and qualification-evidence release artifacts.

HardRT is therefore no longer a v0.5 release blocker. Future work can focus on post-0.5 timing/qualification issues and on controlled adoption by downstream projects such as the future EXN MCU implementation.

---

## EXN

The central EXN repository is no longer waiting for the CCSDSPack v2 migration. Commit `79ef105` moved the architecture/interface authority onto the released v2 wire contract.

### Completed central migration

- central ICD aligned to CCSDS Space Packet + CCSDSPack v2 semantics;
- CCSDSPack configurations migrated to the v2.0.0 baseline;
- JSON interface mirrors reconciled;
- MCU-facing C headers aligned to the refreshed wire contract;
- interface CI added, building against the immutable CCSDSPack **v2.0.0** tag;
- explicit validation of interface configs, JSON mirrors, and C/C++ header contracts;
- packet-data-length, APID/routing, PUS revision, CRC, and application-field rules reconciled in the central interface authority.

This means the **central EXN packet contract is migrated**. The project should no longer be described as waiting for that work.

### Remaining system modernization

The incomplete portion is now downstream integration:

1. reconcile `exn-mcu-rtos` against the v2 interface contract and select its current RTOS/runtime baseline explicitly;
2. reconcile `exn-pi-cam` telemetry/container interfaces;
3. reconcile `exn-fpga-ai` transport/container interfaces;
4. restore cross-component node-to-node regressions and representative system HIL scenarios;
5. explicitly decide whether SpWKit becomes the supported EXN SpaceWire transport and validate that separately if adopted;
6. only then freeze compatible component revisions and publish a coherent EXN system release.

HardRT v0.5.0 is now available as a qualified candidate foundation for the EXN MCU runtime, but adoption remains an explicit EXN design decision rather than an assumed dependency.

There is currently **no published EXN GitHub release**.

---

## EXN-GS

EXN-GS has completed its original CCSDSPack-v2/reproducible-dependency migration; issue **#2 is closed as completed**.

### Current implemented baseline

The current `main` branch provides:

- CCSDSPack v2 packet construction, parsing, framing, and routing;
- explicit `find_package(CCSDSPack 2.0 CONFIG QUIET)` consumption with released v2.0.0 fallback instead of developer-local absolute paths;
- isolated/versioned ExternalProject build directories and stale-cache recovery;
- HardRT pinned to **v0.4.0** for the STM32 simulator;
- transport daemon separated from operator/application behavior;
- independent daemon IPC and physical/device-link state reporting;
- serialized/stabilized device transport and reconnect handling;
- transport counters plus `PING`, `STATUS`, `STATS`, `CONNECT`, `DISCONNECT`, and `RECONNECT` operational commands;
- FTXUI command-mode and connection-state fixes;
- direction-aware packet panes and hardened client/daemon behavior;
- dedicated `scripts/build_simulation.sh` for repeatable simulation/HIL builds;
- packet regressions plus daemon/simulator HIL smoke coverage in CI;
- repository hygiene cleanup for runtime/build/IDE artifacts.

The resulting host-side architecture is now a practical migrated baseline rather than a placeholder modernization target:

```text
EXN-GS UI / CLI
       |
   local IPC
       |
   exn_gsd
       |
  Serial / TCP
       |
STM32 hardware or stm32_sim
```

SpWKit is **not currently an EXN-GS dependency**. If EXN adopts SpaceWire, it should appear as an additional daemon transport backend with its own integration evidence.

There is currently **no published EXN-GS GitHub release**, so the next maturity step is to turn the validated `main` source baseline into an explicit versioned release once the desired host-side feature boundary is frozen.

---

## Recommended Execution Order

1. **SpWKit:** close the remaining v0.6 evidence and boundary gates (#90, #119, #113), establish the `spwkit-fpga` private implementation boundary, and release v0.6.0.
2. **EXN MCU:** select the RTOS/runtime baseline and migrate/reconcile the control-node software to the central v2 packet contract. HardRT v0.5.0 is now available as a candidate foundation.
3. **EXN Pi / FPGA:** reconcile payload and processing interfaces against the same contract.
4. **EXN system:** restore end-to-end simulation/HIL regressions across actual component revisions.
5. **SpaceWire decision:** integrate SpWKit into EXN only where a concrete transport boundary is required and tested.
6. **EXN-GS:** maintain the reproducible migrated baseline and cut a versioned release when its host-side scope is frozen.
7. **EXN:** publish a coherent system baseline after component compatibility and HIL evidence are frozen.

The portfolio is now in a cleaner state: **CCSDSPack v2.0.0 and HardRT v0.5.0 are released foundations, SpWKit v0.6 is the active hardware-integration release line, and EXN component modernization is the major remaining system-level program.**
