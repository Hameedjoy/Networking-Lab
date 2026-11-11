# Three-Site Enterprise Network Lab

## Project Overview

This project involves designing, configuring, and testing a three-site enterprise network using NetSandbox Network Topology Designer.

## The network connects three locations:

- Head Office
- Branch Office
- Warehouse

The objective was to establish communication between devices across all three locations using routers, switches, IP addressing, and static routing. This project provided practical experience in network design, router configuration, IP addressing, routing, connectivity testing, and troubleshooting.

## Network Topology

Click the image to view the full network diagram.
[Three-Site Enterprise Network Topology](network-topology.jpg)

# Network components:
```
Component  | Quantity
Routers         3
Switches        3
PCs             5
Total devices  11
Connections    10
```
## Network Addressing Scheme

The network uses three separate LAN subnets connected through two WAN links.
```
Location      | Network Address  | Default Gateway
Head Office   | 192.168.1.0/24   | 192.168.1.1
Branch Office | 192.168.2.0/24   | 192.168.2.1
Warehouse     | 192.168.3.0/24   | 192.168.3.1
```
## WAN Addressing

WAN Link             | Network      | Router Interface Addresses
Head Office – Branch | 10.0.12.0/30 | 10.0.12.1 and 10.0.12.2
Branch – Warehouse   | 10.0.23.0/30 | 10.0.23.2 and 10.0.23.1

Subnet mask for both WAN links: "255.255.255.252"

# Router Interface Configuration

## Head Office Router (R-HQ)

Interface  | IP Address        |     Purpose
Gi0/0      | 192.168.1.1/24    | Head Office LAN
Gi0/1      | 10.0.12.1/30      | WAN connection to Branch

## Branch Office Router (Router-2)

Interface  | IP Address      | Purpose
Gi0/0      | 192.168.2.1/24  | Branch LAN
Gi0/1      | 10.0.12.2/30    | WAN connection to Head Office
Gi0/2      | 10.0.23.2/30    | WAN connection to Warehouse

## Warehouse Router (R-Warehouse)

Interface | IP Address     | Purpose
Gi0/0     | 10.0.23.1/30   | WAN connection to Branch
Gi0/1     | 192.168.3.1/24 | Warehouse LAN

# Static Routing Configuration

Static routes were configured to allow the three locations to communicate with one another.

## R-HQ

Destination Network | Next Hop
192.168.2.0/24      | 10.0.12.2
192.168.3.0/24      | 10.0.12.2
10.0.23.0/30        | 10.0.12.2

## Router-2

Destination Network  | Next Hop
192.168.1.0/24       | 10.0.12.1
192.168.3.0/24       | 10.0.23.1

## R-Warehouse

Destination Network | Next Hop
192.168.1.0/24      | 10.0.23.2
192.168.2.0/24      | 10.0.23.2
10.0.12.0/30        | 10.0.23.2


## Troubleshooting and Problem Resolution

During network validation, an asymmetric reachability issue occurred between the Head Office and Warehouse router
The error indicated that a request reached its destination, but the reply could not return successfully.


1. I checked router interfaces and IP addressing.
2. I verified WAN connectivity between Router-2 and R-Warehouse.
3. I checked ARP tables to confirm that adjacent routers could resolve each other's addresses.
4. I tested connectivity between the routers using ping.
5. I inspected the Warehouse routing table.
6. I identified a missing static route to the Head Office WAN network.


## Resolution
Added the following static route on R-Warehouse

Setting      | Value
Destination  | 10.0.12.0/30
Next Hop     | 10.0.23.2
Interface    | Gi0/0


After adding the route, the ping test from R-Warehouse to the Head Office WAN interface (10.0.12.1) succeeded and the asymmetric reachability validation error subsequently disappeared.


Ping tests were performed to verify communication between the three office networks.

## Successful tests included:

  Head Office PC to both Branch Office PCs. 
  Head Office PC to both Warehouse PCs.
  Both Warehouse PCs to the Head Office PC.
  Branch Office PCs to the Warehouse PCs.
  Warehouse PC 1 to both Branch Office PCs.
  Warehouse PC 2 to both Branch Office PCs.

The reported ping tests completed successfully with no packet loss in the tested connections.


### What I gained through this project

- Enterprise network topology design.
- IPv4 addressing and subnet masks.
- LAN and WAN connectivity.
- Router and switch configuration.
- Static routing.
- ARP tables.
- ICMP and ping testing.
- Network troubleshooting.
- Identifying and resolving routing issues.
- Network documentation.

### The tool i Used is NetSandbox Network Topology Designer


This project strengthened my understanding of how multiple office networks can communicate through interconnected routers and switches. It also improved my troubleshooting skills by providing hands-on experience identifying and resolving a routing problem.

#### This is part of my ongoing practical learning journey in networking
