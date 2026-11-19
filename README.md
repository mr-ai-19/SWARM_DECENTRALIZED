# Abstract Swarm Framework

A **platform-independent, GPS-free decentralized swarm coordination framework** for autonomous robot fleets. It is designed to work without a central hub or satellite positioning, and to support different vehicle types through small hardware adapters.

> **Project status:** Proof of concept using ESP32-based ground rovers. Aerial support is planned. Benchmark fields below are placeholders and must be replaced with measured results before being presented as completed trial data.

## Why this project?

Traditional autonomous robot fleets often depend on:

- A **single central controller**, which can become a point of failure.
- **GPS**, which may be unavailable indoors, underground, or under interference.
- **Platform-specific software**, requiring separate coordination systems for different vehicle types.

The Abstract Swarm Framework explores a shared, decentralized alternative.

## Features

- **Peer-to-peer mesh communication:** robots exchange information without a central hub.
- **Dynamic leader election:** the swarm can elect a new leader after a leader failure.
- **Shared spatial memory:** a decaying 3D pheromone grid records exploration and coordination information.
- **Hazard marking:** hazard information reported by one robot can be shared with the swarm.
- **Live membership:** robots can join or leave during a mission.
- **Hardware abstraction:** platform-specific behavior is isolated behind a small adapter.
- **Capability-tagged cells:** map information can be interpreted according to each robot's capabilities. For example, a cell may be impassable to a ground rover but traversable by a drone.

## How it works

Each robot contributes observations and coordination data to the swarm over the peer-to-peer network. The shared pheromone grid provides a spatial memory that can grow as the team explores and decay over time. Hazard marks and task-related state are also shared so that robots can coordinate using the same evolving representation.

Robots begin from a **common known pose** and maintain a shared relative coordinate frame using onboard odometry. This design does not rely on GPS or external positioning anchors; localization quality therefore depends on the robots' odometry and the accuracy of their common starting reference.

The framework is intended to preserve shared mission state through leader handover, including pheromone data, hazard marks, and task claims. These behaviors should be validated through implementation testing and measured trials.

## Hardware abstraction

A platform connects to the framework through four standard operations:

| Operation | Purpose |
| --- | --- |
| `position()` | Return the vehicle's current pose. |
| `move()` | Issue a locomotion command. |
| `sense_obstacles()` | Return collision or obstacle data. |
| `send_message()` | Send a message through the mesh. |

Adding a platform means implementing an adapter for these operations, rather than changing the coordination core.

## Proof of concept

| Component | Hardware / protocol | Status |
| --- | --- | --- |
| Microcontroller | ESP32 | Proof of concept |
| Mesh protocol | ESP-NOW | Proof of concept |
| Ground vehicle | Custom chassis | Bench-trial platform |
| Aerial vehicle | Raspberry Pi + Pixhawk | Planned adapter |

### Bench trial

The planned/ongoing test environment is a **3 × 3 m indoor arena** with ground rovers. Record actual measurements before treating the following as results:

| Metric | Result |
| --- | --- |
| Discovery time | `[X] s` |
| Re-election time | `[Y] s` |
| Success under single-robot failure | `[Z]% over [N] runs` |

Replace `[X]`, `[Y]`, `[Z]`, and `[N]` with results from completed trials. Include the test procedure and conditions so the measurements can be reproduced.

## Applications

- **Primary target:** disaster search and rescue in indoor or GPS-denied environments.
- **Potential extensions:** warehouse logistics and infrastructure inspection.

## Getting started

This README describes the framework's goals and proof-of-concept design. Setup and run instructions will be added when the repository's implementation and build workflow are documented.

## Contributing

Contributions to the coordination core, hardware adapters, testing, and documentation are welcome. Please include the platform and test conditions when reporting behavior or performance results.

## Keywords

`swarm robotics` · `decentralized coordination` · `platform-independent` · `hardware abstraction` · `stigmergy` · `leader election` · `GPS-free localization` · `ESP32` · `ESP-NOW` · `disaster response`
