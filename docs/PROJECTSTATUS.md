# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-09-02

This document records the current release baseline, dependency relationships, and immediate engineering priorities for the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | State | Public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Active / stable** | **v2.0.0** | 2.x maintenance and downstream adoption |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Stable maintenance + active development** | **v0.5.1** | Complete v0.6 hardware-driver evidence and release audit |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Active integration modernization** | Architecture/interface baseline on `main`; no system release yet | Reconcile MCU/Pi/FPGA components and restore coherent system HIL |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Active / migrated** | CCSDSPack v2-compatible `main`; no GitHub release yet | Continue HIL hardening and prepare a versioned host-side baseline |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | v0.4.0 | Maintenance and targeted port validation |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | v1.0.0 | Post-1.0 hardening and feature work |

## Dependency Position

The packet-library migration is no longer the portfolio blocker. CCSDSPack v2.0.0 is released and has already been adopted by the central EXN interface baseline and EXN-GS.

```text
CCSDSPack 2.0.0 [released]
├── SpWKit v0.6 [development]
│   ├── installed-package CCSDS/PUS transport integration implemented
│   └── no runtime dependency from libspwkit to CCSDSPack
└── EXN
    ├── central ICD/interfaces [migrated]
    ├── EXN-GS [migrated]
    └── MCU / Pi / FPGA component reconciliation [remaining]

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

v0.5.1 is a maintenance release over v0.5.0 that carries the additive C++17 wrapper-parity fix and refreshed package/API patch metadata. The stable line retains:

- portable C11 runtime with optional C++17 wrapper;
- process-local SpaceWire simulation;
- distributed VSPW-TP/UDP on POSIX and native Windows/Winsock;
- Linux VSPD, `vspwd`, `spwctl`, `spwmon`, and CUSE `/dev/vspwX` presentation;
- installed-package consumers and multi-architecture packages;
- caller-owned/no-heap integration paths and zero-copy ownership semantics where advertised;
- HardRT POSIX/Cortex-M7 integration evidence.

Release engineering has also been consolidated: normal validation is owned by CI, HIL remains explicitly manual, and future release tags are required to point at the exact `main` HEAD. The long-lived repository branches are now `main` and `develop`.

### v0.6 development

`develop` is currently **36 commits ahead of `main`** and carries the active hardware-integration line.

Implemented v0.6 work includes:

- public `SPW_BACKEND_DRIVER` callback/configuration contract;
- preservation of lifecycle, packet EOP/EEP, time-code, readiness, statistics, timeout, and error semantics through the driver backend;
- DMA-capable driver storage mapped onto the existing zero-copy `spw_buffer_t` ownership API;
- deterministic host reference/mock driver coverage;
- no-heap and freestanding/RTOS-friendly driver validation;
- standalone CCSDSPack PUS-C TC/TM transport integration;
- exact byte-preservation tests across UDP and Linux DEVICE/VSPD paths;
- deployment-shaped two-node Docker Compose CCSDS-over-UDP integration;
- STM32H755 DMA integration firmware scaffolding;
- broad documentation and CI/release-policy reconciliation.

### Remaining v0.6 gates

1. **#90:** finalize the CCSDSPack interoperability evidence against the accepted immutable v2.0.0 reference rather than a provisional snapshot.
2. **#119:** execute and record physical STM32H755 DMA/cache ownership evidence. Compile/link coverage alone is not sufficient.
3. **#113:** document the public proprietary-safe FPGA/driver boundary and generic HIL acceptance criteria.
4. Perform the final v0.6 audit, merge `develop` to `main`, tag **v0.6.0**, and verify release publication.

### Explicit non-claims

- RMAP is not currently implemented.
- SpWKit does not contain the proprietary FPGA/HDL SpaceWire core.
- Current software/MCU evidence does not constitute physical SpaceWire electrical/PHY interoperability.

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

1. reconcile `exn-mcu-rtos` against the v2 interface contract;
2. reconcile `exn-pi-cam` telemetry/container interfaces;
3. reconcile `exn-fpga-ai` transport/container interfaces;
4. restore cross-component node-to-node regressions and representative system HIL scenarios;
5. explicitly decide whether SpWKit becomes the supported EXN SpaceWire transport and validate that separately if adopted;
6. only then freeze compatible component revisions and publish a coherent EXN system release.

There is currently **no published EXN GitHub release**.

---

## EXN-GS

EXN-GS has completed its original CCSDSPack-v2/reproducible-dependency migration; issue **#2 is closed as completed**.

### Current implemented baseline

The current `main` branch now provides:

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

1. **SpWKit:** close the remaining v0.6 evidence gates (#90, #119, #113) and release v0.6.0.
2. **EXN MCU:** migrate/reconcile the control-node software to the central v2 packet contract.
3. **EXN Pi / FPGA:** reconcile payload and processing interfaces against the same contract.
4. **EXN system:** restore end-to-end simulation/HIL regressions across actual component revisions.
5. **SpaceWire decision:** integrate SpWKit into EXN only where a concrete transport boundary is required and tested.
6. **EXN-GS:** maintain the now-reproducible migrated baseline and cut a versioned release when its host-side scope is frozen.
7. **EXN:** publish a coherent system baseline after component compatibility and HIL evidence are frozen.

The main portfolio transition since the previous snapshot is that **CCSDSPack adoption is substantially complete at the EXN interface and EXN-GS layers**. Active work has shifted to remaining EXN component integration and the SpWKit v0.6 hardware boundary.
