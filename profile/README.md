# ExoSpaceLabs

![ExoSpaceLabs Logo](../imgs/ExoSpaceLabs-Logo.png)

**Expanding with Space**

ExoSpaceLabs develops open-source engineering infrastructure for spacecraft communications, embedded avionics, real-time execution, simulation, and mission observability. The portfolio emphasizes standards-based interfaces, explicit software/hardware ownership boundaries, reproducible builds, representative hardware validation, and reusable contracts that survive the move from desktop simulation to embedded targets.

## Project Portfolio

| Project | Lifecycle | Current position |
| --- | --- | --- |
| **[CCSDSPack](https://github.com/ExoSpaceLabs/CCSDSPack)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/CCSDSPack/main/docs/imgs/Logo.png" alt="CCSDSPack logo" height="64"> | **Stable v2.0.0 / v2.1 release candidate** | CCSDS Space Packet and ECSS PUS library. The v2.1 line adds an authoritative C11 protocol core beneath the compatible C++17 API. Feature migration is complete and the candidate has passed Raspberry Pi 5/native arm64 and NUCLEO-H755ZI-Q physical acceptance; final promotion/tagging remains under release control. |
| **[SpWKit](https://github.com/ExoSpaceLabs/spwkit)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/spwkit/main/img/SpWKit_logo.png" alt="SpWKit logo" height="48"> | **Stable v0.7.0 / pre-v1 hardening** | Portable C11 SpaceWire development and integration toolkit with simulation, distributed transports, Linux virtual devices, and a stable public hardware-driver boundary. Physical RAW-Ethernet functionality passes across PC, Raspberry Pi 5, and STM32H755; current work is qualifying end-to-end performance and investigating ACK/retry-driven anomalies before the v0.8 readiness freeze. |
| **[HardRT](https://github.com/ExoSpaceLabs/hardrt)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/hardrt/main/docs/images/HardRT_logo.png" alt="HardRT logo" height="84"> | **Stable v0.5.1 / v0.6 release preparation** | Small static-allocation RTOS for deterministic embedded execution. The v0.6 development line implements bounded IPC timeouts, bounded mutex priority inheritance, and stable-phase periodic timing; the full STM32H755 functional/benchmark campaign has passed on the qualified baseline. Remaining v0.6 work is release audit, qualification-equivalence control, and final release closure rather than missing core features. |
| **[EXN](https://github.com/ExoSpaceLabs/exn)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active architecture / integration** | Modular satellite-avionics demonstrator and inter-device interface authority. The architecture is now PUS-C based, prioritizes the OBC ↔ Pi communications path, keeps FPGA USB deployment optional, reserves CM4 for asynchronous internal work, and deliberately keeps component-internal contracts inside their owning repositories. |
| **[EXN-GS](https://github.com/ExoSpaceLabs/exn-gs)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/exn/main/docs/images/exn_logo_transparent.png" alt="EXN logo" height="76"> | **Active / PUS-C migrated** | C++17 ground-control and HIL environment with transport daemon, FTXUI operator client, CLI tooling, Serial/TCP links, STM32 simulation, PUS-C Service 1 verification, System HK support, and central-ICD-driven mission semantics. |
| **[WorldSat Monitor](https://github.com/ExoSpaceLabs/world-sat-monitor)**<br><img src="https://raw.githubusercontent.com/ExoSpaceLabs/world-sat-monitor/main/frontend/public/favicon.svg" alt="WorldSat Monitor logo" height="68"> | **Stable v1.1.0** | Self-hosted satellite and constellation situational-awareness platform with backend SGP4 propagation, persistent object/group management, interactive 3D Earth visualization, provider-health reporting, orbital-source provenance, bounded CelesTrak retry/backoff, and deterministic degraded fallback behavior. |

## Current Engineering Position

The reusable foundations are no longer the main portfolio blocker. The current work is mostly release qualification, performance characterization, and system integration rather than inventing yet another abstraction layer because apparently software engineers have suffered enough.

```text
CCSDSPack
├── v2.0.0 stable release
└── v2.1 C11-core candidate
    ├── protocol/API implementation complete
    ├── Pi 5 + STM32H755 physical acceptance passed
    └── release promotion/tagging pending

HardRT
├── v0.5.1 stable release
└── v0.6 development line
    ├── bounded IPC timeouts implemented
    ├── bounded priority inheritance implemented
    ├── stable-phase periodic timing implemented
    ├── full H755 campaign passed on qualified baseline
    └── release audit / final qualification control pending

SpWKit
├── v0.7.0 stable release
├── software/backend contract established
├── PC ↔ Pi ↔ STM32 physical functionality passed
└── end-to-end performance + ACK/retry investigation active

EXN
├── PUS-C inter-device architecture established
├── OBC ↔ Pi communications path = priority 0
├── FPGA link remains modular / deployment-optional
└── component implementation + end-to-end HIL = main system work
```

## Engineering Focus

- **Space communications:** CCSDS Space Packets, ECSS PUS-C, SpaceWire transport, protocol validation, and explicit interoperability contracts.
- **Embedded and real-time systems:** STM32/Cortex-M software, deterministic scheduling, bounded synchronization, DMA/cache ownership, and hardware qualification.
- **FPGA and hardware integration:** reusable software-facing provider contracts separated from implementation-specific controller/RTL/PHY work.
- **Simulation and HIL:** host simulation, virtual/distributed links, device emulation, fault injection, and staged physical integration.
- **Standards and assurance:** scoped ECSS conformance, requirements-to-test traceability, compatibility discipline, and reproducible qualification evidence.
- **Performance characterization:** measurement before optimization, with software, provider/controller, and physical-link effects kept distinct.
- **Mission observability:** satellite tracking, propagation, orbital-source health, state persistence, and operational visualization.
- **Release engineering:** versioned packages, multi-platform CI, external-consumer tests, hardware evidence, and controlled release promotion.

Detailed lifecycle and release state is maintained in **[Project Status](../docs/PROJECTSTATUS.md)**.

## Engineering Principles

Projects are expected to have a defined scope, documented interfaces, automated validation, explicit versioning/compatibility policy, and a credible route to representative hardware. Public APIs should expose implementation-independent contracts; hardware internals and component-local contracts should remain where they belong. Claims follow evidence.

## Contributing

Issues and pull requests are welcome where a repository is open for contribution. Use each repository's documentation and issue tracker for project-specific requirements, compatibility constraints, and current priorities.

## Contact

- **GitHub:** [ExoSpaceLabs](https://github.com/ExoSpaceLabs)
- **Discussions:** [GitHub Discussions](https://github.com/orgs/ExoSpaceLabs/discussions)
- **Email:** exospacelabs@gmail.com

Repository-level license files are authoritative for each project.
