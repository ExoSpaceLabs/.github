# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio focuses on standards-based interfaces, practical hardware integration, reproducible builds, and software that can move from desktop simulation to representative embedded targets through stable engineering contracts.

## Project Portfolio

| Project | Lifecycle | Description |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/CCSDSPack/main/docs/imgs/Logo.png" alt="CCSDSPack logo" height="64"> | **Stable / v2.0.0** | C++17 library for CCSDS Space Packets and ECSS PUS-A/PUS-C TM/TC. v2.0.0 is the current public baseline, with bounded parsing, structured validation, CUC time, installed-package support, hosted CI, native arm64 validation, and physical Cortex-M7 execution evidence. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/spwkit/main/img/SpWKit_logo.png" alt="SpWKit logo" height="48"> | **Stable v0.5.1 / v0.6 development** | Portable C11 SpaceWire toolkit with an optional C++17 wrapper, local and distributed virtual SpaceWire transports, Linux VSPD/CUSE device presentation, POSIX/Windows support, and embedded/no-heap integration contracts. v0.6 defines the reusable public hardware-driver/DMA boundary and CCSDSPack interoperability layer. Proprietary FPGA/RTL, DMA-engine internals, register maps, board integration, and hardware-specific driver implementation are intentionally kept outside the public repository; the planned private implementation repository is `spwkit-fpga`. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active integration modernization** | Architecture and interface authority for the modular satellite-avionics demonstrator. The central ICD, CCSDSPack configurations, JSON mirrors, MCU-facing headers, and interface CI are now aligned with the CCSDSPack v2.0.0 wire contract. Remaining modernization is concentrated in the flight/payload component repositories and end-to-end system validation. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active / CCSDSPack v2 migrated** | C++17 ground-control and HIL environment with transport daemon, FTXUI operator client, CLI tooling, Serial/TCP links, and STM32 simulation. The main branch now uses the CCSDSPack v2 router/client model, reproducible released dependencies, isolated dependency builds, transport-state hardening, simulation build tooling, and daemon/simulator HIL regression coverage. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/hardrt/main/docs/images/HardRT_logo.png" alt="HardRT logo" height="84"> | **Stable / v0.5.0** | Small portable real-time operating system written in C, with static tasks, configurable scheduling, semaphores, mutexes, message queues, event flags, per-task notifications, POSIX/Cortex-M ports, ISR-safe wake paths, and an optional C++17 wrapper. v0.5.0 adds the event/notification synchronization surface, scheduler/lifecycle hardening, deterministic hosted stress coverage, and expanded STM32H755 qualification evidence. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/world-sat-monitor/main/frontend/public/favicon.svg" alt="WorldSat Monitor logo" height="68"> | **Stable / v1.0.0** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, public orbital-data ingestion, and an interactive 3D Earth interface. Current release: **v1.0.0**. |

## Current Integration Priority

The packet-layer migration is complete at the central interface level. Current work is concentrated on hardware integration and EXN component reconciliation:

```text
CCSDSPack 2.0.0 [released]
├── SpWKit v0.6 [active development]
│   ├── public driver + DMA/zero-copy boundary implemented
│   ├── CCSDSPack packet-transport integration implemented
│   ├── spwkit-fpga [planned private FPGA implementation]
│   └── remaining: physical STM32 evidence, public FPGA boundary closure, release audit
├── HardRT 0.5.0 [released]
│   └── events/notifications + Cortex-M qualification baseline available
└── EXN [packet contract migrated]
    ├── EXN-GS [CCSDSPack v2 migration + HIL regression complete]
    ├── EXN-MCU / Pi / FPGA component reconciliation remaining
    └── SpaceWire/SpWKit adoption remains an explicit transport integration decision
```

1. **CCSDSPack:** maintain v2.0.0 as the stable packet/API contract for downstream projects.
2. **SpWKit:** maintain the v0.5.1 stable line while completing v0.6 hardware-integration evidence. The public project owns the portable driver contract, transport semantics, DMA ownership model, simulation, and interoperability tests; the planned private `spwkit-fpga` repository will own proprietary FPGA/RTL and hardware-specific implementation details.
3. **HardRT:** treat v0.5.0 as the current stable RTOS baseline. The release includes events, task notifications, hardened lifecycle/scheduler semantics, hosted stress validation, and expanded Cortex-M qualification.
4. **EXN:** the central packet/interface migration is complete. Continue by reconciling MCU, Pi, and FPGA components against the v2 contract, then restore a coherent cross-component simulation/HIL baseline.
5. **EXN-GS:** treat the current main branch as the migrated host-side integration baseline. It resolves CCSDSPack through released package semantics, currently pins HardRT v0.4.0 for its STM32 simulator, provides repeatable simulation builds, and exercises daemon/device lifecycle behavior through HIL smoke tests.

Detailed lifecycle and release state is maintained in **[Project Status](../docs/PROJECTSTATUS.md)**.

## Engineering Focus

- **Space communications:** CCSDS Space Packets, ECSS PUS, SpaceWire transport, command/telemetry handling, and protocol validation.
- **Embedded and real-time systems:** STM32/Cortex-M software, FPGA-facing interfaces, portable RTOS primitives, deterministic execution, and hardware-oriented integration boundaries.
- **Simulation and HIL:** host-side device simulation, fault injection, ground-segment tooling, distributed virtual links, and reproducible integration environments.
- **Mission observability:** satellite tracking, orbital propagation, telemetry visualization, state persistence, and operational dashboards.
- **Release engineering:** CMake packages, binary artifacts, multi-platform CI, external-consumer validation, documentation, and versioned compatibility contracts.

## Engineering Principles

Projects are expected to have a defined scope, documented public interfaces, automated validation, explicit compatibility/versioning, and a credible route to representative hardware or integration testing. Experimental work should be clearly identified and separated from supported release baselines. Public reusable contracts should remain independent from proprietary implementation details where that separation improves portability, licensing clarity, and integration discipline.

## Contributing

Issues and pull requests are welcome where a repository is open for contribution. Please use each repository's documentation and issue tracker for project-specific requirements, compatibility constraints, and current priorities.

## Contact

- **GitHub:** [ExoSpaceLabs](https://github.com/ExoSpaceLabs)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/ExoSpaceLabs/discussions)
- **Email:** exospacelabs@gmail.com

Repository-level license files are authoritative for each project.
