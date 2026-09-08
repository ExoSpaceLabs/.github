# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio focuses on standards-based interfaces, practical hardware integration, reproducible builds, and software that can move from desktop simulation to representative embedded targets through stable engineering contracts.

## Project Portfolio

| Project | Lifecycle | Description |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/CCSDSPack/main/docs/imgs/Logo.png" alt="CCSDSPack logo" height="64"> | **Stable / v2.0.0** | C++17 library for CCSDS Space Packets and ECSS PUS-A/PUS-C TM/TC. v2.0.0 is the current public packet/API baseline, with bounded parsing, structured validation, CUC time, installed-package support, hosted CI, native arm64 validation, and physical Cortex-M7 execution evidence. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/spwkit/main/img/SpWKit_logo.png" alt="SpWKit logo" height="48"> | **Stable / v0.6.0** | Portable C11 SpaceWire development and integration toolkit with an optional C++17 wrapper. v0.6.0 adds the stable public hardware-driver boundary, DMA/zero-copy ownership integration, physical STM32H755 DMA/cache qualification, CCSDSPack interoperability, virtual/distributed backends, Linux virtual-device tooling, and multi-architecture packages. Active development has moved to reproducible performance characterization of the software abstraction and provider boundaries. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active integration modernization** | Architecture and interface authority for the modular satellite-avionics demonstrator. The central ICD, CCSDSPack configurations, JSON mirrors, MCU-facing headers, and interface CI are aligned with the CCSDSPack v2.0.0 wire contract. Remaining modernization is concentrated in flight/payload components and end-to-end system validation. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active / CCSDSPack v2 migrated** | C++17 ground-control and HIL environment with transport daemon, FTXUI operator client, CLI tooling, Serial/TCP links, and STM32 simulation. The current baseline uses the CCSDSPack v2 router/client model, reproducible released dependencies, isolated dependency builds, transport-state hardening, simulation tooling, and daemon/simulator HIL regression coverage. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/hardrt/main/docs/images/HardRT_logo.png" alt="HardRT logo" height="84"> | **Stable / v0.5.1** | Small portable real-time operating system written in C, with static tasks, fixed-priority and round-robin scheduling, semaphores, mutexes, queues, event flags, task notifications, POSIX/Cortex-M ports, ISR-safe synchronization paths, and an optional C++17 wrapper. v0.5.1 corrects hosted POSIX execution with scheduler-controlled pthread tasks and asynchronous preemption while retaining the v0.5 public API and Cortex-M scheduling contract. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/world-sat-monitor/main/frontend/public/favicon.svg" alt="WorldSat Monitor logo" height="68"> | **Stable / v1.0.0** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, public orbital-data ingestion, and an interactive 3D Earth interface. |

## Current Integration Priority

The foundational packet, real-time, and SpaceWire software layers now have released baselines. Current work is shifting toward characterization and EXN system integration:

```text
CCSDSPack 2.0.0 [released]
├── SpWKit 0.6.0 [released]
│   ├── public hardware-driver + DMA/zero-copy boundary
│   ├── STM32H755 DMA/cache qualification
│   ├── CCSDSPack packet interoperability
│   └── active: software/provider performance characterization
├── HardRT 0.5.1 [released]
│   ├── event flags + task notifications
│   ├── Cortex-M qualification baseline
│   └── corrected preemptive hosted POSIX execution
└── EXN [packet contract migrated]
    ├── EXN-GS [CCSDSPack v2 migration + HIL regression complete]
    ├── EXN-MCU / Pi / FPGA component reconciliation remaining
    └── system transport and end-to-end HIL integration remaining
```

1. **CCSDSPack:** maintain v2.0.0 as the stable packet/API contract for downstream projects.
2. **SpWKit:** treat v0.6.0 as the stable hardware-integration software boundary. Current development measures abstraction, provider, copied/zero-copy, and STM32 software costs without conflating them with controller/PHY performance.
3. **HardRT:** treat v0.5.1 as the current stable RTOS baseline. The hosted POSIX execution model is now genuinely preemptive for CPU-bound tasks while the common core remains authoritative for scheduling policy.
4. **EXN:** reconcile MCU, Pi, and FPGA components against the central v2 contract and released foundational libraries, then restore coherent cross-component simulation/HIL.
5. **EXN-GS:** maintain the migrated host-side integration baseline and evolve transport/HIL behavior as EXN component interfaces stabilize.

Detailed lifecycle and release state is maintained in **[Project Status](../docs/PROJECTSTATUS.md)**.

## Engineering Focus

- **Space communications:** CCSDS Space Packets, ECSS PUS, SpaceWire transport, command/telemetry handling, and protocol validation.
- **Embedded and real-time systems:** STM32/Cortex-M software, FPGA-facing interfaces, portable RTOS primitives, deterministic execution, and hardware-oriented integration boundaries.
- **Simulation and HIL:** host-side device simulation, fault injection, ground-segment tooling, distributed virtual links, and reproducible integration environments.
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
