# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-08-31

This document records the current release baseline, dependency relationships, and immediate engineering priorities for the ExoSpaceLabs portfolio.

## Portfolio Status

| Project | State | Public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Active / stable** | **v2.0.0** | 2.x maintenance and downstream adoption |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Active development** | **v0.5.0** | Finish v0.6 hardware-driver/DMA boundary and release audit |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Modernization** | No coherent current system release | Migrate packet contracts to CCSDSPack v2.0.0 |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Modernization** | Source baseline 0.1.0 | Replace local CCSDSPack assumptions with the released package |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | v0.4.0 | Maintenance and targeted port validation |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | v1.0.0 | Post-1.0 hardening and feature work |

## Dependency Position

CCSDSPack v2.0.0 is now the released packet-layer baseline:

```text
CCSDSPack 2.0.0 [released]
├── SpWKit v0.6: optional installed-package CCSDS/PUS transport evidence
└── EXN / EXN-GS: migrate existing CCSDS/PUS consumers to 2.x
    └── adopt SpWKit separately only where SpaceWire transport is implemented
```

SpWKit does not depend on CCSDSPack at runtime. CCSDSPack is used only by standalone interoperability tests and examples.

---

## CCSDSPack

**Current release:** **v2.0.0**, published 2026-08-31.

CCSDSPack is no longer a release candidate. `main` and `develop` are reconciled at the post-publication evidence baseline.

The v2 release provides the supported C++17 packet-layer contract for:

- CCSDS Space Packet construction, serialization, bounded parsing, and validation;
- supported PUS-A and PUS-C TC/TM secondary headers;
- packet-level CRC policy;
- numeric basic CUC time support;
- `ccsds::Manager` stream/sequence handling;
- raw pointer-plus-size and vector transport adapters;
- installed CMake package consumption for hosted applications.

Release evidence includes 132/132 native regression/conformance tests, sanitizer and bounded fuzz-smoke jobs, Linux and Windows integration, installed-package consumers, native arm64 execution on Raspberry Pi 5, and physical Cortex-M7 execution on NUCLEO-H755ZI-Q.

### Position

Treat **v2.0.0** as the immutable downstream integration baseline. Future CFDP, stream synchronization/resynchronization, Python bindings, and broader CCSDS protocol scope remain later work and are not implied by v2.0.0.

### Immediate downstream work

- SpWKit issue #90: pin final transport evidence to the immutable v2.0.0 release.
- EXN issue #2: migrate shared packet/interface contracts.
- EXN-GS issue #2: consume the released CMake package rather than developer-local paths.

---

## SpWKit

**Current stable release:** **v0.5.0**, published 2026-08-31.

The previous publication-recovery state is closed. v0.5.0 is the stable `main` baseline and includes:

- portable C11 API with optional C++17 wrapper;
- loopback and process-local simulation;
- distributed VSPW-TP/UDP transport;
- POSIX and Windows/Winsock hosted support;
- Linux VSPD, `vspwd`, tooling, and CUSE `/dev/vspwX` presentation;
- hosted package-consumer validation;
- multi-architecture Debian packages and runtime container publication;
- HardRT POSIX and Cortex-M7 integration evidence.

### v0.6 development

`develop` is the v0.6 integration branch. Umbrella issue #108 tracks the hardware-driver integration boundary.

Already implemented on `develop`:

- portable public hardware-driver/configuration contract;
- preservation of packet terminators, time codes, link/readiness/statistics, timeout, and error semantics through the driver backend;
- DMA-capable driver buffers mapped into the existing zero-copy ownership API;
- deterministic host reference/mock driver coverage;
- no-heap and bare-metal/RTOS-friendly driver boundaries;
- CCSDSPack v2 PUS-C TC/TM transport fixture;
- exact byte-preservation tests over UDP and Linux DEVICE/VSPD peers;
- deployment-shaped two-node Docker Compose CCSDS-over-UDP example.

### Remaining v0.6 gates

1. **#90:** replace the provisional CCSDSPack snapshot with immutable **v2.0.0** and rerun the existing integration evidence.
2. **#119:** validate the driver/DMA ownership boundary on physical STM32H755 hardware, including DMA and cache/coherency behavior.
3. **#113:** document the public FPGA/driver integration boundary and generic HIL acceptance criteria without exposing implementation-specific hardware internals.
4. Complete the v0.6 audit, merge `develop` to `main`, tag **v0.6.0**, and verify release publication.

### Explicit non-claims

- RMAP is not currently implemented.
- SpWKit does not contain a real FPGA SpaceWire HDL core.
- Current software tests do not constitute physical SpaceWire electrical/PHY interoperability evidence.

---

## EXN / EXN-GS

The EXN architecture remains useful, but the current integration baseline predates CCSDSPack v2 and is not yet a reproducible modern system release.

Immediate priorities are:

1. migrate shared packet interfaces and direct consumers to CCSDSPack v2.0.0;
2. remove developer-local dependency paths;
3. restore clean-checkout CI and cross-component packet regressions;
4. reconcile the ICD/documentation with implemented behavior;
5. add SpWKit only where a real SpaceWire transport path is deliberately implemented and tested.

## Recommended Execution Order

1. Finalize SpWKit #90 against CCSDSPack v2.0.0.
2. Complete SpWKit STM32H755 DMA/runtime evidence (#119).
3. Finish the public hardware integration boundary documentation (#113).
4. Audit and release SpWKit v0.6.0.
5. Migrate EXN-GS to the released CCSDSPack package.
6. Migrate remaining EXN packet consumers and restore end-to-end integration evidence.

CCSDSPack v2.0.0 and SpWKit v0.5.0 are now published baselines. The active work has moved to SpWKit v0.6 hardware integration and downstream CCSDSPack adoption.
