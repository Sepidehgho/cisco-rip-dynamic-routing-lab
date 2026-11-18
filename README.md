
# Cisco Dynamic Routing Lab – RIPv2 & Loopback Interfaces

## 📌 Project Overview

This project demonstrates the implementation of **Dynamic Routing using RIPv2 (Routing Information Protocol version 2)** in Cisco Packet Tracer.

The network consists of five interconnected Cisco routers, multiple IPv4 networks, and loopback interfaces.

The main goal of this lab is to understand how routers dynamically exchange routing information, learn remote networks, and select paths to reach different destinations.

The topology also includes an alternative path, making it suitable for practicing routing redundancy and troubleshooting.

## 🖥️ Network Topology

The network topology was designed and simulated using **Cisco Packet Tracer**.

### Network Components

- 5 Cisco routers
- Multiple Gigabit Ethernet and Fast Ethernet connections
- IPv4 addressing
- Loopback interfaces
- Multiple routes between network segments
- RIPv2 dynamic routing

### Network Structure

The routers are connected as follows:

```text
             R1 -------- R2 -------- R3 -------- R4
              \                                   /
               \                                 /
                \------------- R5 --------------/

```

The topology provides two potential paths between R1 and R4:

**Primary path:**
R1 → R2 → R3 → R4

**Alternative path:**
R1 → R5 → R4

These paths can be used to demonstrate how routing changes when a network connection becomes unavailable.

### IP Addressing

| Connection | Network |
|---|---|
| R1 – R2 | 10.10.12.0/24 |
| R2 – R3 | 10.10.23.0/24 |
| R3 – R4 | 10.10.34.0/24 |
| R1 – R5 | 10.11.15.0/24 |
| R5 – R4 | 10.11.45.0/24 |

### Loopback Networks

| Router | Loopback Network |
|---|---|
| R1 | 172.16.16.0/24 |
| R1 | 172.16.17.0/24 |
| R4 | 172.17.17.0/24 |
| R4 | 172.17.18.0/24 |

These loopback networks simulate additional networks without requiring physical switches or end devices.

## 🔧 Technologies Used

- Cisco Packet Tracer
- Cisco IOS CLI
- RIPv2
- Dynamic Routing
- Distance Vector Routing
- Loopback Interfaces
- IPv4 Addressing
- Routing Tables
- Network Troubleshooting

## 🌐 What Is RIP?

RIP (Routing Information Protocol) is a **Distance Vector Dynamic Routing Protocol**.

RIP allows routers to automatically exchange routing information with neighboring routers.

Instead of manually configuring static routes for every destination, routers learn remote networks through routing updates.

### Important RIP Characteristics

| Feature | Description |
|---|---|
| Routing Type | Distance Vector |
| Metric | Hop Count |
| Maximum Hop Count | 15 |
| Unreachable Network | 16 Hops |
| Administrative Distance | 120 |
| RIPv2 | Supports Classless Routing and VLSM |
| Updates | Periodic updates, normally every 30 seconds |

RIP selects routes using **Hop Count**.

The path with the lowest hop count is generally preferred.

## ⚙️ RIPv2 Configuration

The following commands demonstrate a typical RIPv2 configuration.

> Note: These are example configurations based on the topology. The exact running configurations should be verified in the Packet Tracer project.

### Router R1

```bash
enable
configure terminal

router rip
version 2
no auto-summary

network 10.0.0.0
network 172.16.0.0

end
write memory
```

### Router R2

```bash
enable
configure terminal

router rip
version 2
no auto-summary

network 10.0.0.0

end
write memory
```

### Router R3

```bash
enable
configure terminal

router rip
version 2
no auto-summary

network 10.0.0.0

end
write memory
```

### Router R4

```bash
enable
configure terminal

router rip
version 2
no auto-summary

network 10.0.0.0
network 172.17.0.0

end
write memory
```

### Router R5

```bash
enable
configure terminal

router rip
version 2
no auto-summary

network 10.0.0.0

end
write memory
```

**Important:** RIP `network` commands use classful major network addresses. The `no auto-summary` command disables automatic route summarization.

## 🔁 Loopback Interfaces

Loopback interfaces are virtual interfaces that do not require physical network connections.

They are commonly used for:

- Simulating additional networks
- Testing routing protocols
- Providing stable logical interfaces
- Practicing network connectivity

### Example Loopback Configuration

On R1:

```bash
enable
configure terminal

interface loopback 0
ip address 172.16.16.1 255.255.255.0

interface loopback 1
ip address 172.16.17.1 255.255.255.0

end
write memory
```

On R4:

```bash
enable
configure terminal

interface loopback 0
ip address 172.17.17.1 255.255.255.0

interface loopback 1
ip address 172.17.18.1 255.255.255.0

end
write memory
```

These configurations illustrate how virtual networks can be added to a router and advertised through RIP.

## 🔍 Verification Commands

After configuring the routing protocol, several Cisco IOS commands can be used to verify the network.

### 1. Check the Routing Table

```bash
show ip route
```

This command displays the routes known by the router.

Routes learned through RIP are identified by the letter:

```text
R
```

Example:

```text
R 172.17.17.0/24 [120/2] via 10.10.12.2
```

Explanation:

- `R` = Route learned through RIP
- `172.17.17.0/24` = Destination network
- `120` = Administrative Distance
- `2` = RIP hop count
- `10.10.12.2` = Next-hop IP address

This is an illustrative routing-table entry, not an actual captured output from the lab.

### 2. Check RIP Configuration

```bash
show ip protocols
```

This command displays information about the configured routing protocol.

It can be used to verify:

- Whether RIP is enabled
- RIP version
- Advertised networks
- Routing update information
- Administrative Distance

### 3. Check Interface Status

```bash
show ip interface brief
```

This command shows interface IP addresses and operational status.

Interfaces should normally display:

```text
up    up
```

### 4. Test Connectivity

Use `ping` to verify whether remote IP addresses are reachable.

Example:

```bash
ping 172.17.17.1
```

Successful replies indicate that the destination is reachable.

### 5. Trace the Network Path

```bash
traceroute 172.17.17.1
```

This command helps identify the path that packets take through the network.

It is useful for understanding routing decisions and troubleshooting connectivity.

## 🔀 Routing Redundancy

The topology contains an alternative connection between R1 and R4 through R5.

This allows the lab to be used for testing routing redundancy.

### Example Test Scenario

1. Verify connectivity between R1 and R4.
2. Check the routing table using `show ip route`.
3. Identify the current route to the destination.
4. Shut down one interface along the active path.
5. Wait for RIP to update its routing information.
6. Check whether an alternative route is learned.
7. Restore the interface and observe routing changes.

Example command for shutting down an interface:

```bash
configure terminal

interface gigabitEthernet 0/0
shutdown
```

To restore the interface:

```bash
configure terminal

interface gigabitEthernet 0/0
no shutdown
```

**Note:** Replace `gigabitEthernet 0/0` with the actual interface being tested.

RIP uses hop count rather than bandwidth when choosing routes.

The presence of an alternative physical connection does not automatically guarantee successful failover; the interfaces, routing advertisements, and network configuration must be correct.

## 🛠️ Troubleshooting

Common routing problems can be investigated using the following commands:

| Command | Purpose |
|---|---|
| `show ip route` | Check learned routes |
| `show ip protocols` | Check RIP configuration |
| `show ip interface brief` | Check interface status |
| `show running-config` | Review router configuration |
| `ping` | Test connectivity |
| `traceroute` | Identify the packet path |

### Common Issues

**1. Incorrect IP Addressing**

Check whether router interfaces use the expected IP addresses and subnet masks.

**2. Interface Is Down**

Verify the interface status and use `no shutdown` when necessary.

**3. RIP Is Not Configured Correctly**

Check the RIP version, network statements, and `no auto-summary` configuration.

**4. Missing RIP Routes**

Verify that the relevant interfaces participate in RIP and that routing updates can reach neighboring routers.

**5. Network Connectivity Failure**

Use `ping`, `traceroute`, and `show ip route` to identify where communication fails.

## 🎯 Learning Objectives

Through this project, I practiced:

- Designing a multi-router network topology
- Configuring IPv4 addresses
- Understanding Dynamic Routing
- Configuring RIPv2 on Cisco routers
- Understanding Distance Vector Routing
- Working with loopback interfaces
- Understanding Administrative Distance
- Analyzing routing tables
- Verifying network connectivity
- Troubleshooting router connections
- Understanding routing redundancy and alternative paths

## 📂 Repository Structure

```text
cisco-rip-dynamic-routing-lab/
│
├── README.md
│
├── RIP-Dynamic-Routing.pkt
│
├── screenshots/
│   └── network-topology.png
│
└── configurations/
    ├── R1.txt
    ├── R2.txt
    ├── R3.txt
    ├── R4.txt
    └── R5.txt
```

Configuration files and the Packet Tracer project should be added to the repository when available.

## 🚀 How to Run the Project

1. Install Cisco Packet Tracer.
2. Clone or download this GitHub repository.
3. Open the `.pkt` file using Cisco Packet Tracer.
4. Examine the router connections and IPv4 configuration.
5. Open a router's CLI.
6. Run `show ip protocols` to inspect the routing configuration.
7. Run `show ip route` to check the routing table.
8. Use `ping` and `traceroute` to test connectivity.
9. Experiment with interface shutdowns to investigate route changes.

## 📚 Conclusion

This lab provides practical experience with Cisco dynamic routing using RIPv2.

It demonstrates how routers can exchange routing information, learn remote networks, and use routing tables to forward packets.

The project also introduces loopback interfaces, routing metrics, network troubleshooting, and alternative network paths.

It is part of my hands-on Cisco networking practice and CCNA preparation.

## 🧰 Tools & Environment

- Cisco Packet Tracer
- Cisco IOS
- IPv4 Networking
- RIPv2
- Cisco CLI
