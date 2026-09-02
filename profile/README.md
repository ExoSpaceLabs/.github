# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio focuses on standards-based interfaces, practical hardware integration, reproducible builds, and software that can move from desktop simulation to representative embedded targets through stable engineering contracts.

## Project Portfolio

| Project | Lifecycle | Description |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)** | **Stable / v2.0.0** | C++17 library for CCSDS Space Packets and ECSS PUS-A/PUS-C TM/TC. v2.0.0 is the current public baseline, with bounded parsing, structured validation, CUC time, installed-package support, hosted CI, native arm64 validation, and physical Cortex-M7 execution evidence. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)** | **Stable v0.5.1 / v0.6 development** | Portable C11 SpaceWire toolkit with an optional C++17 wrapper, local and distributed virtual SpaceWire transports, Linux VSPD/CUSE device presentation, POSIX/Windows support, and embedded/no-heap integration contracts. v0.5.1 is the current maintenance release; v0.6 adds the public hardware-driver/DMA boundary, reference-driver validation, CCSDSPack interoperability, and the path toward hardware-backed SpaceWire without claiming a physical SpaceWire implementation. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)** | **Active integration modernization** | Architecture and interface authority for the modular satellite-avionics demonstrator. The central ICD, CCSDSPack configurations, JSON mirrors, MCU-facing headers, and interface CI are now aligned with the CCSDSPack v2.0.0 wire contract. Remaining modernization is concentrated in the flight/payload component repositories and end-to-end system validation. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)** | **Active / CCSDSPack v2 migrated** | C++17 ground-control and HIL environment with transport daemon, FTXUI operator client, CLI tooling, Serial/TCP links, and STM32 simulation. The main branch now uses the CCSDSPack v2 router/client model, reproducible released dependencies, isolated dependency builds, transport-state hardening, simulation build tooling, and daemon/simulator HIL regression coverage. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)** | **Active** | Small portable real-time operating system written in C, with static tasks, configurable scheduling, semaphores, mutexes, message queues, POSIX/Cortex-M ports, and an optional C++17 wrapper. Current release: **v0.4.0**. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)** | **Active** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, public orbital-data ingestion, and an interactive 3D Earth interface. Current release: **v1.0.0**. |

## Current Integration Priority

The packet-layer migration has moved from planning into downstream integration:

```text
CCSDSPack 2.0.0 [released]
├── SpWKit v0.6 [active development]
│   ├── public driver + DMA/zero-copy boundary implemented
│   ├── CCSDSPack packet-transport integration implemented
│   └── remaining: STM32 runtime evidence, public FPGA boundary, release audit
└── EXN [packet contract migrated]
    ├── EXN-GS [CCSDSPack v2 migration + HIL regression complete]
    ├── EXN-MCU / Pi / FPGA component reconciliation remaining
    └── SpaceWire/SpWKit adoption remains an explicit transport integration decision
```

1. **CCSDSPack:** maintain v2.0.0 as the stable packet/API contract for downstream projects.
2. **SpWKit:** maintain the v0.5.1 stable line while completing v0.6 hardware-integration evidence. The current v0.6 software boundary already includes the driver backend, DMA ownership mapping, reference driver, distributed transport, and CCSDS/PUS interoperability; physical SpaceWire/FPGA interoperability remains deliberately outside current claims.
3. **EXN:** the central packet/interface migration is complete. Continue by reconciling MCU, Pi, and FPGA components against the v2 contract, then restore a coherent cross-component simulation/HIL baseline.
4. **EXN-GS:** treat the current main branch as the migrated host-side integration baseline. It resolves CCSDSPack through released package semantics, pins HardRT v0.4.0, provides repeatable simulation builds, and exercises daemon/device lifecycle behavior through HIL smoke tests.

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
