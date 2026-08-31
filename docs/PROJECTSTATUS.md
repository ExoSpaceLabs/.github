# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-08-31

This document records the current engineering lifecycle, release baseline, dependency relationships, and next milestone for the ExoSpaceLabs portfolio. Repository issue trackers and release pages remain authoritative for implementation details and release evidence.

## Portfolio Dependency Map

The current program branches from the CCSDSPack v2 release rather than forming one strict linear dependency chain:

```text
CCSDSPack 2.0.0
├── SpWKit #90: optional installed-package CCSDS/PUS transport interoperability
└── EXN / EXN-GS: migrate existing CCSDS/PUS consumers and interfaces to 2.x
    └── SpaceWire/SpWKit adoption is separate EXN transport scope if selected
```

SpWKit **v0.5.0 publication recovery** can proceed in parallel with CCSDSPack 2.0.0 release closure because CCSDSPack is not a runtime dependency of the SpWKit core.

## Status Summary

| Project | State | Current public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Release candidate** | v1.2.0 | Close/waive remaining v2 gates and publish v2.0.0 |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Publication recovery** | v0.4.0; v0.5.0 tag exists but publication is incomplete | Repair the failed release path and publish v0.5.0; then complete CCSDSPack 2.x interoperability in #90 |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Modernization** | Architecture/interface repository; no coherent current system release | Migrate shared packet contracts/components to CCSDSPack 2.x and restore system integration evidence |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Modernization** | Source baseline 0.1.0 | Remove developer-local CCSDSPack fallback and validate against released 2.x packages |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | v0.4.0 | Continue targeted RTOS/port validation and maintenance |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | v1.0.0 | Continue normal post-1.0 feature and operational hardening |

---

## CCSDSPack

**Role:** C++17 library for CCSDS Space Packet PDUs and supported ECSS PUS-A/PUS-C secondary headers, with explicit packet policy, parsing/validation, CUC time support, packaging, and conformance evidence.

### Current state

- Latest public release: **v1.2.0**, published 2026-08-02.
- `develop` declares **2.0.0** and contains the standards/API refactor.
- Release-preparation PR **#128** was merged on 2026-08-26 after the v2 software release-validation matrix passed.
- v2 is not yet the public `main`/release baseline.
- There are no open pull requests currently blocking the cut.
- Remaining release uncertainty is concentrated in release tracker **#87**, where unchecked items still need explicit completion, evidence, waiver, or deferral.

### Release position

CCSDSPack is at **release closure**, not open-ended feature development. New non-critical API/protocol work should stay outside the v2.0.0 gate unless it resolves a demonstrated release defect.

### Required actions

1. Audit every unchecked item in #87 and classify it as required, evidenced complete, or deferred.
2. Execute hardware-validation gates that remain mandatory, or record an explicit waiver/deferral.
3. Promote/reconcile the validated v2 line according to the release process.
4. Create and publish **v2.0.0** with release artifacts and installed-package consumer verification.
5. Treat the published 2.0.0 package/API contract as the downstream migration baseline.

### Downstream impact

Publishing CCSDSPack 2.0.0 unblocks:

- SpWKit issue **#90**, which proves CCSDSPack-produced PUS/CCSDS bytes survive SpWKit transport unchanged without making CCSDSPack a SpWKit runtime dependency;
- EXN modernization issue **#2**;
- EXN-GS migration issue **#2**.

---

## SpWKit

**Role:** Portable C11 SpaceWire software API with an optional C++17 wrapper, deterministic local simulation, distributed VSPW-TP/UDP links, Linux virtual-device/service paths, hosted POSIX/Windows support, and embedded/no-heap integration contracts.

### Current implemented scope

Current repository evidence includes:

- stable public C API with optional header-only C++ wrapper;
- loopback and process-local simulator paths;
- VSPW-TP/UDP distributed virtual SpaceWire transport;
- Linux VSPD/`vspwd` service path and production CUSE `/dev/vspwX` presentation;
- native Winsock VSPW-TP parity;
- hosted package-consumer validation;
- HardRT POSIX integration and Cortex-M7 compile/link/no-heap evidence.

**RMAP is not currently implemented in this repository and should not be listed as a present SpWKit capability.** Physical SpaceWire interoperability also remains a separate hardware/HIL claim.

### Current release state

- Latest published GitHub release: **v0.4.0**.
- Repository version: **0.5.0**.
- An immutable **v0.5.0** tag exists.
- Release workflow run `32470238125` passed the major package and installed-consumer gates.
- The **multi-architecture OCI image** job failed in that release attempt, so release evidence generation, GitHub Release/GHCR publication, and post-publish verification were skipped.

Issue **#87** is the v0.5 umbrella and now contains the current publication-recovery evidence.

### Required actions

#### A. Recover v0.5.0 publication

1. Diagnose and repair the failing OCI release path.
2. Rerun the failed release jobs against the existing immutable v0.5.0 tag.
3. Publish GitHub Release/container artifacts.
4. Complete post-publish verification and close the v0.5 release boundary.

#### B. Add CCSDSPack 2.x interoperability afterward

Tracked in **#90**.

1. Wait for public CCSDSPack 2.0.0.
2. Pin the validated released package contract, not moving `develop`.
3. Build standalone producer/consumer applications against installed CCSDSPack and SpWKit packages.
4. Generate representative PUS TC/TM bytes with CCSDSPack, transport through independent SpWKit peers, verify exact byte identity, and parse/validate on the receiver.
5. Keep CCSDSPack completely outside the `libspwkit` runtime dependency graph.
6. Publish the interoperability evidence in a follow-up release rather than changing the existing v0.5.0 tag.

---

## EXN

**Role:** Architecture/interface project for a modular satellite-avionics demonstrator spanning Raspberry Pi payload processing, STM32 control software, FPGA processing, shared CCSDS/PUS interfaces, and ground/HIL tooling.

### Current state

The architecture remains useful, but the current repository is **not a reliable integrated release baseline**. Its shared packet definitions and component assumptions predate the CCSDSPack v2 contract and need reconciliation.

The repository has therefore been marked **Modernization** and its README now explicitly distinguishes implemented interfaces from planned SpaceWire integration.

### Required actions

Tracked in **#2**.

1. Publish CCSDSPack 2.0.0.
2. Migrate shared EXN packet/interface definitions to the released 2.x contract.
3. Migrate direct CCSDSPack consumers in component repositories.
4. Restore clean-checkout CI and cross-component CCSDS/PUS regressions.
5. Reconcile the central ICD/documentation with actual implemented behavior.
6. Decide explicitly whether SpWKit becomes the EXN SpaceWire layer. If adopted, implement/test it as separate transport scope rather than describing it as an existing dependency.
7. Cut a coherent EXN system baseline only after dependencies and integration evidence are versioned and reproducible.

### Acceptance criteria

- no developer-local dependency paths;
- explicit CCSDSPack compatibility/version policy;
- reproducible clean-clone builds;
- CCSDS/PUS interoperability tests across relevant component boundaries;
- representative command, housekeeping, payload-data, and HIL/simulation scenarios;
- architecture/ICD documentation matching implemented contracts;
- no SpaceWire/SpWKit claim without corresponding implementation and validation.

---

## EXN-GS

**Role:** C++17 EXN ground-control/HIL environment with daemon, FTXUI terminal interface, command-line client, Serial/TCP device links, CCSDS/PUS handling, and STM32-oriented simulation.

### Current state

`cmake/deps.cmake` currently:

1. tries `find_package(CCSDSPack QUIET)`;
2. if that fails, attempts to build CCSDSPack from the developer-specific absolute path `/home/dev/Works/CCSDSPack`;
3. creates a local imported/alias target for that build.

This means a clean checkout is not reproducible on another machine unless a compatible CCSDSPack package is already installed. There is **no current SpWKit dependency in EXN-GS**, and the README has been updated to state that explicitly.

The repository root also contains local/generated-looking `.idea` and `logs` directories; issue #2 now includes a hygiene review rather than silently treating those as deliberate product assets.

### Required actions

Tracked in **#2**.

1. Remove the developer-local absolute dependency fallback.
2. Consume released CCSDSPack 2.x through its canonical CMake package target/version policy.
3. Adapt packet/PUS construction, parsing, and error handling to the v2 API.
4. Add clean-checkout installed-package CI.
5. Re-run the existing Serial/TCP daemon/simulator regression paths.
6. Only add SpWKit if EXN deliberately introduces a SpaceWire transport backend for EXN-GS.

---

## HardRT

**Role:** Small portable RTOS written in C, with static allocation, configurable scheduling, semaphores, mutexes, message queues, POSIX/Cortex-M ports, and an optional C++17 wrapper.

### Current state

- Current release: **v0.4.0**.
- The project has a clear standalone scope and is not blocked by the CCSDSPack/EXN release program.
- SpWKit already uses HardRT as an external integration proof without making HardRT a SpWKit runtime dependency.

### Priority

Maintain RTOS/port validation and avoid coupling generic HardRT core scope to EXN-specific requirements unless those requirements produce reusable RTOS functionality.

---

## WorldSat Monitor

**Role:** Self-hosted satellite/constellation situational-awareness platform with public orbital-data ingestion, backend SGP4 propagation, persistent object/group management, and an interactive 3D Earth interface.

### Current state

- Current release: **v1.0.0**.
- The project has a clear user-facing scope, documented source/image deployment, backend propagation, and service-oriented packaging.
- It is independent of the avionics-library modernization program.

### Priority

Treat v1.0.0 as the stable baseline and continue normal post-release hardening/features independently.

---

## Recommended Execution Order

1. **CCSDSPack:** close v2 release gates and publish 2.0.0.
2. **SpWKit in parallel:** repair the v0.5.0 release publication path and complete publication of the existing tag.
3. **After CCSDSPack 2.0.0:** execute SpWKit #90 interoperability proof.
4. **EXN-GS:** migrate the first concrete host-side CCSDSPack consumer and restore clean package integration.
5. **EXN component repositories:** migrate remaining packet/interface consumers and reconcile the central ICD.
6. **EXN transport decision:** adopt/test SpWKit only where the architecture actually needs SpaceWire.
7. **EXN system baseline:** validate end-to-end and cut a coherent post-modernization release.

This order separates release engineering, packet-library migration, and transport integration into auditable gates instead of combining them into one unbounded modernization branch.
