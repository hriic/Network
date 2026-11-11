# Network

A structured collection of my Computer Networks labs, Cisco Packet Tracer simulations, configuration notes, and supporting study material.

## Repository overview

This repository documents practical networking exercises in a reproducible format. Each lab is organized in its own folder and includes a detailed README, relevant screenshots, and simulation files when available.

## Lab projects

### Task 1 — Basic Switch and End Device Configuration

**Focus:** Cisco Catalyst 2960, Layer 2 switching, static IPv4 addressing, management SVI, basic switch access configuration, IOS verification commands, and ping testing.

- [Open the complete Task 1 documentation](./Task-1-Basic-Switch-and-End-Device-Configuration/README.md)
- Packet Tracer simulation file: `Task1_Basic_Switch_Configuration.pkt` (supplied separately; repository upload is pending).
- [View the topology screenshot](./Task-1-Basic-Switch-and-End-Device-Configuration/screenshots/topology.png)

The lab uses two Catalyst 2960 switches and two PCs connected in a single /24 LAN. Its documentation includes the port map, IP addressing table, switch configuration commands, verification workflow, test interpretation, interface-status reference, reflection answers, and troubleshooting notes.

## Study notes

- [IP Address Assignment & DHCP Summary](./Ip%20Address%20Assignment%20%26%20DHCP_Summary.png)

## Suggested structure for future labs

New exercises should be added as separate folders using a consistent structure:

```text
Lab-Name/
├── README.md
├── project-file.pkt
└── screenshots/
    ├── topology.png
    └── verification.png
```

This keeps each exercise easy to navigate, review, reproduce, and extend.
