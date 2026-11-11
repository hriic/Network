# Task 1 — Basic Switch and End Device Configuration

> **Course:** Computer Networks (0790422)  
> **University:** Philadelphia University  
> **Semester:** First Semester 2026/2027  
> **Lab:** Chapter 4 — Basic Switch Configuration  
> **Platform:** Cisco Packet Tracer (Cisco Catalyst 2960)

## 1. Project overview

This lab builds and configures a small LAN with two Cisco Catalyst 2960 switches and two end devices. The work covers physical/logical topology setup, static IPv4 addressing, basic switch hardening, management SVIs, configuration backups, device inspection, and connectivity checks.

## 2. Topology

![Packet Tracer topology](screenshots/topology.png)

The logical device mapping used in the Packet Tracer workspace is:

- **Switch0 = S1**
- **Switch1 = S2**
- **PC0 = PC-A**
- **PC1 = PC-B**

### Cable and port map

| Device A | Port | Device B | Port |
|---|---|---|---|
| S1 | FastEthernet0/1 | S2 | FastEthernet0/1 |
| PC-A | FastEthernet0 | S1 | FastEthernet0/6 |
| S2 | FastEthernet0/18 | PC-B | FastEthernet0 |

Use copper straight-through cables for PC-to-switch links and a copper crossover cable for the switch-to-switch link, as specified by the lab topology.

## 3. IPv4 addressing plan

All four interfaces use the same /24 subnet.

| Device | Interface | IPv4 address | Subnet mask |
|---|---|---|---|
| S1 | VLAN 1 (management SVI) | 192.168.1.1 | 255.255.255.0 |
| S2 | VLAN 1 (management SVI) | 192.168.1.2 | 255.255.255.0 |
| PC-A | NIC | 192.168.1.10 | 255.255.255.0 |
| PC-B | NIC | 192.168.1.11 | 255.255.255.0 |

The lab's addressing table does not specify a default gateway; none is required for communication within this single subnet.

## 4. PC host configuration

For each PC, open **Desktop → IP Configuration**, select **Static**, and enter the matching address and subnet mask from the table above.

## 5. Switch configuration

The commands below show the intended baseline configuration. Run the S1 block on S1 and the S2 block on S2.

### S1

```text
enable
configure terminal
hostname S1
no ip domain-lookup
enable secret class
line console 0
 password cisco
 login
exit
interface vlan 1
 ip address 192.168.1.1 255.255.255.0
 no shutdown
exit
banner motd #WARNING: Unauthorized access is prohibited!#
end
copy running-config startup-config
```

### S2

```text
enable
configure terminal
hostname S2
no ip domain-lookup
enable secret class
line console 0
 password cisco
 login
exit
interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
exit
banner motd #WARNING: Unauthorized access is prohibited!#
end
copy running-config startup-config
```

> **Lab credential note:** `class` and `cisco` are the simple credentials specified by the student lab. They are for this Packet Tracer exercise only; do not reuse them on real systems.

## 6. Verification commands

Run these commands from privileged EXEC mode (`S1#` or `S2#`):

| Command | Purpose |
|---|---|
| `show running-config` | Display the active configuration |
| `show version` | Display IOS version and device information |
| `show ip interface brief` | Summarize interface IP addresses and line/protocol state |

### Connectivity checks

From a PC command prompt:

```text
ping 192.168.1.1
ping 192.168.1.2
```

From a switch CLI:

```text
ping 192.168.1.10
ping 192.168.1.11
```

An additional end-to-end test between the PCs is:

```text
PC-A> ping 192.168.1.11
```

## 7. Evidence captured during the lab

### S1 configuration and save confirmation

![S1 CLI configuration and save output](screenshots/s1-config-and-save.png)

The captured CLI output shows the MOTD banner command, the `copy running-config startup-config` operation completing with `[OK]`, and portions of S1's running configuration including the hostname and disabled DNS lookup.

### IOS/device information

The Packet Tracer switch information shown during startup identified the device as a Catalyst 2960-24TT running IOS 15.0(2)SE4.

### Initial PC ping test

The captured first ping run from PC-A showed **3 replies out of 4 packets (25% loss)** for each switch management address. The first request timed out and the remaining three received replies. This is recorded as an initial test, not as a zero-loss final result. Re-run both pings after the first attempt and record the final output before reporting complete connectivity.

## 8. Interface status table

The expected link states based on the cabling plan are shown below. Verify the actual values on both switches with `show ip interface brief` before copying them into the submitted worksheet.

| Interface | S1 Status | S1 Protocol | S2 Status | S2 Protocol |
|---|---|---|---|---|
| Fa0/1 | up | up | up | up |
| Fa0/6 | up | up | down | down |
| Fa0/18 | down | down | up | up |
| VLAN 1 | up* | up* | up* | up* |

* VLAN 1 should be up/up when the SVI is enabled and at least one Layer 2 port in VLAN 1 is active. Confirm the observed values rather than assuming them.

## 9. Reflection questions

**1. Why are some FastEthernet ports on the switches up and others down?**

Some FastEthernet ports are up because they are connected to active devices. Other ports are down because they are not connected to any active device.

**2. What could prevent a ping from being sent between the PCs?**

Incorrect IP addresses or subnet masks, disconnected or incorrectly connected cables, disabled interfaces, different VLAN configurations, or a host firewall blocking ICMP echo traffic could prevent successful communication between the PCs.

## 10. Submission files

The repository currently contains this lab documentation and the screenshots captured in the conversation. The final Packet Tracer `.pkt` file and the completed student worksheet `.docx` were not attached here, so they are **not included yet**. Add those two files when they are ready for a complete hand-in archive.
