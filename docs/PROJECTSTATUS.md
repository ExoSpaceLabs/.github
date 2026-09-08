# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-09-08

This document records the current release baseline, dependency relationships, and immediate engineering priorities for the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | State | Public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Active / stable** | **v2.0.0** | 2.x maintenance and downstream adoption |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Active / stable** | **v0.6.0** | Performance-characterization framework and hardware/software overhead profiling |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | **v0.5.1** | Complete post-release evidence/branch cleanup, then continue timing qualification and port work |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Active integration modernization** | Architecture/interface baseline on `main`; no system release yet | Reconcile MCU/Pi/FPGA components and restore coherent system HIL |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Active / migrated** | CCSDSPack v2-compatible `main`; no GitHub release yet | Continue HIL hardening and prepare a versioned host-side baseline |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | **v1.0.0** | Post-1.0 hardening and feature work |

## Dependency Position

CCSDSPack, HardRT, and SpWKit now all have current released baselines. The portfolio bottleneck has shifted away from foundational-library release work and toward EXN component modernization plus performance characterization of the released SpaceWire software boundary.

```text
CCSDSPack 2.0.0 [released]
├── SpWKit 0.6.0 [released]
│   ├── public driver/DMA contract [released]
│   ├── STM32H755 DMA/cache qualification [complete]
│   ├── CCSDSPack interoperability [complete]
│   └── software/provider performance profiling [active]
└── EXN
    ├── central ICD/interfaces [migrated]
    ├── EXN-GS [migrated]
    └── MCU / Pi / FPGA component reconciliation [remaining]

HardRT 0.5.1 [released]
└── candidate RTOS foundation for future EXN MCU integration

SpWKit adoption by EXN remains a separate transport decision.
```

---

## CCSDSPack

**Current release:** **v2.0.0**, published 2026-08-31.

CCSDSPack is the stable packet/API baseline for downstream projects. The release covers CCSDS Space Packet construction, serialization, bounded parsing, supported PUS-A/PUS-C secondary headers, CRC policy, numeric CUC time, stream/sequence handling, validation, hosted package consumption, and representative arm64/Cortex-M7 execution evidence.

### Current portfolio role

- EXN central interfaces validate directly against released CCSDSPack v2.0.0.
- EXN-GS consumes CCSDSPack through released package semantics rather than developer-local paths.
- SpWKit uses CCSDSPack only in standalone interoperability evidence; `libspwkit` remains independent of it.

---

## SpWKit

**Current release:** **v0.6.0**, published 2026-09-08.

v0.6.0 completes the public software boundary required to move applications from virtual SpaceWire transports to platform/vendor hardware drivers without changing the application-facing `spw_port_*` API.

### v0.6.0 baseline

The release provides:

- portable C11 runtime with optional header-only C++17 wrapper;
- process-local simulator and distributed VSPW-TP/UDP transport;
- native POSIX and Windows/Winsock UDP support;
- Linux DEVICE/VSPD integration plus `vspwd`, `spwctl`, `spwmon`, and optional CUSE `/dev/vspwX` presentation;
- caller-owned/no-heap construction and zero-copy ownership paths;
- `SPW_BACKEND_DRIVER` as the stable public hardware-driver callback/configuration contract;
- lifecycle, DATA/EOP/EEP, time-code, readiness, statistics, timeout, and error semantics preserved through the driver backend;
- DMA-capable driver buffers mapped onto the public opaque `spw_buffer_t` ownership API;
- deterministic reference-driver and freestanding/no-heap validation;
- physical NUCLEO-H755ZI-Q Cortex-M7 DMA2 + D-cache ownership qualification through the public driver boundary;
- accepted CCSDSPack v2.0.0 interoperability baseline, including exact PUS-C TC/TM byte preservation over UDP and Linux DEVICE/VSPD;
- two-node Docker Compose integration evidence;
- Debian and GHCR publication for `amd64`, `arm64`, `armhf`, and `riscv64` hosted targets.

The v0.6 public contract intentionally stops at implementation-independent software semantics. Register maps, descriptor layouts, RTL architecture, bus/clock/reset topology, and physical SpaceWire electrical implementation are outside the public API contract.

### Active development after v0.6.0

The next active line is **software abstraction/performance characterization**, tracked by umbrella issue **#133**.

The measurement model deliberately separates four domains:

1. SpWKit public-API software overhead;
2. provider/native interface overhead;
3. controller/FPGA implementation performance;
4. end-to-end physical-link performance.

The first paired-probe profiling substrate is already merged into `develop` under #134. Remaining work covers portable counter backends, TX/RX probe placement, result/statistics schema, copied-vs-zero-copy characterization, direct/native comparison fixtures, STM32H755 cycle measurement, lifecycle-cost profiling, and methodology documentation (#135-#142).

The authoritative software metric is the platform architectural counter value with its counter kind/frequency metadata. Hosted counter ticks must not be mislabeled as dynamic CPU core cycles, and software-only measurements must not be presented as physical SpaceWire throughput or PHY timing.

---

## HardRT

**Current release:** **v0.5.1**, published 2026-09-08.

v0.5.1 is the corrective patch over v0.5.0 for the hosted POSIX execution backend. It preserves the v0.5 public C/C++ API, synchronization-object layouts, and common Cortex-M scheduling contract while replacing the hosted execution mechanism that should have shipped with v0.5.0.

### v0.5 baseline retained

The v0.5 line provides:

- statically allocated tasks and synchronization objects;
- fixed-priority, global round-robin, and priority round-robin scheduling;
- semaphores, owner-tracked mutexes, message queues, 32-bit event flags, and per-task notifications;
- task-context and ISR producer paths with scheduler-aware wake behavior;
- allocation-free C++17 wrappers;
- runtime task creation and EXITED-slot reclamation;
- explicit kernel lifecycle/state rules;
- hardened Cortex-M PendSV, BASEPRI, external-tick, FPU-context, and ISR wake contracts;
- STM32H755 physical qualification with **13/13 functional contracts** and **38/38 benchmark cases**.

### v0.5.1 corrective work

The POSIX backend now:

- uses one pthread per live HardRT application task rather than `ucontext`;
- keeps the HardRT common core authoritative for READY/RUNNING/BLOCKED/SLEEP/EXITED state and scheduling policy;
- uses a monotonic timer pthread for internally owned ticks;
- asynchronously parks/resumes selected task pthreads so CPU-bound code that never enters a HardRT API cannot indefinitely defeat scheduler-selected execution;
- serializes `hrt_tick_from_isr()` through the existing port critical-section contract;
- exports `Threads::Threads` transitively through the installed CMake package.

The POSIX port remains a hosted validation environment, not a hard-real-time timing model. It currently reserves `SIGALRM` and `SIGUSR2` while active.

### Release state and cleanup

The published `0.5.1` tag, `main`, and `develop` are aligned at `43dddff`. The release was physically qualified on the frozen hardware source and then advanced only through release-automation/documentation paths accepted by the release qualification-diff policy.

The release tracker still records two cleanup items:

- attach the retained STM32 physical qualification package and checksum to the GitHub Release;
- remove temporary release/fix branches so the long-lived repository returns to `main` and `develop` only.

Separate post-release work is refining the release-finalization flow for future versions, including architecture-qualified native Linux packages, draft release staging, deterministic STM32 qualification archives, verification, and controlled branch cleanup.

---

## EXN

The central EXN repository is no longer waiting for the CCSDSPack v2 migration. Commit `79ef105` moved the architecture/interface authority onto the released v2 wire contract.

### Completed central migration

- central ICD aligned to CCSDS Space Packet + CCSDSPack v2 semantics;
- CCSDSPack configurations migrated to the v2.0.0 baseline;
- JSON interface mirrors reconciled;
- MCU-facing C headers aligned to the refreshed wire contract;
- interface CI building against the immutable CCSDSPack v2.0.0 tag;
- explicit validation of interface configs, JSON mirrors, and C/C++ header contracts;
- packet-data-length, APID/routing, PUS revision, CRC, and application-field rules reconciled in the central interface authority.

### Remaining system modernization

1. reconcile `exn-mcu-rtos` against the v2 interface contract and select its runtime/RTOS baseline explicitly;
2. reconcile `exn-pi-cam` telemetry/container interfaces;
3. reconcile `exn-fpga-ai` transport/container interfaces;
4. restore cross-component node-to-node regressions and representative system HIL scenarios;
5. explicitly decide whether SpWKit becomes the supported EXN SpaceWire transport and validate that separately if adopted;
6. freeze compatible component revisions and publish a coherent EXN system release.

HardRT v0.5.1 is now available as a candidate foundation for the EXN MCU runtime, but adoption remains an explicit EXN design decision rather than an assumed dependency.

There is currently **no published EXN GitHub release**.

---

## EXN-GS

EXN-GS has completed its original CCSDSPack-v2/reproducible-dependency migration.

### Current implemented baseline

The current `main` branch provides:

- CCSDSPack v2 packet construction, parsing, framing, and routing;
- explicit `find_package(CCSDSPack 2.0 CONFIG QUIET)` consumption with released v2.0.0 fallback;
- isolated/versioned ExternalProject build directories and stale-cache recovery;
- transport daemon separated from operator/application behavior;
- independent daemon IPC and physical/device-link state reporting;
- serialized/stabilized device transport and reconnect handling;
- transport counters plus operational daemon commands;
- FTXUI command-mode and connection-state fixes;
- dedicated repeatable simulation/HIL build tooling;
- packet regressions plus daemon/simulator HIL smoke coverage in CI.

EXN-GS currently pins HardRT v0.4.0 for its STM32 simulator. Moving that dependency to a newer HardRT release should be handled as an explicit integration change with regression evidence rather than silently following the latest tag.

SpWKit is not currently an EXN-GS dependency. If EXN adopts SpaceWire, it should appear as an additional daemon transport backend with its own integration evidence.

There is currently **no published EXN-GS GitHub release**.

---

## Recommended Execution Order

1. **SpWKit:** continue the #133 performance-characterization line on top of the released v0.6.0 contract.
2. **HardRT:** finish 0.5.1 release evidence attachment and branch cleanup, then continue post-0.5 qualification/port work.
3. **EXN MCU:** select the RTOS/runtime baseline and migrate/reconcile the control-node software to the central v2 packet contract.
4. **EXN Pi / FPGA:** reconcile payload and processing interfaces against the same contract.
5. **EXN system:** restore end-to-end simulation/HIL regressions across actual component revisions.
6. **SpaceWire decision:** integrate SpWKit into EXN only where a concrete transport boundary is required and tested.
7. **EXN-GS:** maintain the reproducible migrated baseline and cut a versioned release when its host-side scope is frozen.
8. **EXN:** publish a coherent system baseline after component compatibility and HIL evidence are frozen.

The current portfolio state is materially different from the previous snapshot: **CCSDSPack v2.0.0, HardRT v0.5.1, and SpWKit v0.6.0 are all released foundations. Active engineering has moved toward SpWKit performance characterization and EXN system/component integration.**
