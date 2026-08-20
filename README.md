# 3×3 Neuromorphic Photodetector Array Readout using AER

### Transistor-Level Analog/VLSI Circuit Design | LTspice | Address Event Representation

A transistor-level investigation and simulation of a **3×3 neuromorphic photodetector array readout architecture** using **Address Event Representation (AER)**.

The project focuses on converting photodetector activity into asynchronous neuron events, generating requests from active pixels, and resolving competing requests using **MOSFET-based arbiter circuits and hierarchical arbiter trees**.

> **Project Status: Ongoing**
>
> The current work primarily covers the photodetector/neuron circuit, request generation, steering, choosing circuit, row request circuitry, and arbiter-tree simulation. Address generation and ACK generation, and the complete reset protocol are planned future stages.

---

## Overview

Conventional image sensors generally read pixels sequentially or process complete frames. In an event-driven neuromorphic system, only pixels that generate an event need to request access to the communication system.

The intended signal flow is:

**Photodetector → Pixel / Neuron → Request Generation → Steering → Arbitration → Arbiter Tree → Row / Column Selection → Address Generation → Receiver → ACK → Pixel Reset**

The project is being developed at the transistor level using **LTspice**, with emphasis on analog circuit behaviour, MOSFET-level implementation and asynchronous arbitration.

---

## Project Objectives

- Design and simulate a **3×3 photodetector array**.
- Convert photodetector activity into electrical neuron events.
- Generate request signals from active pixels.
- Design and simulate **2-input asynchronous arbiter circuits**.
- Construct hierarchical arbiter trees.
- Investigate request steering and winner selection.
- Develop row request circuitry for the 2D arbitration architecture.
- Study the AER data-transfer and acknowledge/reset protocol.
- Investigate address transmission and receiver implementation.
- Eventually validate the architecture using a hardware receiver.

---

# System Architecture

The intended complete system consists of the following stages:

**3×3 Photodetector Array**  
↓  
**Pixel / Neuron**  
↓  
**Request Generation**  
↓  
**Steering**  
↓  
**Row Arbitration**  
↓  
**Column Arbitration**  
↓  
**Address Generation**  
↓  
**Receiver**  
↓  
**ACK**  
↓  
**Pixel Reset**

> **Note:** The current implementation does not yet include the complete address generation, receiver, ACK, or pixel-reset stages.

---

# 1. Pixel / Neuron

Each pixel represents a photodetector-neuron element.

When light falls on the photodetector, it produces an electrical response. The resulting signal is processed by the neuron and compared against a threshold.

Once the threshold is crossed, the pixel generates an event.

**Light → Photodetector → Electrical Signal → Neuron / Threshold → Event**

The pixel/neuron circuit has been investigated and simulated using LTspice.

# 2. Request Generation Circuit

The request-generation circuit converts the neuron event into a request signal for the arbitration network.

A request indicates that a particular pixel wants access to the communication system.

The basic operation is:

**Neuron Event → Request Generation Circuit → Request Signal**

For the 3×3 array, the generated requests are associated with the corresponding rows and columns.

The request-generation circuitry has been implemented and investigated in LTspice.

---

# 3. Steering Circuit

The steering circuitry determines how requests are routed through the two-dimensional arbitration architecture.

The intended operation is:

**Pixel Request → Row Request → Row Arbitration → Selected Row → Column Requests → Column Arbitration**

The steering stage is important because multiple pixels may generate requests simultaneously, while the column arbitration stage should operate on the selected row.

The steering circuitry has been investigated as part of the row/column arbitration architecture.

---

# 4. Choosing Circuit

The choosing circuit is the basic **2-input arbitration element**.

When two requests compete for access to the communication bus, the choosing circuit resolves the contention and selects one winning request.

The basic operation is:

**Request 1 + Request 2 → Choosing Circuit → Winner**

The transistor-level implementation uses:

- **BSS84 PMOS**
- **BS170 NMOS**

The circuit has been simulated in LTspice to investigate:

- Request competition
- Winner selection
- Transient response
- MOSFET switching behaviour
- Propagation delay

---

# 5. Arbiter Tree

A larger arbitration network can be constructed by connecting multiple 2-input choosing circuits hierarchically.

For example:

**Request 1 + Request 2 → Arbiter 1**

**Request 3 + Request 4 → Arbiter 2**

**Arbiter 1 + Arbiter 2 → Final Arbiter → Winner**

The tree architecture provides a modular method for resolving multiple simultaneous requests.

### Advantages

- Modular implementation
- Scalable architecture
- Reusable 2-input arbiter cell
- Reduced complexity compared with a single large arbiter
- Suitable for asynchronous event-driven communication

The arbiter-tree implementation has been simulated as part of the project.
---

# 6. 3×3 Photodetector Array

The individual pixel circuits and arbitration blocks are combined to investigate a **3×3 sensor array**.

The array consists of nine pixels:

**P11  P12  P13**

**P21  P22  P23**

**P31  P32  P33**

Each pixel can independently generate a request.

For example, if pixel **P23** becomes active:

**P23 → Row 2 Request → Row Arbitration → Row 2 Selected**

The selected row would then participate in column arbitration to determine the corresponding column.

The row and column information together would identify the active pixel.

# 7. Address Generation, Receiver & Reset

After row/column arbitration, the selected pixel will eventually be converted into an address and transmitted to a receiver.

**Arbitration → Address Generation → Receiver → ACK → Pixel Reset**

The **address generation, receiver, ACK generation, and complete pixel-reset circuitry have not yet been implemented**. These are planned future stages of the project. The AER transmission and reset protocols have been studied as part of the project.

# 8. LTspice Implementation

LTspice is being used for transistor-level circuit simulation and debugging.

The simulations focus on:

- MOSFET switching behaviour
- Request generation
- Steering behaviour
- Arbiter operation
- Winner selection
- Arbiter-tree operation
- Row request signals

---

# 9. Current Project Status

## Completed / Investigated

- [x] Literature study of AER architectures
- [x] 3×3 photodetector array simulation
- [x] Pixel/neuron circuit investigation
- [x] Request-generation circuit
- [x] Steering circuit investigation
- [x] 2-input choosing circuit
- [x] MOSFET-based arbiter design
- [x] Hierarchical arbiter tree
- [x] Row request generation and arbitration
- [x] Study of AER sender/receiver architectures
- [x] Study of AER acknowledge/reset protocols

## Not Yet Implemented

- [ ] Address generation and encoding
- [ ] Dedicated receiver circuitry
- [ ] ACK generation
- [ ] Complete pixel-reset circuit
- [ ] Hardware implementation and validation


# Repository Structure

```text
3x3-photodetector-aer-readout/
│
├── README.md
│
├── Presentation/
│
└── ltspice/
    │
    ├── pixel_neuron/
    │
    ├── arbiter_cell/
    │   ├── steering/
    │   ├── choosing/
    │   └── request_generation/
    │
    ├── arbiter_tree/
    │
    └── 3x3_array/
```

# References

1. **Misha Mahowald**, *VLSI Analogs of Neuronal Visual Processing: A Synthesis of Form and Function*, PhD Thesis, California Institute of Technology.

2. **Kwabena A. Boahen**, *Communicating Neuronal Ensembles Between Neuromorphic Chips*, Proceedings of the 1998 International Conference on Neural Networks.

3. **Kwabena A. Boahen**, *A Burst-Mode Word-Serial Address-Event Representation: 2D Communication with Arbitration*, IEEE Transactions on Circuits and Systems II.

4. Literature on **Address Event Representation (AER)**, asynchronous arbitration, neuromorphic vision, and two-dimensional data-transfer protocols.
