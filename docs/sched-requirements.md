# Quantum Reservation System Requirements
## Definitions

| Term | Definition |
| --- | --- |
| **Quantum Resource** | Is a combination of a classical quantum gateway directly connected to the control electronics which is connected to the Quantum computer. |
| **Quantum Task** | Is a task to be **executed** on the **quantum resource**.<br><br>*A Quantum Task can include classical code in support of quantum code*. |
| **Hybrid Application** | Is an **application** which **contains both classical and quantum code**. Classical portion doesn't necessarily require HPC classical resources. Think of a simple script which generates circuits and runs them. |
| **Job** | Is a **SLURM submitted Hybrid Application**. A **hybrid Application** can require **both** **HPC classical** and **quantum** **resources** at the same time (**simultaneous allocation**) or it can only require a quantum resource (**interleaved allocation**)<br><br>*For the sake of simplicity if an sbatch contains multiple srun commands running different binaries or scripts, the entire set of commands are considered one single Hybrid Application* |

## Requirements
| Req-ID | Requirement Description |
| --- | --- |
| **GEN-001** | Each QC must have a dedicated classical compute. Classical compute run classical services such as resource manager daemons, etc. The combination of the QC and associated classical compute is termed **quantum resource**.|
| **GEN-002** | QC allocation shall ensure access to a quantum resource. |
| **RES-003** | A quantum allocation request shall specify the following parameters: <ul> <li>maximum number of circuits the application will execute</li> <li>maximum circuit size</li><li>maximum gate depth</li><li>maximum number of shots per circuit</li><li>Resource type</li><li>max wall time</li></ul> |
| **RES-004** | A reservation shall only be granted if sufficient QC capacity is available. |
| **RES-005** | QC capacity shall be defined as the number of circuits that can be executed per time unit.<ul><li>**circuit**: shall be defined as the largest supported configuration, constrained by the QC’s qubit count and coherence time.</li><li>**Time unit**: shall be a predefined window (e.g., day, hour, minute) minus the time required for machine calibration.</li></ul>|
| **RES-006** | QC capacity shall be expressed in terms of a maximum number of credits. |
| **RES-007** | The system shall estimate a circuit's execution time using the following criteria:<ul><li>latency of transferring digital circuit pulses to the Arbitrary Wave Generator (AWG)</li><li>latency of transferring analog circuit pulses to the QC</li><li>Duration of gate operations</li><li>Qubit measurement overhead</li><li>The maximum number of qubits</li><li>The maximum number of gates</li></ul> |
| **QOS-008** | Each **job** shall be allocated a set of credits to regulate task submission rates. |
| **QOS-009** | Submitting a **quantum task** shall consume one or more credits, based on the size of the quantum task. |
| **QOS-010** | When credits are exhausted, subsequent tasks shall be queued until additional credits become available. |
| **QOS-011** | Upon submission, the system shall guarantee task completion within the circuit execution time plus a fixed constant overhead. <br><br>This guarantee ensures all circuits in a reservation finish within the specified maximum wall time |
| **QOS-012** | The system shall allow jobs to be assigned priorities, with quantum tasks from higher-priority jobs scheduled first. |
| **QOS-013** | The system shall prevent lower priority tasks from starving. |

