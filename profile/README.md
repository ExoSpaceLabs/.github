# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio focuses on standards-based interfaces, practical hardware integration, reproducible builds, and software that can move from desktop simulation to representative embedded targets without changing its engineering contract every other Tuesday.

## Project Portfolio

| Project | Lifecycle | Description |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)** | **2.0 release candidate** | C++17 library for CCSDS Space Packets and ECSS PUS TM/TC. Provides packet construction, serialization, bounded parsing, validation, mission profiles, package integration, and standards-oriented conformance evidence. Public stable baseline: **v1.2.0**. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)** | **0.5 publication recovery** | Cross-platform C11/C++17 toolkit for SpaceWire development and integration, including packet/time-code handling, RMAP, simulation transports, Linux `spidev`, FTDI/FPGA bridges, UART/SPI backends, and a C ABI. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)** | **Modernization** | Modular satellite-avionics demonstration platform spanning Raspberry Pi payload processing, STM32 flight/control software, FPGA acceleration, SpaceWire networking, CCSDS/PUS interfaces, and ground/HIL tooling. The integration baseline is being migrated to CCSDSPack 2.x and current SpWKit. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)** | **Modernization** | C++17 ground-control and hardware-in-the-loop environment for EXN, with CCSDS/PUS command and telemetry handling, SpaceWire/RMAP integration, terminal tooling, subsystem simulation, and fault-injection workflows. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)** | **Active** | Small portable real-time operating system written in C, with static tasks, configurable scheduling, semaphores, mutexes, message queues, POSIX/Cortex-M ports, and an optional C++17 wrapper. Current release: **v0.4.0**. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)** | **Active** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, public orbital-data ingestion, and an interactive 3D Earth interface. Current release: **v1.0.0**. |

## Current Integration Priority

The main portfolio dependency chain is:

`CCSDSPack 2.0.0` → `SpWKit CCSDSPack 2.x integration` → `EXN / EXN-GS modernization`

1. **CCSDSPack:** close or explicitly defer the remaining v2 release-gate items and publish the validated 2.0.0 line.
2. **SpWKit:** recover publication of the already-created v0.5.0 tag, then migrate its optional CCSDSPack integration to the released 2.x contract in a follow-up release.
3. **EXN:** remove stale and developer-local dependency assumptions, migrate all components to documented released package contracts, restore clean-checkout CI, and re-establish end-to-end HIL/integration regression coverage.

Detailed lifecycle and release state is maintained in **[Project Status](../docs/PROJECTSTATUS.md)**.

## Engineering Focus

- **Space communications:** CCSDS Space Packets, ECSS PUS, SpaceWire, RMAP, command/telemetry transport, and protocol validation.
- **Embedded and real-time systems:** STM32/Cortex-M software, FPGA-facing interfaces, portable RTOS primitives, deterministic execution, and hardware-oriented transport layers.
- **Simulation and HIL:** host-side device simulation, fault injection, ground-segment tooling, and reproducible integration environments.
- **Mission observability:** satellite tracking, orbital propagation, telemetry visualization, state persistence, and operational dashboards.
- **Release engineering:** CMake packages, binary artifacts, multi-platform CI, external-consumer validation, documentation, and versioned compatibility contracts.

## Engineering Principles

Projects are expected to have a defined scope, documented public interfaces, automated validation, explicit compatibility/versioning, and a credible route to representative hardware or integration testing. Experimental work is welcome, but experimental state should be labeled as such rather than promoted through the ancient engineering ritual of hoping nobody notices.

## Contributing

Issues and pull requests are welcome where a repository is open for contribution. Please use each repository's documentation and issue tracker for project-specific requirements, compatibility constraints, and current priorities.

## Contact

- **GitHub:** [ExoSpaceLabs](https://github.com/ExoSpaceLabs)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/ExoSpaceLabs/discussions)
- **Email:** exospacelabs@gmail.com

Repository-level license files are authoritative for each project.
