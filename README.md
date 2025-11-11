# three-router-nat-configure-basicSimulation
# NAT Simulation with 3 Routers

A comprehensive Cisco Packet Tracer project demonstrating Network Address Translation (NAT) configuration across multiple routers with different network segments.

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Network Topology](#network-topology)
- [Requirements](#requirements)
- [Network Configuration](#network-configuration)
- [Step-by-Step Tutorial](#step-by-step-tutorial)
- [Testing & Verification](#testing--verification)
- [Troubleshooting](#troubleshooting)

---

## 🎯 Project Overview

This project simulates a network environment with:
- **3 Routers** interconnected in series
- **3 Switches** (one per router)
- **6 PCs** (2 PCs per switch)
- **3 Network Segments** with different IP ranges
- **NAT Configuration** for inter-network communication

### Network Segments:
- **Network 1:** 10.10.10.0/24 (PC0, PC1)
- **Network 2:** 20.20.20.0/24 (PC2, PC3)
- **Network 3:** 30.30.30.0/24 (PC4, PC5)

---

## 🗺️ Network Topology

![Network Topology](topology.png)

```
                40.40.40.1      40.40.40.2      50.50.50.1      50.50.50.2
                   Se2/0  <────>  Se2/0           Se3/0  <────>  Se2/0
                 Router-PT      Router-PT       Route

---

## 💻 Requirements

- **Cisco Packet Tracer** (version 7.0 or higher)
- Basic knowledge of:
  - IP addressing
  - Router CLI commands
  - NAT concepts

---

## 🔧 Network Configuration

### IP Address Scheme

#### End Devices (PCs)

| Device | IP Address | Subnet Mask | Default Gateway |
|--------|------------|-------------|-----------------|
| PC0 | 10.10.10.2 | 255.255.255.0 | 10.10.10.1 |
| PC1 | 10.10.10.3 | 255.255.255.0 | 10.10.10.1 |
| PC2 | 20.20.20.2 | 255.255.255.0 | 20.20.20.1 |
| PC3 | 20.20.20.3 | 255.255.255.0 | 20.20.20.1 |
| PC4 | 30.30.30.2 | 255.255.255.0 | 30.30.30.1 |
| PC5 | 30.30.30.3 | 255.255.255.0 | 30.30.30.1 |

#### Router Interfaces

| Router | Interface | IP Address | Connected To |
|--------|-----------|------------|--------------|
| Router0 | Gig0/0 | 10.10.10.1 | Switch0 |
| Router0 | Gig0/1 | 40.40.40.1 | Router1 |
| Router1 | Gig0/0 | 20.20.20.1 | Switch1 |
| Router1 | Gig0/1 | 40.40.40.2 | Router0 |
| Router1 | Gig0/2 | 50.50.50.1 | Router2 |
| Router2 | Gig0/0 | 30.30.30.1 | Switch2 |
| Router2 | Gig0/1 | 50.50.50.2 | Router1 |

---

## 📚 Step-by-Step Tutorial

### Step 1: Setting Up the Physical Topology

1. Open **Cisco Packet Tracer**
2. Add the following devices:
   - 3x **1941 Routers** (or similar)
   - 3x **2960 Switches**
   - 6x **PCs**

3. **Connect the devices:**
   - Connect PC0 and PC1 to Switch0
   - Connect PC2 and PC3 to Switch1
   - Connect PC4 and PC5 to Switch2
   - Connect Switch0 to Router0 (Gig0/0)
   - Connect Switch1 to Router1 (Gig0/0)
   - Connect Switch2 to Router2 (Gig0/0)
   - Connect Router0 (Gig0/1) to Router1 (Gig0/1) - *Link Network: 40.40.40.0/24*
   - Connect Router1 (Gig0/2) to Router2 (Gig0/1) - *Link Network: 50.50.50.0/24*

**See:** ROUTER0CLI.png, ROUTER1CLI.png, ROUTER2CLI.png

---

### Step 2: Configure PC IP Addresses

For each PC, click on the device → **Desktop** → **IP Configuration**

#### Network 1 (PCs connected to Router0):
- **PC0:**
  - IP Address: `10.10.10.2`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `10.10.10.1`

- **PC1:**
  - IP Address: `10.10.10.3`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `10.10.10.1`

#### Network 2 (PCs connected to Router1):
- **PC2:**
  - IP Address: `20.20.20.2`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `20.20.20.1`

- **PC3:**
  - IP Address: `20.20.20.3`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `20.20.20.1`

#### Network 3 (PCs connected to Router2):
- **PC4:**
  - IP Address: `30.30.30.2`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `30.30.30.1`

- **PC5:**
  - IP Address: `30.30.30.3`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: `30.30.30.1`

---

### Step 3: Configure Router0

Click on Router0 → **CLI** tab and enter:

```bash
Router> enable
Router# configure terminal

# Configure interface connected to Switch0 (LAN)
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 10.10.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure interface connected to Router1
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 40.40.40.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure NAT
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
Router(config-if)# exit

# Configure access list for NAT
Router(config)# access-list 1 permit 10.10.10.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload

# Configure static routes
Router(config)# ip route 20.20.20.0 255.255.255.0 40.40.40.2
Router(config)# ip route 30.30.30.0 255.255.255.0 40.40.40.2

# Save configuration
Router(config)# end
Router# write memory
```

---

### Step 4: Configure Router1

Click on Router1 → **CLI** tab and enter:

```bash
Router> enable
Router# configure terminal

# Configure interface connected to Switch1 (LAN)
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 20.20.20.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure interface connected to Router0
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 40.40.40.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure interface connected to Router2
Router(config)# interface GigabitEthernet0/2
Router(config-if)# ip address 50.50.50.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure NAT
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/2
Router(config-if)# ip nat outside
Router(config-if)# exit

# Configure access list for NAT
Router(config)# access-list 1 permit 20.20.20.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload

# Configure static routes
Router(config)# ip route 10.10.10.0 255.255.255.0 40.40.40.1
Router(config)# ip route 30.30.30.0 255.255.255.0 50.50.50.2

# Save configuration
Router(config)# end
Router# write memory
```

---

### Step 5: Configure Router2

Click on Router2 → **CLI** tab and enter:

```bash
Router> enable
Router# configure terminal

# Configure interface connected to Switch2 (LAN)
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 30.30.30.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure interface connected to Router1
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 50.50.50.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

# Configure NAT
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
Router(config-if)# exit

# Configure access list for NAT
Router(config)# access-list 1 permit 30.30.30.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload

# Configure static routes
Router(config)# ip route 10.10.10.0 255.255.255.0 50.50.50.1
Router(config)# ip route 20.20.20.0 255.255.255.0 50.50.50.1

# Save configuration
Router(config)# end
Router# write memory
```

---

## ✅ Testing & Verification

### Test Connectivity

1. **From PC0, ping PC2:**
   - Open PC0 → Desktop → Command Prompt
   - Type: `ping 20.20.20.2`
   - You should receive replies

2. **From PC0, ping PC4:**
   - Type: `ping 30.30.30.2`
   - You should receive replies

3. **Test all combinations:**
   - PC1 → PC3
   - PC2 → PC5
   - PC4 → PC0

### Verify Router Configuration

On each router, enter:

```bash
Router# show ip interface brief
Router# show ip route
Router# show ip nat translations
```

---

## 🔍 Troubleshooting

### Common Issues:

**1. PCs cannot ping each other:**
- Check if all router interfaces are "up/up": `show ip interface brief`
- Verify static routes: `show ip route`
- Check NAT configuration: `show ip nat translations`

**2. Wrong gateway configuration:**
- Ensure each PC's default gateway matches its router's LAN interface
- Example: PC0's gateway should be 10.10.10.1 (Router0's Gig0/0)

**3. NAT not working:**
- Verify inside/outside interfaces are configured correctly
- Check access-list permits the correct network
- Use: `show ip nat statistics`

**4. Routing issues:**
- Verify static routes point to correct next-hop addresses
- Ensure all networks are reachable: `show ip route`

---

## 📁 Project Files

- `topology.png` - Network topology diagram
- `ROUTER0CLI.png` - Router0 configuration screenshot
- `ROUTER1CLI.png` - Router1 configuration screenshot
- `ROUTER2CLI.png` - Router2 configuration screenshot
- `cisco.pkt` - Packet Tracer project file

---

## 📖 Learning Objectives

After completing this simulation, you will understand:
- ✅ How to configure basic router interfaces
- ✅ How to implement NAT (Network Address Translation)
- ✅ How to configure static routing between multiple routers
- ✅ How to troubleshoot inter-network connectivity
- ✅ Difference between inside and outside NAT interfaces

---

## 🤝 Contributing

Feel free to fork this project and submit improvements!

---

## 📝 License

This project is for educational purposes.

---

**Note:** Make sure all interface names match your actual Packet Tracer configuration. Interface names may vary (e.g., FastEthernet, GigabitEthernet).
