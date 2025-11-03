# Quantum Reservation System Requirements
## Definitions

| Term | Definition |
| --- | --- |
| **Top-Level Scheduler** | Is a scheduler like SLURM, PBS, etc. It manages both classical and quantum resources |
| **Quantum Resource** | Is a resource which provides quantum operations. It can be physical hardware or software simulator. |
| **Quantum Task** | Is a task to be **executed** on the **quantum resource**.<br><br>*A Quantum Task can include classical code in support of quantum code*. |
| **Hybrid Application** | Is an **application** which **contains both classical and quantum code**. Classical portion doesn't necessarily require HPC classical resources. Think of a simple script which generates circuits and runs them. |
| **Job** | Is a **SLURM submitted Hybrid Application**. A **hybrid Application** can require **both** **HPC classical** and **quantum** **resources** at the same time (**simultaneous allocation**) or it can only require a quantum resource (**interleaved allocation**)<br><br>*For the sake of simplicity if an sbatch contains multiple srun commands running different binaries or scripts, the entire set of commands are considered one single Hybrid Application* |

## Requirements

The Quantum Reservation System defines the mechanisms, policies, and interfaces required to integrate quantum computing resources within traditional HPC scheduling environments. Its purpose is to enable efficient and fair allocation of both classical and quantum resources—whether used independently or as part of hybrid applications. This system must accommodate diverse quantum modalities, support simultaneous or interleaved resource allocations, and ensure predictable execution through quantifiable metrics such as capacity, credits, and wall time. The following requirements outline the functionality expected from the top-level scheduler (e.g., SLURM) and its interaction with underlying quantum resources to provide a unified, extensible, and policy-driven framework for quantum workload management.

| Req-ID | Requirement Description |
| --- | --- |
| **TLS-001** | TLS shall allow the specification of a quantum resource. |
| **TLS-002** | TLS shall allow the specification of a set of metadata to describe a quantum resource. E.g, resource modality, number of supported qubits, etc |
| **TLS-003** | TLS shall provide mechanisms to allocate both quantum and classical resources, either simultaneously or in an interleaved manner. |
| **TLS-004** | TLS shall provide mechanisms to allow the customization of the reservation workflow to handle specific quantum resource requirements. |
| **GEN-005** | Each QC must have a dedicated classical compute. Classical compute run classical services such as resource manager daemons, etc. The combination of the QC and associated classical compute is termed **quantum resource**.|
| **GEN-006** | QC allocation shall ensure access to a quantum resource. |
| **RES-007** | A quantum allocation request from the TLS shall specify the following parameters: <ul> <li>maximum number of circuits the application will execute</li> <li>maximum circuit size</li><li>maximum gate depth</li><li>maximum number of shots per circuit</li><li>Resource type</li><li>max wall time</li></ul> |
| **RES-008** | A reservation shall only be granted if sufficient QC capacity is available. |
| **RES-009** | QC capacity shall be defined as the number of circuits that can be executed per time unit.<ul><li>**circuit**: shall be defined as the largest supported configuration, constrained by the QC’s qubit count and coherence time.</li><li>**Time unit**: shall be a predefined window (e.g., day, hour, minute) minus the time required for machine calibration.</li></ul>|
| **RES-010** | QC capacity shall be expressed in terms of a maximum number of credits. |
| **RES-011** | The system shall estimate a circuit's execution time using the following criteria:<ul><li>latency of transferring digital circuit pulses to the Arbitrary Wave Generator (AWG)</li><li>latency of transferring analog circuit pulses to the QC</li><li>Duration of gate operations</li><li>Qubit measurement overhead</li><li>The maximum number of qubits</li><li>The maximum number of gates</li></ul> |
| **QOS-012** | Each **job** shall be allocated a set of credits to regulate task submission rates. |
| **QOS-013** | Submitting a **quantum task** shall consume one or more credits, based on the size of the quantum task. |
| **QOS-014** | When credits are exhausted, subsequent tasks shall be queued until additional credits become available. |
| **QOS-015** | Upon submission, the system shall guarantee task completion within the circuit execution time plus a fixed constant overhead. <br><br>This guarantee ensures all circuits in a reservation finish within the specified maximum wall time |
| **QOS-016** | The system shall allow jobs to be assigned priorities, with quantum tasks from higher-priority jobs scheduled first. |
| **QOS-017** | The system shall prevent lower priority tasks from starving. |

