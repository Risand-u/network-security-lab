# Static and Dynamic Routing Lab

Date: March 03, 2026
Topic: Static and Dynamic Routing (OSPF)
Stage: Stage 2 — Intermediate
Status: ✅ Completed

## What I Built
A network with 3 routers connected together
where PC0 and PC2 can communicate across
different networks using static and OSPF routing.

## Network Layout
PC0 - Switch1 - Router1 - Router0 - Router2 - Switch2 - PC2

## IP Plan

Left Network:
PC0            → 192.168.1.10
PC1            → 192.168.1.11
Router1 Gig0/0 → 192.168.1.1

Router1 to Router0 Link:
Router1 Gig0/1 → 10.0.0.1
Router0 Gig0/0 → 10.0.0.2

Router0 to Router2 Link:
Router0 Gig0/1 → 10.0.1.1
Router2 Gig0/0 → 10.0.1.2

Right Network:
Router2 Gig0/1 → 192.168.2.1
PC2            → 192.168.2.10
PC3            → 192.168.2.11

## Cable Types Used
PC to Switch     = Copper Straight-Through
Switch to Router = Copper Straight-Through
Router to Router = Copper Cross-Over

## Router IP Commands

Router1:
int gig0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
int gig0/1
ip address 10.0.0.1 255.255.255.0
no shutdown

Router0:
int gig0/0
ip address 10.0.0.2 255.255.255.0
no shutdown
int gig0/1
ip address 10.0.1.1 255.255.255.0
no shutdown

Router2:
int gig0/0
ip address 10.0.1.2 255.255.255.0
no shutdown
int gig0/1
ip address 192.168.2.1 255.255.255.0
no shutdown

## Static Route Commands

Router1:
ip route 10.0.1.0 255.255.255.0 10.0.0.2
ip route 192.168.2.0 255.255.255.0 10.0.0.2

Router0:
ip route 192.168.1.0 255.255.255.0 10.0.0.1
ip route 192.168.2.0 255.255.255.0 10.0.1.2

Router2:
ip route 192.168.1.0 255.255.255.0 10.0.1.1
ip route 10.0.0.0 255.255.255.0 10.0.1.1

## OSPF Commands

Router1:
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
network 10.0.0.0 0.0.0.255 area 0

Router0:
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
network 10.0.1.0 0.0.0.255 area 0

Router2:
router ospf 1
network 10.0.1.0 0.0.0.255 area 0
network 192.168.2.0 0.0.0.255 area 0

## Verification Commands
show ip route
show ip route ospf
show ip ospf neighbor
show ip interface brief

## Test Results
- Router1 OSPF neighbor → Router0 FULL ✅
- Router0 OSPF neighbors → Router1 and Router2 FULL ✅
- Router2 OSPF neighbor → Router0 FULL ✅
- O routes showing on all routers ✅
- PC0 ping PC2 success ✅
- PC0 ping PC3 success ✅

## What I Learned
- Routing decides where to send data
- Static routing is manual configuration
- OSPF is dynamic automatic routing
- All routers need OSPF configured
- O in route table means OSPF learned route
- S in route table means Static route
- C in route table means directly Connected
- Wildcard mask is opposite of subnet mask
- Area 0 is used for basic OSPF setup
