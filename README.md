# Lab 1 – Multi-Network Routing & Centralized DHCP

## 📌 Lab Overview

This lab focuses on designing and configuring a multi-network infrastructure using **Cisco Packet Tracer**.

The topology consists of **three LANs connected through three routers**, with dedicated networks used for router-to-router communication. The lab also demonstrates how a **centralized DHCP server** can provide IP addresses to clients across different networks using **DHCP Relay**.

---

## 🎯 Objectives

- Design a multi-network topology using Cisco Packet Tracer.
- Configure IPv4 addressing for different networks.
- Configure routers and switches.
- Establish communication between multiple networks.
- Configure a centralized DHCP server.
- Configure DHCP Relay using the `ip helper-address` command.
- Configure PCs to obtain IP addresses dynamically.
- Verify connectivity between different networks.
- Understand how DHCP requests are forwarded between different subnets.

---

## 🖧 Network Topology

The topology contains:

- **3 Routers**
  - Router0
  - Router1
  - Router2
- **3 Switches**
  - Switch0
  - Switch1
  - Switch2
- Multiple PCs
- **1 DHCP Server**
- **1 DNS Server**
- **1 Web Server**

The routers connect the different LANs and provide communication between the separate networks.

---

# 🌐 IP Addressing

The lab uses five different networks.

## Network 1 – LAN 1

**Network:** `192.168.1.0/24`

| Parameter | Address |
|---|---|
| Network Address | `192.168.1.0` |
| First Host | `192.168.1.1` |
| Last Host | `192.168.1.254` |
| Broadcast | `192.168.1.255` |
| Router Gateway | `192.168.1.254` |

This network contains the main servers and several client PCs.

---

## Network 2 – LAN 2

**Network:** `192.168.2.0/24`

| Parameter | Address |
|---|---|
| Network Address | `192.168.2.0` |
| First Host | `192.168.2.1` |
| Last Host | `192.168.2.254` |
| Broadcast | `192.168.2.255` |
| Router Gateway | `192.168.2.1` |

This network contains client devices connected through Switch1.

---

## Network 3 – LAN 3

**Network:** `192.168.3.0/24`

| Parameter | Address |
|---|---|
| Network Address | `192.168.3.0` |
| First Host | `192.168.3.1` |
| Last Host | `192.168.3.254` |
| Broadcast | `192.168.3.255` |
| Router Gateway | `192.168.3.1` |

This network contains client devices connected through Switch2.

---

## Network 4 – Router-to-Router Network

**Network:** `192.168.4.0/24`

This network provides communication between **Router1 and Router2**.

- Router1: `192.168.4.1`
- Router2: `192.168.4.2`
- Broadcast: `192.168.4.255`

---

## Network 5 – Router-to-Router Network

**Network:** `192.168.5.0/24`

This network provides communication between **Router0 and Router1**.

- Router0: `192.168.5.1`
- Router1: `192.168.5.2`
- Broadcast: `192.168.5.255`

---

# 🖥️ Servers

## DHCP Server

The DHCP server is located in **Network 1**.

**IP Address:** `192.168.1.6`

The DHCP server is responsible for dynamically assigning IP addresses to client devices.

Since DHCP requests are broadcast messages, routers normally do not forward them between different networks. DHCP Relay is therefore used to allow clients in remote networks to communicate with the centralized DHCP server.

---

## DNS Server

A DNS server is configured in **Network 1**.

It is used to provide DNS name resolution within the network.

---

## Web Server

A web server is configured in **Network 1** and connected to Switch0.

It can be used to test connectivity and access services across the network.

---

# 🔄 DHCP Relay

A major concept demonstrated in this lab is **DHCP Relay**.

A client initially sends a DHCP Discover message as a broadcast. Routers do not forward Layer 2 broadcast messages between different networks.

Therefore, clients located in remote networks cannot directly reach the DHCP server in Network 1.

To solve this, DHCP Relay is configured on the router interface connected to the client network.

The following command is used:

cisco
ip helper-address 192.168.1.6

This allows the router to forward DHCP requests to the centralized DHCP server.

### DHCP Process

```text
Client
   |
   | DHCP Discover
   ↓
Local Router
   |
   | DHCP Relay
   | ip helper-address 192.168.1.6
   ↓
DHCP Server
   |
   | DHCP Offer
   ↓
Router
   |
   ↓
Client
```

Using DHCP Relay allows clients in different subnets to obtain their IP configuration from the same centralized DHCP server.

---

# 🛠️ Router Configuration

## Router0

Router0 connects:

- Network 1
- Network 5

The configured addresses are:

```text
Network 1 → 192.168.1.254
Network 5 → 192.168.5.1
```

Router0 connects the first LAN to Router1.

---

## Router1

Router1 connects:

- Network 5
- Network 2
- Network 4

The configured addresses are:

```text
Network 2 → 192.168.2.1
Network 4 → 192.168.4.1
Network 5 → 192.168.5.2
```

Router1 acts as an intermediate router between the connected networks and provides a path between Router0 and Router2.

---

## Router2

Router2 connects:

- Network 4
- Network 3

The configured addresses are:

```text
Network 3 → 192.168.3.1
Network 4 → 192.168.4.2
```

Router2 provides connectivity between Network 3 and the rest of the topology.

---

# 📡 DHCP Verification

DHCP functionality was tested using client PCs.

The PCs were configured to obtain their IP addresses dynamically from the centralized DHCP server.

Examples include:

```text
PC5 → DHCP assigned
PC6 → DHCP assigned
PC4 → DHCP assigned
```

This verifies that DHCP Relay allows DHCP services to operate across different subnets.

---

# 🔍 Connectivity Testing

After configuring the routers and DHCP services, network connectivity can be verified using the `ping` command.

Examples of connectivity tests include:

```text
PC → Default Gateway
PC → Another Network
PC → Router Interface
PC → Server
```

Successful ping responses confirm that the required network configurations and connectivity are working correctly.


This allows the router to forward DHCP requests to the centralized DHCP server.

### DHCP Process

```text
Client
   |
   | DHCP Discover
   ↓
Local Router
   |
   | DHCP Relay
   | ip helper-address 192.168.1.6
   ↓
DHCP Server
   |
   | DHCP Offer
   ↓
Router
   |
   ↓
Client
```

Using DHCP Relay allows clients in different subnets to obtain their IP configuration from the same centralized DHCP server.

---

# 🛠️ Router Configuration

## Router0

Router0 connects:

- Network 1
- Network 5

The configured addresses are:

```text
Network 1 → 192.168.1.254
Network 5 → 192.168.5.1
```

Router0 connects the first LAN to Router1.

---

## Router1

Router1 connects:

- Network 5
- Network 2
- Network 4

The configured addresses are:

```text
Network 2 → 192.168.2.1
Network 4 → 192.168.4.1
Network 5 → 192.168.5.2
```

Router1 acts as an intermediate router between the connected networks and provides a path between Router0 and Router2.

---

## Router2

Router2 connects:

- Network 4
- Network 3

The configured addresses are:

```text
Network 3 → 192.168.3.1
Network 4 → 192.168.4.2
```

Router2 provides connectivity between Network 3 and the rest of the topology.

---

# 📡 DHCP Verification

DHCP functionality was tested using client PCs.

The PCs were configured to obtain their IP addresses dynamically from the centralized DHCP server.

Examples include:

```text
PC5 → DHCP assigned
PC6 → DHCP assigned
PC4 → DHCP assigned
```

This verifies that DHCP Relay allows DHCP services to operate across different subnets.

---

# 🔍 Connectivity Testing

After configuring the routers and DHCP services, network connectivity can be verified using the `ping` command.

Examples of connectivity tests include:

```text
PC → Default Gateway
PC → Another Network
PC → Router Interface
PC → Server
```

Successful ping responses confirm that the required network configurations and connectivity are working correctly.


# Lab 3 – VLANs, Trunking & Inter-VLAN Routing

## 📌 Lab Overview

This lab focuses on configuring **VLANs, trunk links, and inter-VLAN routing** using Cisco Packet Tracer.

The network is divided into three VLANs representing different departments:

- **VLAN 10 – BA**
- **VLAN 20 – HR**
- **VLAN 30 – IT**

Multiple switches are connected to a central switch using trunk links. A Cisco router is configured using **Router-on-a-Stick** to provide communication between the different VLANs.

---

## 🎯 Objectives

The main objectives of this lab are:

- Create and configure VLANs.
- Assign meaningful names to VLANs.
- Assign switch ports to the appropriate VLANs.
- Configure access ports.
- Configure trunk ports.
- Configure a router for inter-VLAN routing.
- Configure router subinterfaces using 802.1Q encapsulation.
- Assign IP addresses to VLAN gateways.
- Verify VLAN configuration using `show vlan`.
- Understand the difference between access and trunk ports.
- Test communication between devices in different VLANs.

---

# 🖧 Network Topology

The topology contains:

- **1 Cisco Router**
  - Router1
- **4 Cisco Switches**
  - Switch0
  - Switch1
  - Switch2
  - Switch3
- Multiple PCs divided into three VLANs.

The VLANs are distributed across the switches:

```text
                         Router1
                            |
                       Trunk Link
                            |
                         Switch3
                       /    |    \
                  Trunk   Trunk   Trunk
                    /       |       \
               Switch0   Switch1   Switch2
                  |          |         |
              VLANs      VLANs      VLANs
             10/20/30   10/20/30   10/20/30
```

The links between the switches are configured as **trunk links**, allowing traffic from multiple VLANs to pass through the same physical connection.

---

# 🌐 VLAN Configuration

Three VLANs are configured in this lab.

| VLAN ID | VLAN Name | Network |
|---|---|---|
| VLAN 10 | BA | `192.168.10.0/24` |
| VLAN 20 | HR | `192.168.20.0/24` |
| VLAN 30 | IT | `192.168.30.0/24` |

Each VLAN represents a separate logical network.

---

## VLAN 10 – BA

**VLAN ID:** `10`

**VLAN Name:** `BA`

**Network:** `192.168.10.0/24`

| Parameter | Address |
|---|---|
| Network Address | `192.168.10.0` |
| Gateway | `192.168.10.1` |
| Broadcast | `192.168.10.255` |

The BA PCs are assigned to VLAN 10.

---

## VLAN 20 – HR

**VLAN ID:** `20`

**VLAN Name:** `HR`

**Network:** `192.168.20.0/24`

| Parameter | Address |
|---|---|
| Network Address | `192.168.20.0` |
| Gateway | `192.168.20.1` |
| Broadcast | `192.168.20.255` |

The HR PCs are assigned to VLAN 20.

---

## VLAN 30 – IT

**VLAN ID:** `30`

**VLAN Name:** `IT`

**Network:** `192.168.30.0/24`

| Parameter | Address |
|---|---|
| Network Address | `192.168.30.0` |
| Gateway | `192.168.30.1` |
| Broadcast | `192.168.30.255` |

The IT PCs are assigned to VLAN 30.

---

# 🏷️ Creating VLANs

The VLANs are created on the switches using the following configuration:

```cisco
enable
configure terminal

vlan 10
name BA

vlan 20
name HR

vlan 30
name IT
```

The same VLAN IDs and names are configured on the required switches so that VLAN traffic can be carried consistently across the topology.

---

# 🔌 Access Port Configuration

Access ports are used to connect end devices to their respective VLANs.

The topology uses:

- `FastEthernet 0/2` for VLAN 10
- `FastEthernet 0/3` for VLAN 20
- `FastEthernet 0/4` for VLAN 30

### VLAN 10 – BA

```cisco
interface fastethernet 0/2
switchport mode access
switchport access vlan 10
```

### VLAN 20 – HR

```cisco
interface fastethernet 0/3
switchport mode access
switchport access vlan 20
```

### VLAN 30 – IT

```cisco
interface fastethernet 0/4
switchport mode access
switchport access vlan 30
```

An access port carries traffic for a single VLAN and is typically used to connect end devices such as PCs.

---

# 🔗 Trunk Configuration

Trunk ports are used to carry traffic from multiple VLANs over a single physical link.

The switch uplink ports are configured as trunk ports.

Example:

```cisco
interface fastethernet 0/1
switchport mode trunk
```

The trunk links allow VLAN 10, VLAN 20, and VLAN 30 traffic to travel between the switches and toward the router.

---

# 🚦 Router-on-a-Stick

Inter-VLAN routing is implemented using the **Router-on-a-Stick** technique.

Instead of using a separate physical router interface for each VLAN, one physical router interface is divided into multiple logical **subinterfaces**.

The router interface used in the lab is:

```text
GigabitEthernet 0/0/0
```

Each subinterface is associated with one VLAN.

---

# 🛠️ Router Configuration

## VLAN 10 – BA

The router subinterface for VLAN 10 is configured using:

```cisco
interface gigabitEthernet 0/0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
```

This subinterface acts as the default gateway for devices in VLAN 10.

**Gateway:** `192.168.10.1`

---

## VLAN 20 – HR

The router subinterface for VLAN 20 is configured using:

```cisco
interface gigabitEthernet 0/0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

This subinterface acts as the default gateway for devices in VLAN 20.

**Gateway:** `192.168.20.1`

---

## VLAN 30 – IT

The router subinterface for VLAN 30 is configured using:

```cisco
interface gigabitEthernet 0/0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
```

This subinterface acts as the default gateway for devices in VLAN 30.

**Gateway:** `192.168.30.1`

---

# 🔐 802.1Q Encapsulation

The command:

```cisco
encapsulation dot1Q <VLAN-ID>
```

associates each router subinterface with its corresponding VLAN.

For example:

```cisco
encapsulation dot1Q 10
```

associates the subinterface with VLAN 10.

Similarly:

```cisco
encapsulation dot1Q 20
```

is used for VLAN 20, and:

```cisco
encapsulation dot1Q 30
```

is used for VLAN 30.

This allows the router to identify which VLAN each incoming frame belongs to.

---

# 🔄 Inter-VLAN Routing

Devices in different VLANs are logically separated and cannot communicate directly at Layer 2.

The router provides communication between the VLANs.

For example:

```text
VLAN 10
192.168.10.0/24
      |
      | Gateway
      ↓
192.168.10.1
      |
    Router
      |
192.168.20.1
      |
      ↓
VLAN 20
192.168.20.0/24
```

The router receives traffic from one VLAN, routes it at Layer 3, and forwards it toward the destination VLAN.

This allows devices from VLAN 10, VLAN 20, and VLAN 30 to communicate when routing is correctly configured.

---

# 🖥️ End Devices

The PCs are divided between the three VLANs.

### BA Department

The BA PCs are connected to ports assigned to:

```text
VLAN 10
192.168.10.0/24
Gateway: 192.168.10.1
```

### HR Department

The HR PCs are connected to ports assigned to:

```text
VLAN 20
192.168.20.0/24
Gateway: 192.168.20.1
```

### IT Department

The IT PCs are connected to ports assigned to:

```text
VLAN 30
192.168.30.0/24
Gateway: 192.168.30.1
```

---

# 🔍 VLAN Verification

The VLAN configuration can be checked using:

```cisco
show vlan
```

or:

```cisco
show vlan brief
```

These commands can be used to verify:

- VLAN IDs
- VLAN names
- Assigned access ports
- VLAN status

The lab specifically uses the `show vlan` command to verify that the VLAN configuration is correct on the switches.

---

# 🔗 Trunk Verification

Trunk configuration can be verified using:

```cisco
show interfaces trunk
```

This command can be used to check:

- Which interfaces are operating as trunks.
- Which VLANs are allowed on the trunk.
- Trunk status.
- Encapsulation information.

---

# 🧪 Connectivity Testing

After configuring the VLANs, trunks, and router subinterfaces, connectivity can be tested using the `ping` command.

Examples include:

```text
PC in VLAN 10 → Default Gateway 192.168.10.1
PC in VLAN 20 → Default Gateway 192.168.20.1
PC in VLAN 30 → Default Gateway 192.168.30.1
```

Inter-VLAN connectivity can also be tested:

```text
VLAN 10 PC → VLAN 20 PC
VLAN 10 PC → VLAN 30 PC
VLAN 20 PC → VLAN 30 PC
```

Successful ping responses confirm that the VLAN and inter-VLAN routing configuration is functioning correctly.

---

# 📚 Key Concepts Learned

This lab provides practical experience with:

- VLANs
- VLAN IDs
- VLAN naming
- Access ports
- Trunk ports
- 802.1Q
- Router-on-a-Stick
- Router subinterfaces
- Inter-VLAN routing
- Default gateways
- Cisco IOS commands
- VLAN verification
- Network segmentation

---

# ✅ Lab Outcome

The final topology successfully separates the network into three logical VLANs representing **BA, HR, and IT** departments.

Access ports are used to assign end devices to their respective VLANs, while trunk links allow multiple VLANs to travel between switches.

The router is configured using **Router-on-a-Stick**, with separate subinterfaces and 802.1Q encapsulation for each VLAN.

This configuration provides **inter-VLAN routing**, allowing devices from different VLANs to communicate through the router while maintaining logical network segmentation.

---

## 🧰 Tools & Technologies

- Cisco Packet Tracer
- Cisco IOS
- VLAN
- Trunking
- 802.1Q
- Router-on-a-Stick
- Inter-VLAN Routing
- IPv4
- Cisco Switches
- Cisco Router
