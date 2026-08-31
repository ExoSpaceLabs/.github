# ExoSpaceLabs Project Status

[Organization profile](../profile/README.md)

**Snapshot:** 2026-08-31

This document records the current engineering lifecycle, release baseline, dependency relationships, and next milestone for the ExoSpaceLabs portfolio. Repository issue trackers and release pages remain authoritative for implementation details and release evidence.

## Portfolio Critical Path

The current integration dependency chain is:

`CCSDSPack 2.0.0` → `SpWKit CCSDSPack 2.x integration` → `EXN / EXN-GS modernization`

SpWKit **v0.5.0 publication recovery** can proceed in parallel with CCSDSPack 2.0.0 release closure because CCSDSPack support is optional in the current SpWKit baseline.

## Status Summary

| Project | State | Current public baseline | Next milestone |
| --- | --- | --- | --- |
| [CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack) | **Release candidate** | v1.2.0 | Close/waive remaining v2 gates and publish v2.0.0 |
| [SpWKit](https://github.com/ExoSpaceLabs/spwkit) | **Publication recovery** | v0.4.0; v0.5.0 tag exists but publication is incomplete | Repair OCI release job and publish v0.5.0, then migrate optional CCSDSPack integration to 2.x |
| [EXN](https://github.com/ExoSpaceLabs/exn) | **Modernization** | Architecture/integration repository; no coherent current release baseline | Re-establish a versioned dependency and integration baseline on CCSDSPack 2.x and current SpWKit |
| [EXN-GS](https://github.com/ExoSpaceLabs/exn-gs) | **Modernization** | Source baseline 0.1.0 | Remove developer-local/unpinned dependencies and validate against released CCSDSPack 2.x + SpWKit |
| [HardRT](https://github.com/ExoSpaceLabs/hardrt) | **Active / stable** | v0.4.0 | Continue targeted RTOS/port validation and maintenance |
| [WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor) | **Active / stable** | v1.0.0 | Continue post-1.0 feature and operational hardening |

---

## CCSDSPack

**Role:** C++17 CCSDS Space Packet and ECSS PUS packet library. This is the protocol dependency that downstream SpWKit integration and EXN modernization should consume as a released package contract.

### Current state

- Latest public release: **v1.2.0**.
- `develop` declares **2.0.0** and contains the v2 standards/API refactor.
- Release-preparation PR **#128** was merged on 2026-08-26 after the v2 release-validation matrix passed.
- At this snapshot, `develop` is substantially ahead of `main`; v2 has not yet been promoted as the public release baseline.
- There are no open pull requests blocking the cut.
- The remaining uncertainty is concentrated in the release-gate checklist in issue **#87**, particularly items that require explicit completion, evidence, waiver, or deferral.

### Release position

The software line is at **release closure**, not feature development. New non-critical API or protocol work should not be added to the v2.0.0 gate unless it addresses a demonstrated release defect.

### Required actions

1. Audit every unchecked item in release tracker #87 and classify it as:
   - required and still open for v2.0.0;
   - already complete with evidence;
   - explicitly deferred to a later release.
2. Resolve hardware-validation requirements by executing them or documenting an explicit waiver/deferral. Ambiguous unchecked hardware gates should not remain after the release decision.
3. Reconcile/promote the validated v2 line to the release branch according to the repository release process.
4. Create and publish **v2.0.0** with release artifacts and package-consumer verification.
5. Use the published 2.0.0 package/API contract as the downstream integration baseline.

### Downstream impact

Publishing CCSDSPack 2.0.0 unblocks:

- SpWKit issue #87 for optional CCSDSPack 2.x integration;
- EXN modernization issue #2;
- EXN-GS migration issue #2.

---

## SpWKit

**Role:** Cross-platform SpaceWire integration toolkit with C11/C++17 APIs, SpaceWire packet/time-code support, RMAP, simulated transports, and hardware-facing SPI/UART/FTDI/FPGA backends.

### Current state

- Latest published GitHub release: **v0.4.0**.
- Repository/package version: **0.5.0**.
- Immutable **v0.5.0** tag already exists.
- The v0.5.0 release audit validated the major package/consumer paths, including:
  - install/package smoke tests;
  - C-only consumer;
  - shared-library consumer;
  - Windows package and DLL consumers;
  - macOS package consumer;
  - DEB consumers for amd64, arm64, armhf, and i386;
  - reproducible source archive.
- The blocking release job is the **multi-architecture OCI image build**. Because it failed, release evidence generation, GitHub Release/GHCR publication, and post-publish verification were skipped.
- `SPWKIT_ENABLE_CCSDSPACK` is optional and currently defaults to OFF, so recovery of v0.5.0 publication does not need to wait for CCSDSPack 2.0.0.

### Required actions

#### A. Recover v0.5.0 publication

1. Repair the multi-architecture OCI image build in the v0.5.0 release workflow.
2. Rerun the failed release jobs against the existing immutable v0.5.0 tag.
3. Publish the GitHub Release and OCI artifacts.
4. Complete post-publish verification.

Track this work in the existing v0.5.0 release issue **#90**.

#### B. Integrate CCSDSPack 2.x afterward

1. Wait for the public CCSDSPack 2.0.0 release/package contract.
2. Pin/support the released 2.x line rather than an unreleased development branch.
3. Reconcile the exported CMake target and integration API.
4. Enable CCSDSPack-backed SpWKit CI paths and package-consumer validation.
5. Add packet/PUS integration regressions across the dependency boundary.
6. Publish the compatibility change as a follow-up SpWKit release. Do **not** mutate the existing v0.5.0 tag.

Track this work in issue **#87**.

---

## EXN

**Role:** Modular satellite-avionics demonstration and integration platform connecting Raspberry Pi payload processing, STM32 control/flight software, FPGA acceleration, CCSDS/PUS interfaces, SpaceWire networking, and ground/HIL tooling.

### Current state

The architectural direction remains useful, but the repository is **not currently a reliable integrated release baseline**. Documentation and component assumptions were created against older packet/dependency contracts and need to be reconciled with the current CCSDSPack and SpWKit lines.

The EXN repository should therefore be treated as **under modernization**, not as a stable implementation release.

### Modernization sequence

Tracked in issue **#2**.

1. Publish CCSDSPack 2.0.0.
2. Establish the supported SpWKit + CCSDSPack 2.x compatibility baseline.
3. Document exact supported dependency versions and canonical CMake package targets.
4. Migrate EXN-GS and the MCU/Pi/FPGA component interfaces to the refreshed contracts.
5. Review the central ICD/interfaces against implemented component behavior.
6. Restore clean-checkout CI and end-to-end integration/HIL regression coverage.
7. Tag a coherent EXN baseline only after all components can be built and validated from documented released dependencies.

### Acceptance criteria for a new EXN baseline

- no developer-local absolute dependency paths;
- versioned dependency compatibility matrix;
- reproducible clean-clone builds;
- CCSDS/PUS interoperability tests across component boundaries;
- SpaceWire/RMAP integration tests against the supported SpWKit release;
- representative end-to-end HIL/simulation regression coverage;
- architecture and ICD documentation matching actual implementation contracts.

---

## EXN-GS

**Role:** C++17 EXN ground-control and HIL environment with daemon/TUI tooling, CCSDS/PUS command and telemetry handling, device simulation, and SpaceWire/RMAP integration.

### Current state

The current build dependency logic contains concrete portability debt:

- `cmake/deps.cmake` probes the developer-local path `/home/dev/Works/CCSDSPack`;
- its fallback tracks CCSDSPack `main` instead of a versioned released compatibility baseline;
- the expected CCSDSPack CMake target/API must be reconciled with the v2 exported package contract;
- supported SpWKit compatibility is not expressed as a clear versioned contract.

This makes EXN-GS the first practical downstream migration target once CCSDSPack 2.0.0 is public.

### Required actions

Tracked in issue **#2**.

1. Remove local absolute dependency paths.
2. Require/fetch released and supported CCSDSPack 2.x and SpWKit versions.
3. Adapt packet/PUS API usage to CCSDSPack 2.x.
4. Reconcile SpaceWire/RMAP integration with the supported SpWKit API.
5. Add clean-checkout package-consumer CI.
6. Run packet encode/decode, transport, subsystem-simulation, and fault-injection regressions.

---

## HardRT

**Role:** Small portable RTOS written in C, with static allocation, configurable scheduling, synchronization primitives, message queues, POSIX/Cortex-M ports, and an optional C++17 wrapper.

### Current state

- Current release: **v0.4.0**.
- The project has a clear standalone scope and does not sit on the CCSDSPack → SpWKit → EXN release critical path.
- It can continue independently while EXN decides how much of its MCU architecture should consume the current HardRT API.

### Priority

Maintain validation quality and keep EXN integration requirements separate from RTOS core scope unless they produce generally useful RTOS features.

---

## WorldSat Monitor

**Role:** Self-hosted satellite/constellation situational-awareness platform with public orbital-data ingestion, backend propagation, persistent object/group management, and an interactive 3D Earth interface.

### Current state

- Current release: **v1.0.0**.
- The project has a clear user-facing scope, documented source/image deployment, backend propagation, and an established service-oriented stack.
- It is independent of the CCSDSPack/SpWKit/EXN avionics dependency chain.

### Priority

Treat the v1.0.0 line as the stable baseline and continue normal post-release hardening/features without coupling it to the avionics-library release program.

---

## Recommended Execution Order

1. **CCSDSPack:** close v2 release gates and publish 2.0.0.
2. **SpWKit in parallel:** repair the v0.5.0 OCI release job and complete publication.
3. **SpWKit after CCSDSPack 2.0.0:** integrate released CCSDSPack 2.x and publish a follow-up compatibility release.
4. **EXN-GS:** migrate first as the concrete host-side consumer and restore clean package integration.
5. **EXN component repos:** migrate MCU, Pi, and FPGA interfaces/dependencies to the selected compatibility baseline.
6. **EXN:** validate end-to-end, reconcile ICD/documentation, and cut a coherent modernization baseline.

This order keeps release engineering, dependency migration, and system integration as separate gates instead of attempting to modernize the entire stack in one unbounded branch.
