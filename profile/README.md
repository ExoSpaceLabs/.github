# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio focuses on standards-based interfaces, practical hardware integration, reproducible builds, and software that can move from desktop simulation to representative embedded targets through stable engineering contracts.

## Project Portfolio

| Project | Lifecycle | Description |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)** | **2.0 release candidate** | C++17 library for CCSDS Space Packets and ECSS PUS TM/TC. Provides packet construction, serialization, bounded parsing, validation, secondary-header profiles, CUC time support, package integration, and standards-oriented conformance evidence. Public stable baseline: **v1.2.0**. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)** | **0.5 publication recovery** | Portable C11 SpaceWire toolkit with an optional C++17 wrapper, deterministic loopback/process-local simulation, distributed VSPW-TP/UDP links, Linux VSPD/CUSE virtual-device paths, hosted POSIX/Windows support, and no-heap embedded/RTOS integration contracts. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)** | **Modernization** | Modular satellite-avionics demonstration platform spanning Raspberry Pi payload processing, STM32 control software, FPGA acceleration, shared CCSDS/PUS interfaces, and ground/HIL tooling. The current integration baseline is being migrated to CCSDSPack 2.x; SpaceWire/SpWKit adoption is a separate planned integration decision. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)** | **Modernization** | C++17 ground-control and HIL environment with daemon, FTXUI terminal interface, command-line control, Serial/TCP links, CCSDS/PUS handling, and an STM32-oriented simulator. Its dependency setup is being migrated to a reproducible CCSDSPack 2.x package contract. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)** | **Active** | Small portable real-time operating system written in C, with static tasks, configurable scheduling, semaphores, mutexes, message queues, POSIX/Cortex-M ports, and an optional C++17 wrapper. Current release: **v0.4.0**. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)** | **Active** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, public orbital-data ingestion, and an interactive 3D Earth interface. Current release: **v1.0.0**. |

## Current Integration Priority

The current portfolio program branches from the CCSDSPack 2.0 release:

```text
CCSDSPack 2.0.0
├── SpWKit: prove optional CCSDSPack 2.x packet transport interoperability
└── EXN: migrate shared packet interfaces and consumers to CCSDSPack 2.x
    └── evaluate/adopt SpWKit separately for SpaceWire transport where required
```

SpWKit **v0.5.0 publication recovery** is independent of CCSDSPack 2.0 and can proceed in parallel.

1. **CCSDSPack:** close or explicitly defer the remaining v2 release-gate items and publish the validated 2.0.0 line.
2. **SpWKit:** recover publication of the already-created v0.5.0 tag. After CCSDSPack 2.0 is public, complete the separate installed-package interoperability work tracked for CCSDSPack 2.x.
3. **EXN:** migrate current CCSDS/PUS consumers and shared definitions to CCSDSPack 2.x, remove developer-local dependency assumptions, restore clean-checkout CI/HIL validation, and only claim SpaceWire/SpWKit integration once that transport is implemented and tested.

Detailed lifecycle and release state is maintained in **[Project Status](../docs/PROJECTSTATUS.md)**.

## Engineering Focus

- **Space communications:** CCSDS Space Packets, ECSS PUS, SpaceWire transport, command/telemetry handling, and protocol validation.
- **Embedded and real-time systems:** STM32/Cortex-M software, FPGA-facing interfaces, portable RTOS primitives, deterministic execution, and hardware-oriented integration boundaries.
- **Simulation and HIL:** host-side device simulation, fault injection, ground-segment tooling, distributed virtual links, and reproducible integration environments.
- **Mission observability:** satellite tracking, orbital propagation, telemetry visualization, state persistence, and operational dashboards.
- **Release engineering:** CMake packages, binary artifacts, multi-platform CI, external-consumer validation, documentation, and versioned compatibility contracts.

## Engineering Principles

Projects are expected to have a defined scope, documented public interfaces, automated validation, explicit compatibility/versioning, and a credible route to representative hardware or integration testing. Experimental work should be clearly identified and separated from supported release baselines.

## Contributing

Issues and pull requests are welcome where a repository is open for contribution. Please use each repository's documentation and issue tracker for project-specific requirements, compatibility constraints, and current priorities.

## Contact

- **GitHub:** [ExoSpaceLabs](https://github.com/ExoSpaceLabs)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/ExoSpaceLabs/discussions)
- **Email:** exospacelabs@gmail.com

Repository-level license files are authoritative for each project.
