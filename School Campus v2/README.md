# Enterprise School Campus Network Project

## Overview

This project simulates a real-world enterprise school campus network designed using Cisco Packet Tracer. The network follows the hierarchical enterprise architecture model consisting of Core, Distribution, and Access layers to provide scalability, redundancy, security, and simplified management.

The infrastructure was designed to support multiple departments, wireless connectivity, IP telephony, centralized services, and high availability mechanisms.

---

## Network Architecture

The topology was divided into three main layers:

### Core Layer

* Two multilayer core switches were deployed for redundancy and high availability.
* HSRP was implemented to provide a virtual default gateway for end devices.
* Core1 operates as the active gateway while Core2 acts as standby.

### Distribution Layer

* Distribution switches aggregate access layer connections.
* Trunk links and EtherChannels were configured between layers.
* Layer 2 traffic forwarding and VLAN propagation were optimized using STP.

### Access Layer

* Access switches connect end-user devices including PCs, IP Phones, printers, and wireless access points.
* Port Security and BPDU Guard were configured on edge ports.

---

## Implemented Technologies

### VLAN Segmentation

Separate VLANs were created for:

* Management
* Administration
* HR
* Doctors
* IT
* Laboratories
* Teachers
* Classrooms
* Students
* Servers
* Voice
* Wireless Access Points

This segmentation improved security, traffic isolation, and network organization.

---

## Inter-VLAN Routing

Inter-VLAN communication was implemented using:

* SVIs (Switched Virtual Interfaces)
* Layer 3 switching
* IP Routing on multilayer switches

---

## Redundancy and High Availability

### HSRP

HSRP was configured between the two core switches to ensure gateway redundancy and continuous network availability during failures.

### EtherChannel

LACP EtherChannels were configured to:

* Increase bandwidth
* Provide link redundancy
* Improve fault tolerance

---

## Spanning Tree Optimization

STP was configured to prevent switching loops and maintain Layer 2 stability.

Implemented features:

* BPDU Guard
* Root bridge optimization
* Trunk forwarding validation

---

## Wireless Infrastructure

A centralized wireless solution was implemented using:

* Wireless LAN Controller (WLC)
* Lightweight Access Points (LWAPs)
* WLAN grouping and profile mapping

Wireless VLAN integration was also configured for centralized wireless management.

---

## Voice Infrastructure

IP Phones were deployed using a dedicated Voice VLAN:

* Voice VLAN 110
* Switchport voice VLAN configuration on access ports

---

## Network Services

A centralized server room was implemented hosting multiple services including:

* DHCP
* DNS
* Syslog
* TFTP
* AAA services

DHCP scopes were configured for all VLANs.

---

## Security Features

The project includes several Layer 2 security implementations:

* Port Security
* BPDU Guard
* VLAN segmentation
* Dedicated management and server VLANs

---

## Objectives

The project was designed to simulate a real enterprise school environment while applying enterprise networking concepts such as redundancy, segmentation, centralized services, wireless management, and network security.
