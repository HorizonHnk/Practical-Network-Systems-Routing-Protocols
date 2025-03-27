# Network Systems Engineering Project

![GitHub last commit](https://img.shields.io/badge/last%20commit-March%202024-brightgreen)
![GitHub](https://img.shields.io/badge/completion%20time-2%20weeks-blue)
![GitHub](https://img.shields.io/badge/status-completed-success)

This repository contains my comprehensive Network Systems 3 (NSS370S) project implementation, demonstrating practical application of scientific and engineering knowledge in networking. The project was completed individually in just 2 weeks and encompasses three major components: IP address design, virtual network topologies with routing protocols, and traffic engineering using graph algorithms.

## 🔗 Project Links

- **GitHub Repository**: [https://github.com/HorizonHnk/Practical-Network-Systems-Routing-Protocols.git](https://github.com/HorizonHnk/Practical-Network-Systems-Routing-Protocols.git)
- **YouTube Channel**: [https://www.youtube.com/playlist?list=PLrZbkNpNVSwxOyn31G9Q58oF95o9L9CoN](https://www.youtube.com/playlist?list=PLrZbkNpNVSwxOyn31G9Q58oF95o9L9CoN)

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [IP Address Design](#ip-address-design)
- [Virtual Network Topologies](#virtual-network-topologies)
- [Traffic Engineering with NetworkX](#traffic-engineering-with-networkx)
- [Installation & Setup](#installation--setup)
- [Results & Findings](#results--findings)
- [Conclusion](#conclusion)

## 🌐 Project Overview

This project demonstrates practical implementation of advanced networking concepts including:

- IPv4 address design and subnetting
- Interior Gateway Protocols (OSPF and EIGRP)
- Border Gateway Protocol (BGP)
- Multiprotocol Label Switching (MPLS)
- Graph theory and shortest path algorithms

The work was completed as part of the Bachelor of Engineering Technology in Computer Engineering at the Department of Electrical, Electronic and Computer Engineering, Cape Peninsula University of Technology.

## 🔢 IP Address Design

### Objective
Designed an efficient IPv4 addressing scheme for a network with specific host requirements, allowing for 60% future growth while optimizing address utilization.

### Methodology

1. **Host Analysis**
   - Analyzed the initial host requirements from the assignment (approximately 112,000 hosts)
   - Calculated total hosts needed after 60% growth (approximately 180,000 hosts)
   - Determined appropriate subnet mask to accommodate growth

2. **Subnet Planning**
   - Selected appropriate CIDR notation to accommodate all hosts
   - Implemented hierarchical addressing for efficient routing
   - Developed subnet allocation strategy to minimize IP address wastage

3. **Implementation Results**

| Requirement | Value |
|-------------|-------|
| Total Current Hosts | ~112,000 |
| With 60% Growth | ~180,000 |
| Selected CIDR Block | /15 (131,072 usable addresses) |
| Network Address | 172.16.0.0/15 |
| Broadcast Address | 172.17.255.255 |
| Usable Range | 172.16.0.1 - 172.17.255.254 |

4. **Subnetting Strategy**
   - Divided network into departmental subnets
   - Implemented VLSM (Variable Length Subnet Masking) for efficient allocation
   - Reserved address space for future expansion

```
Network: 172.16.0.0/15
├── Subnet A: 172.16.0.0/17 (32,766 hosts)
├── Subnet B: 172.16.128.0/17 (32,766 hosts)
├── Subnet C: 172.17.0.0/17 (32,766 hosts)
└── Subnet D: 172.17.128.0/17 (32,766 hosts)
```

## 🖧 Virtual Network Topologies

### Objective
Designed and implemented two virtual network topologies using GNS3 and VirtualBox, configured different routing protocols within each topology, and established connectivity between them.

### Implementation

#### Topology 1: OSPF Network
- Created a topology with 4 routers in a partial mesh configuration
- Implemented OSPF for internal routing
- Assigned /30 IP addresses (10.0.0.0/30, 10.0.0.4/30, etc.) to links
- Configured MPLS on all routers
- Established LSP (Label Switched Path) between R1 and R4

```
# Example OSPF Configuration for Router 1
router ospf 1
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.0.4 0.0.0.3 area 0
 network 192.168.1.0 0.0.0.255 area 0
!
# Example MPLS Configuration
mpls ip
!
interface FastEthernet0/0
 ip address 10.0.0.1 255.255.255.252
 mpls ip
```

#### Topology 2: EIGRP Network
- Created a topology with 3 routers in a star configuration
- Implemented EIGRP for internal routing
- Assigned /30 IP addresses (172.16.0.0/30, 172.16.0.4/30, etc.) to links

```
# Example EIGRP Configuration for Router 1
router eigrp 100
 network 172.16.0.0 0.0.0.3
 network 172.16.0.4 0.0.0.3
 no auto-summary
```

#### BGP Interconnection
- Connected the two topologies via BGP
- Configured eBGP between border routers
- Ensured route propagation between networks

```
# BGP Configuration for Border Router
router bgp 65001
 neighbor 192.168.100.2 remote-as 65002
 network 10.0.0.0 mask 255.0.0.0
```

### Network Diagram

```
                  +----------------+
                  |                |
 Topology 1 (OSPF)| BGP Connection | Topology 2 (EIGRP)
                  |                |
+-----+     +-----+     +-----+    |    +-----+     +-----+
| R1  |-----| R2  |-----| R3  |-------->| R4  |-----| R5  |
+-----+     +--+--+     +-----+    |    +-----+     +-----+
               |                   |        |
            +--+--+                |     +--+--+
            | R6  |                |     | R7  |
            +-----+                |     +-----+
                                   |
                  +----------------+
```

## 📊 Traffic Engineering with NetworkX

### Objective
Used NetworkX to create a network topology, analyze shortest paths with different algorithms, and visualize the results.

### Implementation

1. **Network Creation**
   - Implemented the assigned topology using NetworkX
   - Added nodes and edges with appropriate weights
   - Created visualization of the network

2. **Shortest Path Analysis**
   - Selected Dijkstra's algorithm for shortest path calculation
   - Compared results with other algorithms (Bellman-Ford, A*)
   - Analyzed how changing edge weights affects optimal paths

```python
import networkx as nx
import matplotlib.pyplot as plt

# Create the network graph
G = nx.Graph()

# Add nodes
for i in range(1, 12):
    G.add_node(i)

# Add edges with weights
edges = [(1, 2, 5), (1, 3, 3), (2, 4, 6), (3, 4, 7), (4, 5, 4),
         (5, 6, 2), (6, 7, 3), (6, 8, 4), (7, 8, 4), (8, 9, 2),
         (8, 10, 3), (9, 11, 6), (10, 11, 4)]

for u, v, w in edges:
    G.add_edge(u, v, weight=w)

# Compute shortest path
path = nx.dijkstra_path(G, source=1, target=11, weight='weight')
print(f"Shortest path: {path}")

# Visualization
pos = nx.spring_layout(G)
nx.draw(G, pos, with_labels=True, node_color='skyblue', node_size=700)
edge_labels = {(u, v): d['weight'] for u, v, d in G.edges(data=True)}
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)
plt.title('Network Topology with Edge Weights')
plt.show()
```

3. **Path Comparison Analysis**
   - Compared paths when all weights = 1 vs. original weights
   - Highlighted the differences in path selection (red for weighted path, green for unweighted)
   - Analyzed the implications for network design and traffic engineering

4. **Broader Applications**
   - Explored applications of graph algorithms in other domains:
     - Supply chain optimization
     - Transportation networks
     - Social network analysis
     - Data center traffic optimization

## 💻 Installation & Setup

### Prerequisites
- GNS3 (2.2.0 or later)
- VirtualBox (6.1 or later)
- Cisco IOS images for routers
- Python 3.7+
- NetworkX library
- Matplotlib library

### Setup Instructions

1. **GNS3 and VirtualBox Setup**
   ```bash
   # Install GNS3 on Ubuntu
   sudo add-apt-repository ppa:gns3/ppa
   sudo apt update
   sudo apt install gns3-gui gns3-server
   
   # Install VirtualBox
   sudo apt install virtualbox
   ```

2. **Python Environment**
   ```bash
   # Create virtual environment
   python -m venv venv
   source venv/bin/activate
   
   # Install required packages
   pip install networkx matplotlib numpy pandas
   ```

3. **Project Setup**
   ```bash
   # Clone repository
   git clone https://github.com/yourusername/network-systems-project.git
   cd network-systems-project
   
   # Run NetworkX analysis
   python traffic_engineering.py
   ```

## 📈 Results & Findings

### IP Addressing
- Successfully designed an addressing scheme that accommodates current needs and future growth
- Achieved high address utilization efficiency through VLSM implementation
- Created a hierarchical structure that simplifies management and troubleshooting

### Network Topologies
- Successfully established connectivity within and between both topologies
- Verified route propagation with BGP
- Implemented and tested MPLS functionality
- Packet capture analysis showed correct label assignment and forwarding

### Traffic Engineering
- Identified optimal paths through both weighted and unweighted networks
- Analysis showed that edge weights significantly impact path selection
- Implemented automatic path highlighting for visual analysis
- Found practical applications for graph theory in multiple engineering domains

## 🎯 Conclusion

This project demonstrates proficiency in applying scientific and engineering knowledge to real-world networking problems. Through careful design and implementation of IP addressing schemes, routing protocols, and traffic engineering algorithms, I've shown how theoretical networking concepts translate to practical applications.

The combination of virtual network simulation and algorithmic analysis provides a comprehensive understanding of both the operational and optimization aspects of modern networks. Despite the complexity of the tasks, completing this project individually in just 2 weeks demonstrates effective time management and technical competence in network engineering.

