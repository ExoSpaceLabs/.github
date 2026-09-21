# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio focuses on standards-based interfaces, practical hardware integration, reproducible builds, and software that can move from desktop simulation to representative embedded targets through stable engineering contracts.

## Project Portfolio

| Project | Lifecycle | Description |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/CCSDSPack/main/docs/imgs/Logo.png" alt="CCSDSPack logo" height="64"> | **Stable / v2.0.0** | C++17 library for CCSDS Space Packets and ECSS PUS-A/PUS-C TM/TC. v2.0.0 is the current packet/API baseline, with bounded parsing, structured validation, CUC time, installed-package support, hosted CI, native arm64 validation, and physical Cortex-M7 execution evidence. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/spwkit/main/img/SpWKit_logo.png" alt="SpWKit logo" height="48"> | **Stable / v0.7.0** | Portable C11 SpaceWire development and integration toolkit with an optional C++17 wrapper. v0.7.0 hardens the software-visible backend contract with explicit threading, lifecycle, timeout, error, resource, reset, and zero-copy ownership semantics; executable backend-equivalence evidence; scoped ECSS-E-ST-50-12C Rev.1 traceability; STM32H755 DMA/cache qualification; and controlled performance-regression evidence. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/hardrt/main/docs/images/HardRT_logo.png" alt="HardRT logo" height="84"> | **Stable / v0.5.1** | Small portable real-time operating system written in C, with static tasks, fixed-priority and round-robin scheduling, semaphores, mutexes, queues, event flags, task notifications, POSIX/Cortex-M ports, ISR-safe synchronization paths, and an optional C++17 wrapper. v0.5.1 provides scheduler-controlled pthread execution and asynchronous hosted preemption while preserving the v0.5 public API and Cortex-M scheduling contract. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active architecture / integration** | Modular satellite-avionics demonstrator and interface authority. The central packet contract is aligned with CCSDSPack v2.0.0, while the architecture now defines Linux payload services, communications routing, configurable FPGA-result destinations, and an OBC-supervised model that permits direct payload-data paths when appropriate. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active / CCSDSPack v2 migrated** | C++17 ground-control and HIL environment with transport daemon, FTXUI operator client, CLI tooling, Serial/TCP links, STM32 simulation, reproducible released dependencies, transport-state hardening, and daemon/simulator HIL regression coverage. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/world-sat-monitor/main/frontend/public/favicon.svg" alt="WorldSat Monitor logo" height="68"> | **Stable / v1.0.0** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, orbital-data ingestion, and an interactive 3D Earth interface. |

## Current Integration Priority

The packet, RTOS, and SpaceWire software foundations now have stable released baselines. Current work is moving toward pre-1.0 contract hardening and system integration:

```text
CCSDSPack 2.0.0 [released]
├── SpWKit 0.7.0 [released]
│   ├── driver/DMA software boundary [stable]
│   ├── backend behavioral-equivalence evidence
│   ├── scoped ECSS-E-ST-50-12C software traceability
│   ├── performance-regression evidence
│   └── next: v0.8 API/ABI + ECSS architecture phase
├── HardRT 0.5.1 [released]
│   ├── event flags + task notifications
│   ├── Cortex-M physical qualification
│   └── corrected preemptive hosted POSIX execution
└── EXN [architecture + packet contract active]
    ├── dynamic payload/communications routing defined
    ├── EXN-GS migrated to CCSDSPack v2
    ├── MCU / Pi / FPGA implementations still require reconciliation
    └── end-to-end system HIL remains the major integration target
```

1. **CCSDSPack:** maintain v2.0.0 as the stable packet/API contract for downstream projects.
2. **SpWKit:** treat v0.7.0 as the current stable software-visible backend contract. The v0.8 phase is reserved for any remaining intentional API/ABI cleanup plus ECSS-E-ST-40 engineering-process traceability, ECSS-Q-ST-80 assurance work, and disposition of deferred software-visible SpaceWire capabilities before the v0.9 freeze.
3. **HardRT:** maintain v0.5.1 as the stable RTOS baseline while release-finalization tooling and longer-term timing/qualification work continue on the development line.
4. **EXN:** use the now-defined routing/service architecture to drive concrete MCU, Linux payload, camera, communications, and FPGA implementations, then re-establish coherent cross-component simulation/HIL.
5. **EXN-GS:** maintain the migrated host-side baseline and evolve transport/HIL integration only against explicit component/interface revisions.

Detailed lifecycle and release state is maintained in **[Project Status](../docs/PROJECTSTATUS.md)**.

## Engineering Focus

- **Space communications:** CCSDS Space Packets, ECSS PUS, SpaceWire transport, command/telemetry handling, backend equivalence, and protocol validation.
- **Embedded and real-time systems:** STM32/Cortex-M software, FPGA-facing interfaces, portable RTOS primitives, deterministic execution, and hardware-oriented integration boundaries.
- **Simulation and HIL:** host-side device simulation, fault injection, ground-segment tooling, distributed virtual links, and reproducible integration environments.
- **Standards and assurance:** scoped ECSS conformance, requirements-to-test traceability, compatibility contracts, and reproducible release evidence.
- **Performance characterization:** reproducible cycle/counter measurements that separate software abstraction, provider/controller, and end-to-end physical-link domains.
- **Mission observability:** satellite tracking, orbital propagation, telemetry visualization, state persistence, and operational dashboards.
- **Release engineering:** CMake packages, binary artifacts, multi-platform CI, external-consumer validation, documentation, qualification evidence, and versioned compatibility contracts.

## Engineering Principles

Projects are expected to have a defined scope, documented public interfaces, automated validation, explicit compatibility/versioning, and a credible route to representative hardware or integration testing. Experimental work should be clearly identified and separated from supported release baselines. Public APIs should expose implementation-independent contracts rather than implementation-specific hardware internals.

## Contributing

Issues and pull requests are welcome where a repository is open for contribution. Please use each repository's documentation and issue tracker for project-specific requirements, compatibility constraints, and current priorities.

## Contact

- **GitHub:** [ExoSpaceLabs](https://github.com/ExoSpaceLabs)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/ExoSpaceLabs/discussions)
- **Email:** exospacelabs@gmail.com

Repository-level license files are authoritative for each project.
