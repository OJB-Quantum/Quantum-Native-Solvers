# Quantum-Native-Solvers
A compilation of quantum-native solver techniques that can be mapped and run on a quantum computer. Compiled by Onri Jay Benally.

[![License](https://img.shields.io/badge/Creative_Commons-License-green)](https://choosealicense.com/licenses/cc-by-4.0) | [Repository](https://github.com/OJB-Quantum/Quantum-Native-Solvers)

<a id="contents"></a>

## Contents

- [Introduction](#introduction)
  - [Core Definitions and Global Legend](#core-definitions-legend)
- [Towards Deployment of Industry and Academic Quantum Processors](#towards-deployment-of-industry-and-academic-quantum-processors)
- [I. Quantum Simulation Taxonomy](#quantum-simulation-taxonomy)
  - [Classification Tree](#classification-tree)
  - [Placement Guide](#placement-guide)
- [II. Hardware Readiness and Tiers](#hardware-readiness)
  - [Compact Readiness Summary](#compact-readiness-summary)
  - [Extended Readiness Analysis](#extended-readiness-analysis)
  - [Sparse-Solver Readiness Checks](#sparse-solver-readiness-checks)
- [III. Resource Metrics and Qubit Scaling](#resource-metrics)
  - [Error Correction Footprint](#error-correction-footprint)
  - [Estimated Qubit Requirements](#estimated-qubit-requirements)
  - [D2Q5 Encoding Comparison](#d2q5-encoding-comparison)
- [IV. System Integration and Routing](#system-integration)
  - [Proposed Heterogeneous Architecture](#proposed-heterogeneous-architecture)
  - [Logic Routing and Decision Tree](#logic-routing-decision-tree)
  - [Sparse-Solver Routing Guide](#sparse-solver-routing-guide)
- [References](#references)

---

<a id="introduction"></a>

## Introduction

The purpose of this repository is to monitor computational techniques (over time) that can be used to determine whether a working mathematical or computational framework of interest may be eligible for quantum simulation.

Interestingly, some models such as the [Landau-Lifshitz-Gilbert (LLG) equation](https://iopscience.iop.org/article/10.1088/1367-2630/ae115c) (used in micromagnetism studies) can be systematically derived from [Lindbladian](https://en.wikipedia.org/wiki/Lindbladian) dynamics, which are based on the general form of Markovian master equations used to describe open quantum systems. Another [quantum analog of the LLG](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.133.266704) also exists based on quantum correlation dynamics, which acknowledges a difference from open quantum systems in the previous example. LLG solvers are typically implemented on classical computing resources, especially that of general-purpose graphics processing units (GPUs). However, through careful derivation, supported by literature, such a method can transcend the classical description and becomes eligible for quantum simulation algorithms, as well as numerically exact and analytical verifications. Although this is not always the case for other classical or semi-classical frameworks and models, examples like these that are physics-informed or physics-supported should encourage one to explore the limits.

<a id="core-definitions-legend"></a>

### Core Definitions and Global Legend

> **Quantum-native** (adj.): designed for, and inherently dependent upon, quantum-mechanical resources, so that its core function, scaling, or correctness requires nonclassical phenomena (for example, superposition, interference, entanglement), rather than merely imitating them on classical hardware.
>
> **quantum**: “Borrowed from Latin *quantum*,” historically ‘how much; as much as’ (neuter of *quantus*), later specialized to a discrete physical amount. - *Oxford English Dictionary*; see also Oxford’s gloss tying *quantum* to *quantus*.
> **native**: “From Latin *nativus* (‘inborn; produced by birth’), via Middle English/French,” yielding senses such as ‘innate, natural; belonging by birth.’ - *Oxford English Dictionary*.

---

## Towards Deployment of Industry and Academic Quantum Processors

| Label | Meaning | Interpretation |
| --- | --- | --- |
| C | Customer-accessible system or offering | Cloud access, procurement, or partner agreements may be required. |
| R | Research demonstration or testbed | Applies to the demonstrated implementation or protocol, rather than unrestricted access to every reported capability. |
| T | Announced target | An announced date does not establish completed deployment or demonstrated performance. |
| CLOUD | Remote execution | Hardware is hosted by the provider. |
| SITE | Customer or institutional installation | An offering does not establish that every proposed installation is operating. |
| LAB | Research apparatus | Register size and physical infrastructure are separate quantities. |
| FACILITY | Facility-oriented deployment | Does not itself establish fault-tolerant computation. |

```text
Qubit Architectures × Computational Readiness × Deployment
│
├─ I. Industry-Developed Machines and Offerings
│  │  (Commercial cloud, customer sites, and corporate research laboratories)
│  │
│  ├─ Superconducting Processors
│  │  ├─ IBM Heron [C; CLOUD/ FACILITY] 
│  │  │  ├─ r1: 133 programmable qubits
│  │  │  ├─ r2/ r3: 156 programmable qubits; heavy-hex connectivity
│  │  │  └─ Fixed-frequency qubits with tunable couplers
│  │  ├─ IBM Nighthawk [C; CLOUD/ FACILITY] 
│  │  │  ├─ r1/ r2: 120 programmable qubits; square-lattice connectivity
│  │  │  ├─ r2: cloud availability from September 2026; independent fast reset
│  │  │  └─ r2: 458 quantum elements = 120 qubits + 218 couplers + 120 reset elements
│  │  ├─ Rigetti Cepheus-1-108Q [C; CLOUD] 
│  │  │  ├─ Nominal 108-qubit system built from twelve 9-qubit chiplets
│  │  │  └─ General availability announced April 2026; usable count is backend-specific
│  │  ├─ IQM Garnet/ Emerald [C; CLOUD] 
│  │  │  └─ 20/ 54 qubits; Emerald uses square-lattice connectivity
│  │  ├─ Google Willow [R; LAB/ FACILITY] 
│  │  │  ├─ 105-qubit processor supports a distance-7 surface-code memory
│  │  │  ├─ Below-threshold scaling and beyond-breakeven memory demonstrated
│  │  │  └─ Real-time distance-5 decoding demonstrated on a 72-qubit processor
│  │  ├─ AWS Ocelot [R; LAB] 
│  │  │  ├─ Five cat data qubits + four transmon syndrome ancillas
│  │  │  ├─ Stabilization buffers are additional circuit resources
│  │  │  └─ Concatenated bosonic/ repetition-code logical-memory prototype
│  │  └─ IBM Loon [R; architecture prototype] 
│  │     └─ Long-range on-chip c-couplers and connectivity for qLDPC-code research
│  │
│  ├─ Trapped-Ion Processors
│  │  ├─ Quantinuum H2/ Helios [C; CLOUD; Helios also SITE offering] 
│  │  │  ├─ H2: 56 physical qubits
│  │  │  ├─ Helios: 98 physical qubits; QCCD transport and all-to-all connectivity
│  │  │  └─ Helios logical demonstrations [R capability] 
│  │  │     ├─ 94 error-detected logical qubits: global-entanglement experiment
│  │  │     ├─ 50 error-detected logical qubits: magnetism-simulation experiment
│  │  │     └─ 48 error-corrected logical qubits: state-preparation/ measurement result
│  │  ├─ IonQ Forte/ Forte Enterprise [C; CLOUD/ SITE] 
│  │  │  ├─ 36 physical qubits; all-to-all connectivity
│  │  │  └─ Algorithmic qubits (#AQ) identify a benchmark, not a logical-qubit count
│  │  └─ IonQ Tempo development system [R; development benchmark] 
│  │     └─ #AQ 64 reported in 2025; customer deployment is a separate status
│  │
│  ├─ Neutral-Atom Processors
│  │  ├─ QuEra Aquila [C; CLOUD] 
│  │  │  └─ Up to 256 atoms; programmable analog Rydberg simulation
│  │  ├─ QuEra Gemini [C; SITE/ partner access] 
│  │  │  └─ 260 physical qubits; gate-model processor and QEC testbed
│  │  ├─ Atom Computing AC1000 [C; SITE offering] 
│  │  │  └─ >1,200 ytterbium-171 nuclear-spin qubits; mid-circuit control
│  │  └─ Pasqal Orion-class systems [C; institutional/ partner use] 
│  │     └─ Analog Rydberg simulation; experiments reported with up to 256 atoms
│  │
│  ├─ Industry Research Collaborations
│  │  └─ Microsoft–Atom Computing [R; industry research] 
│  │     ├─ 24 entangled logical qubits
│  │     └─ 28 logical qubits in an error-managed algorithm demonstration
│  │
│  ├─ Specialized and Other Architectures
│  │  ├─ Alice & Bob Boson 4 [C; CLOUD] 
│  │  │  └─ One exposed cat qubit; restricted experimental instruction set
│  │  ├─ D-Wave Advantage2 [C; CLOUD/ FACILITY] 
│  │  │  └─ 4,400+ flux qubits; quantum annealing
│  │  ├─ Xanadu Aurora [R; modular racks] 
│  │  │  └─ Photonic prototype; cluster-state processing and modular integration
│  │  └─ PsiQuantum Omega [R; foundry-manufactured chipset] 
│  │     └─ Photonic components and interconnections; chipset rather than a qubit register
│  │
│  ├─ Semiconductor Spin Research Hardware
│  │  └─ Intel Tunnel Falls [R; research-chip access] 
│  │     └─ 12-dot research device; computational encoding depends on implementation
│  │
│  ├─ Classical Cryogenic Control Infrastructure
│  │  ├─ Intel Horse Ridge II: cryo-CMOS control at approximately 4 K 
│  │  └─ Intel Pando Tree: demultiplexing at approximately 10–20 mK 
│  │
│  └─ Announced Fault-Tolerant and Next-System Targets [T]
│     ├─ IBM: Starling targets 200 logical qubits and 100 million gates by 2029 
│     │  └─ Kookaburra/ Cockatoo: planned module and intermodule milestones
│     ├─ Alice & Bob Helium: announced QEC testbed targeting a first logical qubit 
│     ├─ Pasqal Vela: >256 high-quality qubits announced for 2026 
│     └─ QuEra Libra: 256 logical qubits targeted for 2028 
│
└─ II. Academic and Institutional Research Platforms
   │  (University laboratories, national centers, and university-linked collaborations)
   │
   ├─ Institutional Hybrid Platforms
   │  └─ Fujitsu–RIKEN [C; institutional hybrid platform] 
   │     ├─ Demonstrated: 256-qubit superconducting system
   │     └─ Target [T]: 1,000-qubit system; announced 2026 installation/ launch
   │
   ├─ University–Industry Collaborative Processors
   │  └─ Harvard–MIT–QuEra [R; LAB] 
   │     ├─ Reconfigurable processor with up to 448 atoms
   │     ├─ Up to 96 simultaneously active encoded logical qubits
   │     └─ Logical operations and repeated QEC; protocol-specific protection
   │
   ├─ Small-Register University and University-Linked Research Processors
   │  │  (Register size does not specify apparatus size or control-channel count)
   │  │
   │  ├─ Silicon Donor Electron–Nuclear Spin Registers
   │  │  └─ Silicon Quantum Computing/ UNSW [R; LAB] 
   │  │     ├─ 11 qubits: nine nuclear-spin data qubits + two electron-spin ancillas
   │  │     └─ Donor-based registers; distinct from gate-defined quantum dots
   │  │
   │  ├─ Gate-Defined Semiconductor Quantum-Dot Spin Processors
   │  │  ├─ Si/SiGe and silicon MOS research devices [R; LAB]
   │  │  ├─ QuTech/ TU Delft: universal control of six Si/SiGe spin qubits 
   │  │  └─ Register size is demonstration-specific; 2–4 qubits is not a ceiling
   │  │
   │  └─ Diamond Spin–Photon Quantum-Network Nodes [R; LAB] 
   │     ├─ NV electron-spin interface, nuclear-spin memories, and optical photons
   │     ├─ Three-memory-qubit repetition code with repeated bit-flip correction
   │     └─ Network-node and memory demonstrations; sensing is a separate application
   │
   └─ Fundamental Research Testbeds [R; LAB]
      │  (Architecture exploration and bounded protocol demonstrations)
      │
      ├─ Superconducting Research Processors and Component Testbeds
      │  ├─ Transmon testbeds
      │  │  ├─ Fixed-frequency and tunable designs; gates, readout, materials, and QEC
      │  │  └─ Cross-resonance for fixed-frequency devices; other gates depend on design 
      │  ├─ Fluxonium devices
      │  │  ├─ Coherence, couplers, and multiqubit gate studies
      │  │  ├─ Microwave-activated CZ gates demonstrated 
      │  │  └─ Report T1, Ramsey T2*, or echo T2 for the specified device 
      │  └─ Flux-qubit test circuits
      │     ├─ Hamiltonian engineering and quantum-annealing physics
      │     └─ Small-instance factorization using an annealing multiplier Hamiltonian 
      │
      ├─ Trapped-Ion Research Testbeds
      │  └─ Gate control, motional coupling, and multilevel qudit encoding 
      │
      └─ Photonic/ Optical Research Testbeds
         ├─ Optical benches and integrated photonic circuits
         ├─ Dual-rail encoding, Hong–Ou–Mandel interference, gates, and teleportation
         ├─ Boson-sampling experiments: sampling tasks rather than universal gate sets
         └─ Linear optics with suitable sources, detection, and feedforward:
            universal quantum-computing schemes, subject to implementation resources
```

---

| Label | Execution or scope |
| :---: | :--- |
| **Q** | Quantum circuits perform the central computation. |
| **H** | Quantum and classical computations form an essential hybrid workflow. |
| **A** | Native interactions in a programmable physical simulator implement the model. |
| **C** | Classical computation, potentially accelerated by GPUs. |
| **R** | A problem-specific research direction requiring an explicit construction and resource analysis. |
| **P26** | A cited preprint. |

[Back to contents](#contents)

---

<a id="quantum-simulation-taxonomy"></a>

## I. Quantum Simulation Taxonomy

<a id="classification-tree"></a>

### Classification Tree

<img width="2651" height="auto" alt="image" src="https://github.com/user-attachments/assets/88a9d559-aeec-43ce-a285-2d3fd31cce7f" />

```text
Quantum Simulation and Solver Approaches
│
├─ I. Lattice kinetic methods and automata [Q/H/A]
│  ├─ A. Quantum lattice Boltzmann methods (QLBM) [Q/H/A]
│  │  ├─ Reversible streaming and encoded population transport
│  │  ├─ Collision through dilation, measurement, or classical feedback
│  │  ├─ Carleman expansions and multiple-circuit formulations
│  │  └─ Node-level ensemble formulations for nonlinear fluid models
│  ├─ B. Quantum lattice gas automata (QLGA) [Q/A]
│  │  ├─ Occupation registers with collision and streaming operations
│  │  └─ Model-specific hydrodynamic limits and field reconstruction
│  └─ C. Quantum walks and quantum cellular automata (QCA) [Q/A]
│     ├─ Continuous-time, coined, and split-step quantum walks
│     ├─ Controlled Dirac-like or Schrödinger-like continuum limits
│     └─ Background gauge links or explicitly represented dynamical fields
│
├─ II. Linear systems and differential equations [Q/H/A]
│  ├─ A. Quantum linear system algorithms (QLSA) [Q]
│  │  ├─ HHL, block-encoding, and singular-value transformation methods
│  │  ├─ Variable-time and adiabatic linear-system algorithms
│  │  └─ Sparse quantum linear solvers [Q]
│  │     ├─ Sparse-access HHL and subsequent QLSA constructions
│  │     ├─ Nonzero-location and matrix-entry oracles for sparse matrices
│  │     ├─ Sparse block encodings with QSVT inverse or pseudoinverse filters
│  │     ├─ Hermitian embeddings for general non-Hermitian linear systems
│  │     ├─ Solution states |x⟩ ∝ A⁻¹|b⟩; singular systems require support conditions
│  │     └─ Sparsity, conditioning, encoding normalization, preparation, and readout costs
│  ├─ B. Linear differential equation algorithms [Q]
│  │  ├─ Taylor and spectral embeddings into linear systems
│  │  ├─ Time-dependent Dyson-series constructions and forcing terms
│  │  └─ Stability, conditioning, and solution-state recovery costs
│  ├─ C. Schrödingerisation [Q/A]
│  │  └─ Enlarged-space unitary evolution with auxiliary-coordinate recovery
│  ├─ D. Linear combination of Hamiltonian simulation (LCHS) [Q/A]
│  │  ├─ Dissipative linear evolution with suitable kernels and quadrature
│  │  └─ Hybrid oscillator–qubit implementations [Q/A; P26] (ref. 8)
│  ├─ E. Finite elements and preconditioning [Q/H]
│  │  ├─ Stiffness-matrix, load-vector, and boundary-data encodings
│  │  ├─ BPX multilevel preconditioners for specified elliptic problems
│  │  ├─ Additive Schwarz quantum preconditioning [Q; P26] (ref. 9)
│  │  └─ Sparse discretizations with explicit preconditioner encoding and application costs
│  └─ F. Lindbladian embeddings of classical linear ODEs [Q/A]
│     └─ Engineered open-system evolution with solution extraction
│
├─ III. Variational dynamics and thermal state preparation [Q/H/A; R]
│  ├─ A. Variational quantum simulation (VQS) [H]
│  │  ├─ Real-time and imaginary-time projected evolution
│  │  └─ Adaptive ansatz growth, residual control, and metric regularization
│  ├─ B. Quantum imaginary-time evolution (QITE) [H]
│  │  ├─ State-dependent unitary approximations to normalized evolution
│  │  └─ Local-domain accuracy and correlation-length constraints
│  ├─ C. Variational quantum linear solver (VQLS) [H]
│  │  ├─ Driven linear systems with prescribed sources and boundary data
│  │  └─ Variational sparse-system workflows [H]
│  │     ├─ Explicit operator decomposition or access circuits for residual estimation
│  │     ├─ Ansatz expressivity, conditioning, optimization, and measurement costs
│  │     └─ Matrix-entry sparsity does not guarantee a short Pauli decomposition
│  ├─ D. Variational differential-equation residual methods [H; R]
│  │  └─ Circuit representations with PDE and boundary-condition losses
│  ├─ E. Quantum Car–Parrinello molecular dynamics (QCPMD) [H]
│  │  └─ Classical nuclei coupled to parameterized electronic states
│  └─ F. Thermal field double (TFD) and variational thermal methods [Q/H/A]
│     ├─ QAOA-inspired alternating system and intersystem evolution
│     ├─ Engineered TFD parent Hamiltonians with specified approximation bounds
│     ├─ Variational quantum thermalizer (VQT): free-energy minimization
│     ├─ Explicit entropy evaluation, estimation, or tractable entropy models
│     ├─ Imaginary-time purification through QITE or variational approximations
│     ├─ Entanglement forging or circuit cutting with reconstruction overhead
│     ├─ Gauge-theory and Sachdev–Ye–Kitaev (SYK) model applications
│     ├─ Noise-aware compilation and engineered thermalization channels
│     └─ Product-spectrum ansätze, TPQ, and QMETTS as alternative routes
│
├─ IV. Hamiltonian evolution, scattering, and thermal response [Q/H/A]
│  ├─ A. Product formulas [Q]
│  │  ├─ Lie–Trotter, Strang, and higher even-order Suzuki formulas
│  │  └─ Randomized orderings and qDRIFT
│  ├─ B. Linear combination of unitaries (LCU) [Q]
│  │  └─ Truncated Taylor and time-dependent Dyson constructions
│  ├─ C. Qubitization and quantum signal processing (QSP) [Q]
│  │  ├─ Hamiltonian transformations; QSVT extends to general matrices
│  │  └─ Sparse-Hamiltonian simulation [Q]
│  │     ├─ Sparse row access and coherent matrix-entry queries
│  │     ├─ Quantum-walk or block-encoding constructions for exp(−iHt)
│  │     └─ Row sparsity, encoding normalization, evolution time, and precision costs
│  ├─ D. Interaction-picture simulation [Q]
│  │  └─ Exploit an efficiently simulated dominant Hamiltonian term
│  ├─ E. Scattering and resolvent methods [Q/H]
│  │  ├─ Incoming wave packets, evolution, and channel measurements
│  │  ├─ Phase shifts, amplitudes, and cross sections with normalization
│  │  └─ Sparse shifted-system resolvents using II.A; recovery of scales and overlaps
│  └─ F. Finite temperature correlators and spectroscopy [Q/H/A]
│     ├─ TFD or other thermal preparations for Kubo response functions
│     ├─ Dynamical correlations and spectral reconstruction
│     ├─ Out-of-time-order correlators (OTOCs) with specified operator ordering
│     └─ Schwinger–Keldysh observables using explicit contour-to-circuit mappings
│
├─ V. Quantum sampling and thermal ensembles [Q/H/A; R]
│  ├─ A. Quantum amplitude estimation (QAE) [Q]
│  │  └─ Expectation estimation with preparation and coherent-oracle costs
│  ├─ B. Quantum walks for Markov-chain tasks [Q]
│  │  └─ Mixing, hitting-time, and annealing methods with spectral assumptions
│  ├─ C. Quantum Metropolis sampling [Q]
│  │  └─ Energy resolution, acceptance procedures, and mixing-time analysis
│  ├─ D. Dissipative Gibbs preparation [Q/A]
│  │  └─ Quantum detailed balance with temperature and mixing constraints
│  ├─ E. Thermal typicality and ensemble methods [Q/H]
│  │  ├─ Thermal pure quantum (TPQ) state preparation and observable estimation
│  │  └─ Quantum minimally entangled typical thermal states (QMETTS)
│  └─ F. QPU-assisted classical Monte Carlo [H; R]
│     ├─ SQD or QSCI trial states supplied to classical phaseless AFQMC
│     └─ Quantum-generated proposals with explicit stationary-distribution checks
│
├─ VI. Open systems, trajectories, and stationary states [Q/H/A; R]
│  ├─ A. Quantum channel simulation [Q/A]
│  │  ├─ Stinespring dilation and Kraus representations
│  │  └─ Collision models, ancilla reset, and engineered environments
│  ├─ B. Lindblad or GKSL evolution [Q/A]
│  │  ├─ Product-formula, LCU, block-encoding, and randomized constructions
│  │  └─ Trajectory-based algorithms with class-specific query bounds
│  ├─ C. Monitored quantum trajectories [Q/H/A]
│  │  ├─ Jump and diffusive trajectories with appropriate measurements
│  │  └─ Classical averaging over records produced by quantum evolution
│  ├─ D. Stationary-state preparation [Q/H/A; R]
│  │  ├─ Dissipative relaxation or variational searches with physical-state checks
│  │  └─ Sparse Liouvillian linear-algebra routes [Q/H; R]
│  │     ├─ Trace-constrained systems or null-space searches for L|ρ⟩⟩ = 0
│  │     ├─ Singular and non-Hermitian operators require appropriate embeddings
│  │     └─ Uniqueness, conditioning, positivity, and physical-state recovery checks
│  ├─ E. Non-Markovian dynamics [Q/H/A]
│  │  ├─ Explicit bath registers, pseudomodes, and reaction coordinates
│  │  └─ Process-tensor representations with memory and circuit-cost accounting
│  └─ F. Representations and their physical meanings [Q/H/A]
│     ├─ Density matrix vectorization: |ρ⟩⟩ with Hilbert–Schmidt normalization
│     ├─ Canonical purification: |√ρ⟩⟩, whose reduced state is ρ
│     └─ Channel Choi operator: represents a map acting on a reference pair
│
├─ VII. Nonlinear, nonlocal, and stochastic differential equations [Q/H; R]
│  ├─ A. Carleman lifting with explicit truncation and convergence bounds [Q/H; R]
│  ├─ B. Koopman or Liouville encodings of observables or distributions [Q/H; R]
│  ├─ C. Fractional Laplacians and nonlocal kernels with efficient operator access [Q/H; R]
│  ├─ D. Wigner and phase-space evolution with explicit state encodings [Q/H; R]
│  ├─ E. Classical stochastic differential equations [Q]
│  │  └─ Linear Itô dilation and second-moment Lindblad constructions [Q; P26] (ref. 10)
│  ├─ F. Quantum stochastic differential equations [Q]
│  │  └─ System–field models with an explicit bath discretization or dilation
│  ├─ G. PDE uncertainty quantification using coherent solution oracles [Q/H; R]
│  └─ H. Adaptive meshes, hp elements, and nonlinear multiphysics [Q/H; R]
│     └─ Include refinement, coupling, stability, and field-readout costs
│
├─ VIII. Dirac equation and relativistic dynamics [Q/H/A; R]
│  ├─ A. H = c α·(p − qA) + βmc² + qφ for prescribed electromagnetic fields [Q/A]
│  ├─ B. Spinor encodings and controlled spatial discretization [Q/A]
│  ├─ C. Walk, QCA, product-formula, Fourier, and qubitization routes [Q/A]
│  ├─ D. Spectral filters or variational methods targeting a physical energy sector [Q/H]
│  ├─ E. Relativistic scattering and Klein-tunneling model studies [Q/A]
│  └─ F. Dirac–Maxwell and Dirac–Klein–Gordon coupling [Q/H/A; R]
│     └─ Pair creation and interacting quantum fields require many-body models
│
├─ IX. Schrödinger equation and nonrelativistic dynamics [Q/H/A; R]
│  ├─ A. First quantization: spatial grids, plane waves, and orbital bases [Q/A]
│  ├─ B. Second quantization: occupation registers and fermion-to-qubit maps [Q/A]
│  ├─ C. Real-time Hamiltonian simulation or variational evolution [Q/H/A]
│  ├─ D. QFT position–momentum transformations in encoded split-operator methods [Q]
│  ├─ E. Stationary eigenstates: H|ψ⟩ = E|ψ⟩; see X [Q/H/A]
│  ├─ F. Driven resolvents: [(E + iη)I − H]|x⟩ = |b⟩, η > 0 [Q/H]
│  │  └─ Sparse quantum linear solvers in II.A; η affects conditioning and resolution
│  └─ G. Nonlinear Schrödinger or Gross–Pitaevskii models [Q/H/A; R]
│     └─ Feedback, lifting, or a justified many-body mean-field limit
│
├─ X. Eigenstates and quantum subspace algorithms [Q/H/A]
│  ├─ A. Quantum phase estimation (QPE) and iterative spectral estimation [Q]
│  ├─ B. Adiabatic preparation, spectral filtering, QITE, and dissipative cooling [Q/H/A]
│  ├─ C. VQE, ADAPT-VQE, and variational quantum deflation (VQD) [H]
│  ├─ D. Quantum subspace expansion (QSE), quantum Krylov, and quantum Lanczos [Q/H]
│  │  └─ Sparse-operator quantum eigensolver workflows [Q/H]
│  │     ├─ Block-encoded or time-evolved Krylov-state preparation
│  │     ├─ Quantum estimation of projected Hamiltonian and overlap matrices [Q]
│  │     ├─ Classical projected-matrix diagonalization and regularization [C]
│  │     └─ Initial-state overlap, spectral gaps, subspace conditioning, and sampling costs
│  └─ E. Configuration sampling with classical diagonalization [H]
│     ├─ Quantum-selected configuration interaction (QSCI)
│     ├─ Sample-based quantum diagonalization (SQD)
│     ├─ Sample-based Krylov quantum diagonalization (SKQD)
│     ├─ Randomized propagation variants such as SqDRIFT
│     └─ Classical sparse eigensolvers on QPU-selected configuration subspaces [C]
│
├─ XI. Analog and mixed analog–circuit simulation [Q/A]
│  ├─ A. Native Ising, XY, and accessible Heisenberg-type spin models [A]
│  ├─ B. Optical-lattice Bose–Hubbard and Fermi–Hubbard models [A]
│  ├─ C. Rydberg, trapped-ion, superconducting, and other programmable simulators [A]
│  ├─ D. Adiabatic paths and accessible quantum-annealing dynamics [A]
│  └─ E. Native interaction blocks interleaved with gates [Q/A]
│     └─ Match the implemented Hamiltonian, controls, and physical observables
│
├─ XII. Bosonic, qudit, and continuous-variable simulation [Q/A]
│  ├─ A. Binary, unary, and problem-adapted bosonic occupation encodings [Q/A]
│  ├─ B. Photon, phonon, magnon, and molecular vibrational modes [Q/A]
│  ├─ C. Native oscillator dynamics [A]
│  │  ├─ Gaussian transformations for quadratic Hamiltonians
│  │  └─ Non-Gaussian resources for interacting or anharmonic models
│  ├─ D. Qudit encodings of multilevel systems and truncated gauge links [Q/A]
│  └─ E. Hybrid oscillator–qubit spin–boson and vibronic models [Q/A]
│
├─ XIII. Quantum field theory and lattice gauge models [Q/H/A; R]
│  ├─ A. Truncated scalar fields and interacting lattice fermions [Q/H/A]
│  ├─ B. Thirring, Gross–Neveu, and specified effective field theories [Q/H/A]
│  ├─ C. Abelian models: Schwinger, finite-group, and quantum-link formulations [Q/H/A]
│  ├─ D. Truncated non-Abelian SU(2) and SU(3) models [Q/A; R]
│  └─ E. Gauss-law constraints, string dynamics, confinement, and pair production [Q/H/A]
│     └─ Include gauge truncation, finite-volume, and continuum-limit errors
│
├─ XIV. Quantum magnetism and spintronics [Q/H/A/C; R]
│  ├─ A. Exchange, anisotropy, Zeeman, and Dzyaloshinskii–Moriya interactions [Q/H/A]
│  ├─ B. Spin transport, correlations, and dynamical structure factors [Q/H/A]
│  ├─ C. Spin–cavity, spin–vibration, and magnon–phonon models [Q/H/A]
│  ├─ D. Relaxation, pumping, and dephasing with specified jump operators [Q/H/A]
│  └─ E. Microscopic-to-macroscopic connections [H/C; R]
│     ├─ Landau–Lifshitz reductions under stated Lindblad mean-field assumptions [C; R]
│     └─ QPU-derived observables or parameters supplied to classical LLG models [H]
│
├─ XV. Classical waves, mechanics, and field propagation [Q/H; R]
│  ├─ A. Coupled harmonic oscillators encoded in quantum amplitudes [Q]
│  ├─ B. Wave, acoustic, and linear elastodynamic equations [Q/H]
│  ├─ C. Maxwell systems with sources, losses, and divergence constraints [Q/H; R]
│  └─ D. Vlasov and plasma models with specified self-consistent coupling [Q/H; R]
│     └─ Efficient data access and selected observable recovery define scope
│
├─ XVI. Embedding and multiscale workflows [H; R]
│  ├─ A. Quantum impurity solvers inside dynamical mean-field theory (DMFT) [H]
│  ├─ B. Density matrix embedding theory (DMET) [H]
│  ├─ C. Active-space, fragment, and quantum-mechanical/molecular-mechanical models [H]
│  ├─ D. Density-functional workflows containing a specified QPU subproblem [H]
│  ├─ E. Quantum local corrections for classical numerical homogenization [H; P26] (ref. 11)
│  └─ F. Domain decomposition with convergence and communication accounting [H; R]
│
├─ XVII. Observable extraction and inverse modelling [Q/H/A; R]
│  ├─ A. Local expectations, energies, occupancies, and correlations [Q/H/A]
│  ├─ B. Measurement grouping and applicable classical-shadow protocols [Q/H]
│  ├─ C. Green functions, spectral densities, and response coefficients [Q/H/A]
│  │  └─ Resolvent solution states require normalization and matrix-element estimation
│  ├─ D. Interferometric overlaps and transition amplitudes [Q/H]
│  ├─ E. Wigner or Husimi reconstruction with measurement-cost accounting [Q/H/A]
│  └─ F. Parameter inference driven by simulation observables [H; R]
│
├─ XVIII. Encoding, resources, and performance assessment [Q/H/A/C; R]
│  ├─ A. Initial states, boundary data, oracles, and block encodings [Q/H/A/C]
│  ├─ B. Sparsity, locality, symmetry sectors, and qubit tapering [Q/H/A/C]
│  │  └─ Sparse-solver access and resource assessment [Q/H/C]
│  │     ├─ Basis-dependent row/column sparsity versus Pauli-term sparsity and low rank
│  │     ├─ CSR/CSC classical storage requires an explicit coherent-access construction
│  │     ├─ Oracle synthesis, data loading, block normalization, and ancilla costs
│  │     ├─ Preconditioning benefits versus construction and application overhead
│  │     └─ Solution-state observables versus full classical-vector reconstruction
│  ├─ C. Tensor-network-derived state preparation and circuit compression [H]
│  ├─ D. Discretization, truncation, algorithmic, statistical, and hardware errors [Q/H/A/C]
│  ├─ E. Error mitigation or error-corrected logical implementation [Q/H/A]
│  ├─ F. Preparation, gate depth, ancillas, repetitions, and classical processing [Q/H/A/C]
│  └─ G. Matched comparisons of accuracy, observables, and full computational cost [Q/H/A/C]
│
└─ XIX. Classical comparisons and quantum-assisted interfaces [C/H]
   ├─ A. Classical quantum Monte Carlo [C]
   │  ├─ VMC, DMC, PIMC, PIGS, reptation, AFQMC, and series expansion
   │  └─ Fixed-node, constrained-path, phaseless, sign, and phase limitations
   ├─ B. Classical tensor networks [C]
   │  └─ MPS, DMRG, TEBD, TDVP, PEPS, TTN, and thermal-state methods
   ├─ C. Classical equations and transport [C]
   │  ├─ FEM, finite differences, spectral methods, multigrid, and Krylov solvers
   │  ├─ NEGF, TDDFT, particle-in-cell, LBM, and micromagnetic LLG
   │  └─ Classical sparse-solver baselines [C]
   │     ├─ Sparse LU/Cholesky and preconditioned CG/GMRES for appropriate systems
   │     ├─ Lanczos, Arnoldi, and shift-invert methods for sparse eigenproblems
   │     └─ CPU/GPU sparse solvers used in quantum-physics simulations remain classical
   ├─ D. Classical open-system simulation [C]
   │  └─ Master equations, Monte Carlo wavefunctions, and bath-memory methods
   └─ E. Quantum-assisted variants require an explicit QPU subroutine [H]
      └─ GPU execution alone belongs to the classical computational route
```

<a id="placement-guide"></a>

### Placement Guide

| Location | Label | Role | Connection |
| :--- | :---: | :--- | :--- |
| II.A | Q | Sparse quantum linear solvers | Directly prepares a normalized inverse-action or pseudoinverse-action state under the algorithm's access and spectral assumptions. |
| II.E | Q/H | Sparse discretization and preconditioning costs | Indirectly affects QLSA performance through conditioning, normalization, and encoding overhead. |
| III.C | H | Variational sparse-system workflows | Uses quantum residual estimation and classical optimization; sparsity alone does not establish a speedup. |
| IV.C | Q | Sparse-Hamiltonian simulation | Directly implements time evolution, providing a subroutine for some QLSAs and spectral methods. |
| IV.E and IX.F | Q/H | Sparse shifted-system resolvents | Directly applies a linear solver to an energy-shifted operator; broadening influences conditioning and spectral resolution. |
| VI.D | Q/H; R | Sparse Liouvillian stationary-state routes | Reformulates a stationary-state problem as constrained linear algebra or a null-space search, with additional physical-state checks. |
| X.D | Q/H | Sparse-operator quantum eigensolver workflows | Uses encoded operators or evolution to construct a measured subspace for eigenvalue estimation. |
| X.E | C | Sparse eigensolvers on sampled configurations | Classical diagonalization follows quantum configuration selection. |
| XVII.C | Q/H/A | Resolvent observable recovery | Converts normalized solution-state information into physical Green-function or response quantities using scales and overlaps. |
| XVIII.B | Q/H/C | Sparse access and resource assessment | Accounts for the representation and coherent access required by the solver. |
| XIX.C | C | Classical sparse-solver baselines | Supplies matched CPU/GPU linear-system and eigenproblem comparisons. |

[Back to contents](#contents)

---

<a id="hardware-readiness"></a>

## II. Hardware Readiness and Tiers

Readiness describes a particular implementation, target observable, and accuracy. The placements below are qualitative assessments based on the cited constructions and demonstrations, with scope specific to each algorithm and implementation.

<img width="2403" height="auto" alt="image" src="https://github.com/user-attachments/assets/adbb4c76-433b-43ee-9ffa-4cafaa4768b7" />

<a id="compact-readiness-summary"></a>

### Compact Readiness Summary

| Tier or category | Hardware and computational profile | Representative methods | Interpretation |
| --- | --- | --- | --- |
| **I. Demonstrations and bounded prototypes** | Accessible native interactions or circuits that fit a selected device's coherence, connectivity, and measurement budget. Qubit count alone provides insufficient guidance. | Quantum walks and QCA; short product-formula evolution; small VQE, VQS, QITE, VQLS, and TFD experiments; selected QLBM, channel, QSP, HHL, and sparse linear-system prototypes; native spin, atom, bosonic, and qudit simulations. | Establishes performance on the specified instance. Analog simulators reach many-body regimes beyond the few-qubit circuit setting. Small compiled sparse examples do not establish scalable matrix access or a useful speedup. |
| **II. Structured hybrid workflows** | Repeated circuit execution with classical optimization, subspace diagonalization, sampling, embedding, or feedback. Error mitigation and early logical implementations are possible choices. | QSCI/SQD with classical sparse diagonalization; measured quantum Krylov and suitable SKQD variants; variational sparse linear systems; QPU-derived AFQMC trials; variational thermal pipelines and QMETTS; impurity solvers; QCPMD; multiple-circuit QLBM; selected FEM, nonunitary-evolution, and quantum local-correction workflows. | Practical feasibility requires control of circuit depth, repetitions, classical workload, and model error. Classical assembly, optimization, sparse factorization, or diagonalization remains C within the overall H workflow. Hybrid execution alone supplies no speedup guarantee. |
| **III. Scalable coherent algorithms, generally oriented toward fault tolerance** | Sustained accurate evolution, implementable oracles or block encodings, controlled state preparation, and enough logical resources for the requested accuracy. | Precision QPE; sparse-Hamiltonian simulation and qubitization; QSP/QSVT inverse and pseudoinverse filters; sparse-access QLSA; LDE solvers; LCHS; circuit-based Schrödingerisation; suitable preconditioned FEM; sparse shifted-system resolvents; block-encoded quantum Krylov/Lanczos; coherent QAE; Gibbs preparation; general Lindblad and many-body scattering algorithms. | Error correction enables deeper reliable computation. Useful advantage still requires favourable input, conditioning, spectral, and output assumptions. Oracle construction, normalization, preconditioning, and reconstruction belong in the full cost. |
| **R. Construction-dependent research directions** | Resources and maturity assessed for the exact proposal. Some admit small prototypes; others have theoretical or classical numerical studies. | General quantum AMR and hp-FEM; nonlinear multiphysics; broad RT-TDDFT interfaces; unspecified domain decomposition; fractional and phase-space solvers; sparse Liouvillian stationary-state proposals without a complete recovery construction; unresolved sparse-access or preconditioning interfaces. | Preserve the distinction between a physical target, a quantum algorithm, and a hardware demonstration. Specified preprint constructions are assessed within their stated scope rather than automatically assigned R. |
| **C. Classical comparisons and support** | CPUs, GPUs, distributed memory, or classical sampling and tensor contractions. | Classical QMC, MPS/PEPS, DMRG, sparse LU/Cholesky, CG/GMRES, Lanczos/Arnoldi, FEM, NEGF, TDDFT, LLG, and classical quantum trajectories. | These methods supply baselines and hybrid components. A separately specified QPU subroutine establishes quantum assistance. GPU execution of a sparse quantum-physics model remains classical computation. |

<a id="extended-readiness-analysis"></a>

### Extended Readiness Analysis

<details markdown="1">
<summary><strong>Tier I: Demonstrations and Bounded Prototypes</strong></summary>

Designed for experiments and small implementation studies, this tier determines hardware suitability based on the circuit or native interaction model. Although the rows categorize suitable classes of bounded tasks, this does not guarantee that every variant has a corresponding hardware demonstration or universal processor support.

| Category | Label | Techniques and bounded targets | Scope and practical constraint |
| --- | :---: | --- | --- |
| **A. Walks, automata, and lattice transport** | Q/H/A | Split-step walks, graph Hamiltonians, QCA, transport with prescribed gauge links, and selected QLBM or QLGA kernels. | Compile streaming and collision separately. Fluid behaviour requires a demonstrated continuum limit, boundary treatment, and closure. Large classical benchmark grids in a quantum algorithm paper are separate from QPU execution. [QLBM construction](https://arxiv.org/abs/2502.16568). |
| **B. Short Hamiltonian evolution** | Q/A | Product-formula spin dynamics, small encoded Schrödinger grids, Dirac wave packets, tunneling observables, and bounded sparse-Hamiltonian circuits. | Accessible evolution time and error set scope. Sparse graph degree does not by itself determine compiled gate depth. Dirac spectral studies require the intended energy sector and control of lattice artifacts. |
| **C. Variational states and dynamics** | H | VQE, real-time VQS, variational imaginary time, and local-domain QITE on manageable instances. | Ansatz error, metric conditioning, correlations, and measurement cost govern scaling. QITE becomes expensive as required domains grow. [QITE experiments and construction](https://arxiv.org/abs/1901.07653). |
| **D. Small linear-system and matrix-function experiments** | Q/H | VQLS on encoded sparse systems, compiled HHL demonstrations, and low-degree QSP/QSVT filters with accessible encodings. | Count source-state preparation, operator-access circuits, controlled operations, and postselection or amplification. Matrix-entry sparsity does not guarantee a short Pauli decomposition. VQLS prepares a solution-state approximation; stationary eigenpairs require eigenstate algorithms. [VQLS](https://quantum-journal.org/papers/q-2023-11-22-1188/), [HHL](https://arxiv.org/abs/0811.3171). |
| **E. Thermal preparation** | Q/H/A | QAOA-inspired TFD preparation, variational TFD parent-Hamiltonian methods, VQT, and product-spectrum ansätze on selected systems. | Temperature alone gives insufficient readiness information. Entropy treatment, ansatz expressibility, and purification cost are essential. The execution label follows the actual preparation protocol. [Trapped-ion TFD experiment](https://arxiv.org/abs/1906.02699), [VQT](https://arxiv.org/abs/1910.02071). |
| **F. Open-system channels** | Q/H/A | Ancilla-assisted damping or dephasing, collision models, reset protocols, and monitored jump trajectories. | Requires the relevant preparation, measurement, and reset capabilities. A classical MCWF calculation belongs to C unless quantum hardware performs the trajectory evolution. Sparse storage of a classical Liouvillian does not establish a quantum channel implementation. |
| **G. Analog, bosonic, and qudit simulation** | Q/A | Native spin interactions, optical-lattice models, oscillator dynamics, and selected truncated gauge models on multilevel hardware. | Judge native Hamiltonian control, calibration, truncation, and observable access. Universal circuit depth is an incomplete metric for analog hardware. [Qudit gauge-theory experiment](https://www.nature.com/articles/s41567-025-02797-w). |
| **H. Amplitude-estimation experiments** | Q/H | Maximum-likelihood or iterative amplitude estimation with bounded amplification depth. | Removing QPE reduces some resources, but repeated Grover operations still produce deep circuits. A depth cap changes the attainable precision and sampling tradeoff. [Iterative QAE](https://arxiv.org/abs/1912.05559). |

</details>

<details markdown="1">
<summary><strong>Tier II: Structured Hybrid Workflows</strong></summary>

In this tier, the quantum processor supplies states, samples, or observables to a classical calculation. The complete workflow includes every repetition, optimization step, and classical solve. Error mitigation, parallel circuit execution, and error correction address different limitations and should be costed separately.

| Category | Label | Techniques and targets | Scope and practical constraint |
| --- | :---: | --- | --- |
| **A. Configuration sampling and diagonalization** | H | QSCI, SQD, and extensions for excited states; quantum-generated configurations define a classically diagonalized subspace, using sparse eigensolvers where the projected representation supports them. | Match classical subspace size, basis, and accuracy against classical selection methods. Sampling quality, matrix assembly, sparse storage, and classical diagonalization may dominate. Quantum selection does not guarantee that the reduced matrix remains sparse. [Extended SQD](https://arxiv.org/abs/2411.00468), [benchmarking considerations, P26](https://arxiv.org/abs/2608.11569). |
| **B. Quantum subspace dynamics** | H | Quantum subspace expansion, measured quantum Lanczos, quantum Krylov, and suitable SKQD or SqDRIFT variants. | Time-evolution depth, initial overlap, projected Hamiltonian and overlap conditioning, and state concentration determine feasibility. The projected matrices need not inherit the original operator's sparsity. Some block-encoded Krylov constructions naturally move into III. [Randomized Krylov diagonalization](https://arxiv.org/abs/2508.02578), [quantum Lanczos](https://quantum-journal.org/papers/q-2023-05-23-1018/). |
| **C. QPU-assisted classical QMC** | H | SQD or QSCI wavefunctions supplied as trials to classical phaseless AFQMC; other explicit quantum-generated trial constructions. | Retain the classical approximation and sampling costs. Generic QAE-driven DMC acceleration requires its own coherent algorithm. [SQD-assisted AFQMC](https://arxiv.org/abs/2503.05967). |
| **D. Thermal state and response pipelines** | H | TFD-QITE, variational purification, VQT, QMETTS, and quantum preparation of thermal typical states followed by correlator estimation. | Include preparation at each temperature, entropy evaluation where needed, correlation time, and sampling variance. A single pure typical state approximates suitable thermal observables under typicality assumptions. [QITE and thermal ensembles](https://arxiv.org/abs/1901.07653). |
| **E. Entanglement forging and circuit cutting** | H | Smaller circuits reconstruct selected TFD or many-body quantities; parent-Hamiltonian optimization may use the reconstructed expectations. | Width reduction trades against classical reconstruction and measurements. An N-qubit device reconstructing properties of a 2N-qubit state carries this overhead. [TFD forging](https://arxiv.org/abs/2311.10566). |
| **F. Lattice kinetics and variational PDE workflows** | H | Multiple-circuit QLBM, measurement and feedback between steps, and variational PDE residual methods. | Reinitialization, field reconstruction, boundary handling, and accumulated error may dominate. Nonlinear dynamics requires an explicit closure or feedback model. |
| **G. Structured finite-element prototypes** | Q/H | VQLS-based sparse discretized solves or implementable preconditioned FEM circuits on specified problems. | Mesh dimensions alone fail to determine resources. Include matrix encoding, condition number, coefficient access, source-state preparation, and requested solution functionals. A classical sparse mesh matrix requires an explicit coherent-access or operator-decomposition construction. [BPX-preconditioned FEM](https://arxiv.org/abs/2403.19512). |
| **H. Embedding and coupled molecular calculations** | H | Quantum impurity solvers inside DMFT, DMET or active-space embedding; QCPMD with classical nuclear motion. | Accuracy of bath fitting, forces, self-consistency, and repeated quantum measurements limits scope. QCPMD device suitability requires a compiled instance. Unspecified extensions receive an additional R marker. [QCPMD](https://arxiv.org/abs/2212.11921). |
| **I. Open spins, spectroscopy, and device parameters** | Q/H/A | Monitored trajectories, small explicit bath models, dynamical correlations, and quantum-derived parameters for classical spin or device models. | Bath-memory size and observation time determine resources. A microscopic-to-LLG reduction requires specified physical assumptions; the measurement protocol determines whether QPE is useful. Classical LLG propagation remains C inside a workflow supplied with quantum-derived parameters. [LL reduction](https://arxiv.org/abs/2406.10613). |
| **J. Nonunitary-evolution prototypes** | Q/H/A | Small, explicitly compiled Schrödingerisation or LCHS constructions; auxiliary oscillator implementations where supported. | Scope must include auxiliary-state preparation, truncation, recovery probability, and hardware controls. Large accurate circuit instances generally fall in III. Oscillator–qubit processing alone does not require H; an essential classical optimization or embedding loop does. [Schrödingerisation](https://arxiv.org/abs/2212.13969), [oscillator–qubit LCHS, P26](https://arxiv.org/abs/2605.10708v3). |
| **K. Hybrid sparse linear-system and response workflows** | H | VQLS applied to encoded sparse discretizations; classical assembly and essential optimization with quantum residual estimation; specified variational shifted-system solves for selected response quantities. | Track row/column sparsity separately from the measured operator decomposition. Include conditioning, residual-to-solution error bounds, optimizer iterations, repetitions, and norm/overlap recovery. A complete classical field requires reconstruction in addition to quantum residual estimation. [VQLS](https://quantum-journal.org/papers/q-2023-11-22-1188/). |
| **L. Quantum local corrections in sparse multiscale problems** | H; P26 | Classical coarse-scale solution with specified quantum fine-scale corrections in numerical homogenization. | Assess local operator access, coefficient contrast, preparation, selected measurements, and repeated local calls. Generalization to arbitrary nonlinear or coupled multiscale models requires a separate construction. The cited numerical study classically simulates the local quantum solver. [Quantum-enhanced homogenization](https://arxiv.org/abs/2603.28521v2). |

</details>

<details markdown="1">
<summary><strong>Tier III: Scalable Coherent Algorithms Oriented Toward Fault Tolerance</strong></summary>

This tier identifies implementations whose precision or evolution demands typically exceed noisy circuit budgets. Logical qubits and error correction enable reliable depth, but they leave algorithmic assumptions, data access, and readout costs intact. Large ancilla registers are method dependent; some algorithms trade width for longer execution.

| Category | Label | Techniques and targets | Conditions for useful scaling |
| --- | :---: | --- | --- |
| **A. Spectral estimation and state preparation** | Q | Precision QPE, iterative spectral estimation, polynomial filtering, and adiabatic preparation. | Count state overlap, spectral gaps, controlled evolution, and target energy precision. Difficult ground-state preparation may dominate all subsequent calculations. |
| **B. Hamiltonian simulation, including sparse operators** | Q | Sparse-Hamiltonian quantum-walk or block-encoding constructions; qubitization, QSP, LCU, interaction-picture methods, and suitable high-order product formulas for electronic, spin, bosonic, Dirac, or Schrödinger models. | Resources follow the access model, operator normalization, row sparsity where applicable, simulation time, precision, and encoding. Sparse-Hamiltonian evolution implements a propagator rather than directly solving a stationary linear system. Product formulas also have small demonstrations in I. [Qubitization](https://quantum-journal.org/papers/q-2019-07-12-163/), [QSVT primitives](https://arxiv.org/abs/1806.01838), [first-quantized chemistry](https://www.nature.com/articles/s41534-019-0199-y). |
| **C. Sparse quantum linear systems and differential equations** | Q | Sparse-access HHL and subsequent QLSAs; sparse block encodings with QSVT inverse or pseudoinverse filters; Taylor or spectral LDE embeddings; time-dependent Dyson constructions. | Specify coherent nonzero-location and matrix-entry queries or another explicit block encoding. Include row/column sparsity, encoding normalization, condition number, source preparation, precision, stability, forcing, and solution recovery. Non-Hermitian systems require an appropriate construction; singular systems require specified pseudoinverse and support treatment. The usual output is a normalized solution state or selected functionals. [HHL](https://arxiv.org/abs/0811.3171), [QSVT](https://arxiv.org/abs/1806.01838), [Berry–Childs–Ostrander–Wang LDE algorithm](https://arxiv.org/abs/1701.03684). |
| **D. Nonunitary linear dynamics** | Q/A | LCHS, circuit-based Schrödingerisation, specified oscillator–qubit implementations, and engineered Lindbladian ODE embeddings. | Efficient generator access, suitable stability assumptions, auxiliary construction, truncation, and recovery probability are central. Sparse generators still require their access costs to be included. Native auxiliary oscillators require compatible controls and non-Gaussian resources where specified. [LCHS](https://link.springer.com/article/10.1007/s00220-025-05509-w), [ODEs via Lindbladians](https://link.aps.org/doi/10.1103/cvl9-97qg), [oscillator–qubit construction, P26](https://arxiv.org/abs/2605.10708v3). |
| **E. Preconditioned sparse FEM and structured PDEs** | Q | Block-encoded sparse discretizations with BPX or other explicitly constructed quantum preconditioners; the specified two-level Additive Schwarz finite-element construction. | Preconditioning must improve the full encoded solve, including construction, normalization, source transformation, application cost, and observable recovery. A preconditioner or its inverse need not preserve the original matrix sparsity. Classical V-cycles or BiCGSTAB require separate quantum constructions. [Quantum FEM](https://arxiv.org/abs/2403.19512), [Additive Schwarz construction, P26](https://arxiv.org/abs/2605.26090). |
| **F. Coherent expectation estimation** | Q | QAE and quantum Monte Carlo integration around reversible model and payoff circuits. | Quantum sampling advantages require efficient state preparation and sufficiently accurate coherent amplification. Complete output fields may remove the intended benefit. [Quantum Monte Carlo acceleration](https://arxiv.org/abs/1504.06987). |
| **G. Thermal and Gibbs algorithms** | Q/H/A | Imaginary-time polynomial filters, purified Gibbs preparation, quantum Metropolis, and detailed-balanced dissipative Gibbs samplers with explicit implementations. | Temperature, mixing time, overlap, filter success probability, and energy resolution may dominate. Efficient simulation of a thermalizing generator alone supplies no general rapid-mixing guarantee. Native thermalization belongs to A only when the implemented dynamics and target ensemble are specified. [Quantum Metropolis](https://arxiv.org/abs/0911.3635), [detailed-balanced Gibbs construction](https://arxiv.org/abs/2311.09207). |
| **H. General open-system simulation** | Q | Accurate Lindblad evolution using channel, dilation, LCU, block-encoding, or trajectory constructions; explicit environment simulation for memory effects; specified linear Itô dilation and second-moment constructions. | Retain the access model and operator normalization in complexity statements. Class-specific additive jump-operator query bounds are separate from full gate cost. Sparse Liouvillian representation does not itself provide physical stationary-state preparation. [Trajectory-based algorithms](https://quantum-journal.org/papers/q-2026-04-13-2063/), [linear Itô construction, P26](https://arxiv.org/abs/2601.05928v4). |
| **I. Many-body scattering and field theory** | Q | Prepared incoming states, interacting evolution, channel amplitudes, gauge-constrained models, and multiparticle production. | Include state preparation, finite volume, field truncation, continuum accuracy, and final-state measurements. Smaller analog or qudit gauge experiments belong to I. |
| **J. Long-time and high-precision response** | Q/H | Spectral functions, finite temperature Kubo response, and OTOCs using appropriate thermal states and operator orderings. | Spectral resolution, correlator amplitude, observation time, and measurement protocol set requirements. QPE is optional for suitable time-domain approaches. Resolvent-based response additionally requires the checks in III.M. |
| **K. Encoded classical mechanics and waves** | Q | Coupled harmonic oscillators and compatible wave-equation mappings to Hamiltonian evolution. | Advantage concerns specified outputs with efficient initial-state and coefficient access. General nonlinear mechanics or arbitrary field readout needs separate analysis. [Harmonic oscillator algorithm](https://arxiv.org/abs/2303.13012). |
| **L. Block-encoded quantum Krylov and sparse-operator eigensolvers** | Q/H | Quantum Krylov-state construction, block-encoded quantum Lanczos, and specified filtered subspace methods for accessible sparse operators. | Include initial-state overlap, operator normalization, spectral gaps, Krylov dimension, subspace conditioning, measurement cost, and any classical projected diagonalization. Input sparsity does not imply sparse eigenvectors or sparse projected matrices. An eigenproblem differs from a driven linear-system solve. [Quantum Lanczos](https://quantum-journal.org/papers/q-2023-05-23-1018/), [measurement-efficient Krylov analysis](https://quantum-journal.org/papers/q-2024-08-13-1438/). |
| **M. Sparse shifted-system resolvents and response extraction** | Q/H | Quantum linear-system constructions for energy-shifted Hamiltonians, selected Green-function matrix elements, and driven-response solution states. | Define the source and shifted operator, including positive spectral broadening. Include non-Hermitian treatment, conditioning, block normalization, solution norms, and overlap estimation. Narrower broadening improves nominal resolution but may increase conditioning and inverse-approximation cost. A normalized solution state alone does not supply an absolutely normalized Green function. [HHL solution-state framework](https://arxiv.org/abs/0811.3171), [QSVT inverse/pseudoinverse primitives](https://arxiv.org/abs/1806.01838). |

</details>

<details markdown="1">
<summary><strong>Category R: Construction-Dependent Research Directions</strong></summary>

R applies to a missing problem-specific mapping, access construction, recovery procedure, or resource analysis. Specified Additive Schwarz, linear Itô, oscillator–qubit, and numerical-homogenization constructions are assessed in their applicable rows above.

| Category | Label | Techniques or unresolved targets | Required construction and assessment |
| --- | :---: | --- | --- |
| **A. General nonlinear and adaptive field solvers** | Q/H; R | General quantum AMR, hp-FEM, nonlinear multiphysics, and unrestricted Carleman, Koopman, or Liouville extensions. | Specify closure, truncation, refinement, boundary treatment, stability, coupling, coefficient access, and output recovery. |
| **B. General fractional and phase-space solvers** | Q/H; R | Nonlocal kernels, fractional operators, Wigner evolution, and target-specific stochastic extensions without a complete efficient encoding. | Nonlocality may produce dense matrices. Establish an efficient sparse, structured, low-rank, or other explicit access construction rather than assume sparsity. |
| **C. Sparse Liouvillian stationary-state proposals** | Q/H; R | Null-space searches or trace-constrained linear-system formulations for stationary density operators where the complete quantum preparation/recovery route remains unspecified. | Treat singularity and non-Hermiticity; establish uniqueness or specify the stationary sector. Include conditioning, Hermiticity, trace normalization, positivity, and usable physical-state recovery. A normalized vectorized density operator is distinct from a purification or a directly prepared mixed state. |
| **D. Sparse access and quantum preconditioning interfaces** | Q/H; R | Arbitrary CSR/CSC-to-oracle loading, proposed quantum use of classical preconditioners, or distributed sparse queries without an implemented access model. | Specify reversible index/value access, loading and coherent memory costs, block normalization, transformed sources, inverse/application costs, and communication overhead. |
| **E. Broad electronic-structure and multiscale interfaces** | H; R | Broad RT-TDDFT interfaces, general DFT subproblem proposals, unspecified domain decomposition, and nonlinear homogenization extensions. | Identify the QPU subproblem and its convergence, accuracy, measurement, and communication requirements. A classical DFT or sparse local solver alone remains C. |
| **F. Full-field and distributed solution recovery** | Q/H; R | Proposals requiring arbitrary full classical fields or coherent solver registers distributed across incompatible hardware modules without a complete protocol. | Include reconstruction, repeated source calls, coherent transfer or teleportation, code mapping, waiting errors, and matched classical outputs. Coherent storage and classical RAM have distinct roles. |

</details>

<details markdown="1">
<summary><strong>Category C: Classical Comparisons and Support</strong></summary>

| Family | Label | Examples | Role in a quantum workflow |
| --- | :---: | --- | --- |
| **Quantum Monte Carlo executed classically** | C | VMC, DMC, PIMC, PIGS, reptation, AFQMC, and stochastic series expansion. | Baseline calculations or sampling with QPU-derived trials. Sign, phase, and constraint approximations retain their method-specific scope. |
| **Classical tensor networks** | C | MPS, PEPS, TTN, DMRG, TEBD, TDVP, and thermal tensor methods. | Reference calculations, state preparation, compression, or classical subspace work. |
| **Classical sparse linear-system solvers** | C | Sparse LU; Cholesky for suitable positive-definite matrices; preconditioned CG for Hermitian positive-definite systems; GMRES and other appropriate nonsymmetric methods. | Full-vector baselines, classical preprocessing, coefficient solves, resolvents, and reduced-system components. Include factorization fill-in, preconditioner setup, memory, and repeated-source amortization. |
| **Classical sparse eigensolvers** | C | Lanczos, Arnoldi, shift-invert methods, and suitable iterative subspace solvers. | Spectral baselines and classical diagonalization within QSCI/SQD or measured quantum Krylov workflows. Classical diagonalization remains C even when a QPU selects the subspace. |
| **Classical field and transport solvers** | C | FEM, multigrid, Krylov methods, LBM, NEGF, TDDFT, particle-in-cell, and micromagnetic LLG. | Full classical field solutions, embedding environments, or multiscale coupling. Sparse quantum-physics models solved on GPUs remain within C. |
| **Classical open-system solvers** | C | Master-equation integration, sparse trace-constrained steady-state solves, MCWF trajectories, stochastic Schrödinger integration, and bath-memory methods. | Reference trajectories, stationary states, and reduced-state dynamics. GPU acceleration changes execution speed within the classical route. |
| **Classical storage, compilation, and control** | C | CSR/CSC matrices, meshes, sampled configurations, optimizers, decoders, circuit compilation, and measurement-record processing. | Supplies data and classical support. Coherent sparse-oracle access requires an additional implemented construction; ordinary classical RAM does not store amplitude-encoded quantum solution states. |

</details>

<a id="sparse-solver-readiness-checks"></a>

### Sparse-Solver Readiness Checks

| Check | Quantity or construction to specify | Consequence for readiness |
| --- | --- | --- |
| **Mathematical task** | Driven linear solve, Hamiltonian evolution, eigenproblem, resolvent, or stationary-state problem. | Determines which algorithm family applies; these tasks are not interchangeable. |
| **Operator representation** | Matrix dimension, basis, row/column sparsity, Pauli-term count, symmetry reduction, or low-rank structure. | Different representations produce different query and compilation costs. |
| **Coherent access** | Nonzero-location and matrix-entry oracles, reversible coefficient generation, or another explicit block encoding. | Establishes whether the assumed quantum input model is available. Classical CSR/CSC storage alone is insufficient. |
| **Source or initial state** | Preparation cost, source norm, initial overlap, and any coherent reuse. | Input preparation may dominate the nominal solver cost. |
| **Spectral properties** | Condition number or retained singular-value threshold, eigenvalue gaps, stability, and operator normalization. | Governs inverse/filter degree, amplification, convergence, or evolution requirements. |
| **Preconditioning** | Construction, application, normalization, source transformation, and recovery costs. | A lower condition number is useful only when the full encoded workload benefits. |
| **Output and physical meaning** | Selected observables, samples, a coherent state, absolute response scales, or a complete classical vector. | Determines measurement, norm recovery, reconstruction, and physical-state requirements. |
| **Complete implementation** | Gates, depth, logical qubits, ancillas, correction cycles, repetitions, interfaces, and classical processing. | Determines whether the specified instance fits I, II, or III, or retains an unresolved R component. |
| **Matched comparison** | Identical accuracy and output task, including classical setup and quantum loading. | Separates executable implementations from substantiated claims of quantum advantage. |

[Back to contents](#contents)

---

<a id="resource-metrics"></a>

## III. Resource Metrics and Qubit Scaling

<a id="error-correction-footprint"></a>

### Error Correction Footprint

| Code distance $d$ | Physical data qubits | Syndrome-measurement qubits | Patch footprint per logical qubit | Leading approximation |
| --- | ---: | ---: | ---: | ---: |
| 7 | 49 | 48 | **97** | $2d^2=98$ |
| 13 | 169 | 168 | **337** | $2d^2=338$ |

<a id="estimated-qubit-requirements"></a>

### Estimated Qubit Requirements

<a id="physical-circuit-qubit-requirements"></a>

#### A. Bounded Physical-Circuit and Analog Examples

| Method | Register rule | Representative instance | Interpretation |
| --- | --- | --- | --- |
| **Coined quantum walk** | $\lceil\log_2N\rceil+\lceil\log_2c\rceil+a$, for $N$ position labels and $c$ coin states. | $N=1,024$, $c=2$, and one explicitly allocated work qubit gives $10+1+1=\mathbf{12}$. | The extra work qubit is an example choice. A continuous-time walk can omit the coin; gate implementation determines workspace. |
| **D2Q5 binary occupancy representation** | $5M^2+a$ for an $M\times M$ lattice with five binary occupancy channels per site. | $M=8$ gives **320 data qubits**, plus work qubits. | Each channel has binary occupation. Representing a general numerical population requires a different encoding. Width alone establishes no near-term executability. |
| **D2Q5 stored numerical populations** | $5M^2b+a$ for $b$ binary digits per population, with sign or format bits included in $b$. | $M=8$, $b=8$ gives **2,560 data qubits**, plus arithmetic workspace. | The chosen numerical representation and precision determine $b$; classical floating-point populations are separate from binary occupations. |
| **D2Q5 amplitude encoding** | $\lceil\log_2(5M^2)\rceil+a$ for a packed index; separate position and velocity registers can introduce padding. | $M=8$: **9 data qubits**; $M=16$: **11**; $M=32$: **13**. | These examples assume a single normalized population vector. Preparation, collision, boundaries, normalization recovery, and measurements add costs. [QLBM formulation](https://arxiv.org/abs/2502.16568). |
| **VQE, VQS, or QITE** | $n_{\mathrm{state}}+a$. Direct occupation encoding uses one qubit per active spin orbital before reductions. | Four active spin orbitals use **4 data qubits**; twelve active spin orbitals use **12**. | Active-space selection, frozen orbitals, and symmetry tapering change width. The algorithm name alone fixes neither the state width nor the ancillas. |
| **Small HHL construction** | $n+r+1+a$, where $n=\lceil\log_2N\rceil$, $r$ is the chosen QPE-register width, and one qubit flags the inversion rotation. | $N=4$, $r=3$ gives **6 qubits plus further workspace**. | This is a selected register layout. Energy precision and controlled arithmetic must be adequate for the actual matrix and tolerance. |
| **Iterative or likelihood-based QAE** | Width of the prepared state, objective encoding, and work registers. The number of additional QPE-register qubits can be zero. | An explicitly chosen 8-qubit input register plus one objective flag gives **9 qubits plus workspace**. | Omitting a QPE register leaves state-preparation and repeated amplification costs. The objective may instead occupy an existing register. [IQAE](https://arxiv.org/abs/1912.05559). |
| **One-qubit amplitude-damping channel** | One system qubit plus one environmental qubit for its rank-2 dilation. | **2 qubits** for the elementary channel circuit. | Repeated steps can reuse a reset environment qubit if the physical model and hardware permit it. |
| **General channel dilation** | $n_{\mathrm{sys}}+\lceil\log_2r_K\rceil+a$, for a channel with Kraus rank $r_K$ and a pure initial environment. | A one-qubit rank-4 channel uses **3 qubits before gate workspace**. | Kraus rank determines this minimal environment dimension. Retained memory may require additional environment registers. |
| **Direct TFD purification** | $2n_{\mathrm{sys}}+a$. | A 6-qubit target system uses **12 purification qubits**, plus work registers. | The reference register carries the purification. Entanglement forging reconstructs selected quantities with different classical and measurement costs. [TFD preparation](https://arxiv.org/abs/1906.02699). |
| **Native spin or qudit simulation** | Number of physical sites and internal levels used by the implemented model. | A 64-site array with one two-level spin per site uses **64 spin carriers**. | Native analog evolution has its own control and measurement budget; logarithmic amplitude encoding describes a different representation. |

<a id="hybrid-qubit-requirements"></a>

#### B. Structured Hybrid Workflows

| Method and chosen instance | Algorithmic qubits | Patch-only physical qubits at $d=7$ | Patch-only physical qubits at $d=13$ | Unresolved resources |
| --- | ---: | ---: | ---: | --- |
| **Multiple-circuit D2Q5 amplitude encoding, $M=16$** | $11+a$ | $1,067+97a$ | $3,707+337a$ | Workspace, state reconstruction, repetitions, and concurrent circuit copies. Sequential kernels can reuse qubits. |
| **VQLS/FEM with $N=1,024$ algebraic degrees of freedom** | $10+a$ | $970+97a$ | $3,370+337a$ | Cost-function circuits, operator access, state preparation, and conditioning. Node count and algebraic degrees of freedom can differ. |
| **QSCI/SQD with 24 active spin orbitals** | $24+a$ | $2,328+97a$ | $8,088+337a$ | Ansatz workspace and classical subspace diagonalization. Classical subspace size is separate from qubit count. |
| **Quantum Krylov or SKQD on 24 state qubits** | $24+a$ | $2,328+97a$ | $8,088+337a$ | Evolution depth, overlap estimation or configuration sampling, and classical conditioning. |
| **Direct TFD pipeline for a 12-qubit system** | $24+a$ | $2,328+97a$ | $8,088+337a$ | Preparation, entropy evaluation where used, temperature-dependent difficulty, and correlator circuits. |
| **QMETTS on a 12-qubit system** | $12+a$ | $1,164+97a$ | $4,044+337a$ | Imaginary-time construction and ensemble sampling; a second full purification register is absent in this chosen route. |
| **Impurity solver with 4 impurity and 8 bath spin orbitals** | $12+a$ | $1,164+97a$ | $4,044+337a$ | Bath fitting, observable measurement, and repeated self-consistency. |
| **QCPMD with 12 active electronic spin orbitals** | $12+a$ | $1,164+97a$ | $4,044+337a$ | Electronic ansatz and force estimation; the nuclear coordinates are classical in this workflow. |
| **Quantum-generated AFQMC trial with 24 active spin orbitals** | $24+a$ | $2,328+97a$ | $8,088+337a$ | Trial-generation procedure and classical AFQMC cost; direct overlap-based variants can require different circuits. |

<a id="scalable-qubit-requirements"></a>

#### C. Scalable Coherent Algorithms and Fault-Tolerant Resource Models

| Method and representative encoding | Algorithmic logical width | Patch-only physical qubits at $d=7$ | Patch-only physical qubits at $d=13$ | Additional resource drivers |
| --- | ---: | ---: | ---: | --- |
| **QLSA for $N=2^{20}$ unknowns** | $20+a$ | $1,940+97a$ | $6,740+337a$ | State preparation, block encoding, inversion/filter precision, conditioning, and reversible arithmetic. |
| **LDE solution with $N=2^{20}$ components** | $20+a$ | $1,940+97a$ | $6,740+337a$ | A history-based construction may add a time register, polynomial-order registers, forcing, and solver workspace within $a$. |
| **Spinless one-particle Schrödinger state on $256^3$ grid points** | $24+a$ | $2,328+97a$ | $8,088+337a$ | Kinetic and potential access, simulation primitives, and boundary operations. |
| **Four-component one-particle Dirac spinor on $256^3$ grid points** | $26+a$ | $2,522+97a$ | $8,762+337a$ | Two spinor-component qubits supplement the 24 position qubits; spectral-sector and discretization controls are required. |
| **Occupation encoding of 64 active spin orbitals** | $64+a$ | $6,208+97a$ | $21,568+337a$ | Hamiltonian factorization, selection registers, arithmetic, and state preparation. Symmetry reductions change the data count. |
| **Conventional QPE on an $n$-qubit state using an $r$-qubit phase register** | $n+r+a$ | $97(n+r+a)$ | $337(n+r+a)$ | Controlled evolution and phase precision. Iterative versions can reuse a smaller phase register with different scheduling. |
| **Direct purified Gibbs state for 32 system qubits** | $64+a$ | $6,208+97a$ | $21,568+337a$ | Imaginary-time filters, success probability, temperature, and preparation circuitry. |
| **Schrödingerisation with 20 state qubits and a chosen 8-qubit auxiliary-coordinate register** | $28+a$ | $2,716+97a$ | $9,436+337a$ | Eight auxiliary qubits are an illustrative truncation choice; physical extent and resolution must satisfy the target error. |
| **LCHS with 20 state qubits and a chosen 6-qubit quadrature-selection register** | $26+a$ | $2,522+97a$ | $8,762+337a$ | The 64-term index capacity is illustrative. Kernel, quadrature accuracy, preparation, selection, and recovery determine the actual count. |
| **Quantum Metropolis on $n$ state qubits** | $n+a$ | $97(n+a)$ | $337(n+a)$ | Energy registers, stored outcomes, rejection handling, precision, and coherent workspace are construction dependent. |
| **General Lindblad or retained-bath simulation on $n$ system qubits** | $n+a$ | $97(n+a)$ | $337(n+a)$ | Ancilla reuse, dilation, trajectory method, memory length, and accuracy determine $a$. |
| **Sequential MPS preparation/sampling with bond dimension $\chi=64$ and local dimension 2** | $6+1+a=7+a$ | $679+97a$ | $2,359+337a$ | Six bond qubits and one recycled site qubit describe this restricted streaming task. Retaining an entire L-site quantum output requires additional site registers. |

<a id="d2q5-encoding-comparison"></a>

### D2Q5 Encoding Comparison

| Lattice | Binary occupancy channels | Stored populations with $b=8$ | Amplitude-encoded data qubits |
| --- | ---: | ---: | ---: |
| $8\times8$ | 320 | 2,560 | 9 |
| $16\times16$ | 1,280 | 10,240 | 11 |
| $32\times32$ | 5,120 | 40,960 | 13 |

[Back to contents](#contents)

---

<a id="system-integration"></a>

## IV. System Integration and Routing

<a id="proposed-heterogeneous-architecture"></a>

### Proposed Heterogeneous Architecture

<img width="2316" height="auto" alt="image" src="https://github.com/user-attachments/assets/eec0ec06-1112-41e1-932e-146dfd7ed1d3" />

```text
Heterogeneous Quantum Computer (Architected for Fault-Tolerant Compatibility)
├─ Main QPU: Neutral-Atom (Rydberg/ tweezer)
│   ├─ Fast parallel CZ gates (~99.5%); long coherence (≈12.6 sec, hyperfine)
│   ├─ Reconfigurable layouts; mid-circuit measurement & erasure
│   └─ On-chip nanophotonics for coupling/ imaging
├─ Co-Processor: Quantum Photonic IC (QPIC)
│   ├─ Time-bin/ cluster-state generation; fusion operations
│   ├─ Waveguide arrays, thin-film LiNbO₃ modulators, Photon Number Resolving (PNR) detectors
│   └─ Fiber network to other racks/ fridges
├─ Translator(s) & quantum transducers: μw ↔ optical
│   ├─ Electro-optic (LiNbO₃), opto-mechanical, Rydberg ensembles
│   ├─ Superconducting metamaterial waveguides (engineered μw buses/ slow-light or TWPA media for transduction nodes & paramps)
│   ├─ Targets: internal η ≥ 0.1–0.5, added noise ≲ 1 photon
│   └─ With JPA/ JTWPA/KI-TWPA pre-amps (paramps), pump-noise filtering
├─ Quantum Memory: Superconducting Cat (bosonic)
│   ├─ Passive bit-flip suppression (noise bias)
│   ├─ Repetition-cat outer code; bias-preserving gates (SNAP-enabled)
│   └─ Logical store/ refresh; interface to ancilla
├─ Ancilla Layer: Transmons (tight to cat memories & readout)
│   ├─ Microwave-tunable Transmon (all-microwave, fixed-freq)
│   │   ├─ Effective ZZ/ CZ tuning via microwave dressing (MAP/ CR/ MATC-style)
│   │   └─ Flux-noise immunity; no DC-flux lines; compatible with fixed-freq layouts
│   └─ Voltage-tunable Transmon (advanced gatemon)
│       ├─ Frequency agility via electrostatic gating (semiconductor JJ)
│       └─ Syndrome extraction, parity checks, and cavity SNAP orchestration
└─ Classical Control & RAM
    ├─ Cryo-CMOS SRAM/ FBRAM/ GC-eDRAM near 4–12 K; microcode/ waveform cache
    ├─ Single-Flux-Quantum Digital (4 K stage; higher-integration families)
    │   ├─ RSFQ (DC-biased; ultrafast legacy baseline; static-power overhead); reference-grade timing/ waveform source or local clocks
    │   ├─ eSFQ/ ERSFQ (DC-biased, zero static power; preferred for scalable SFQ logic & SFQ-DACs/ JAWS)
    │   └─ RQL (AC-powered; no on-chip static power; multi-phase AC clock/ power; AC/ DC converters as needed)
    │       • SFQ-based AWG/ DAC for qubit drive (JAWS); isolate to mitigate quasiparticles; near-deterministic timing
    └─ Compiler for biased/ erasure noise and transduction-aware routing
```

<a id="logic-routing-decision-tree"></a>

### Logic Routing and Decision Tree

```text
Proposed heterogeneous quantum computing system
│
├─ 0. Determine the required computation, output, and execution route
│  ├─ A. What is the requested result?
│  │  ├─ Selected observables or samples → quantify preparation and measurement cost
│  │  ├─ Complete classical fields → include reconstruction and classical baselines [C]
│  │  └─ Coherent intermediate states → require quantum storage and coherent access
│  ├─ B. Which execution classification matches the specified implementation?
│  │  ├─ Central computation through quantum circuits → Q
│  │  ├─ Essential quantum–classical optimization or embedding → H
│  │  ├─ Native programmable interactions implement the target model → A
│  │  └─ CPU/GPU execution, including simulation of quantum circuits → C
│  ├─ C. Does one module perform the entire coherent subroutine?
│  │  ├─ Yes → execute locally; exchange parameters and results classically
│  │  └─ Requires several modules → evaluate the quantum interconnect in branch 3
│  ├─ D. Does the compiled implementation fit the resource and error budget?
│  │  ├─ Check loading, preparation, gates, ancillas, interfaces, and readout: XVIII
│  │  ├─ Yes → assign a physical circuit, analog, hybrid, or logical execution route
│  │  └─ Exceeds budget → reduce scope, change encoding, or retain an unresolved target
│  └─ E. What do the scope and provenance markers mean?
│     ├─ Unresolved problem-specific construction or extension → append R
│     ├─ Explicitly cited 2026 preprint → append P26 with its reference
│     └─ Neither marker alone establishes hardware readiness or quantum advantage
│
├─ 1. Main processor: neutral atoms in optical tweezer arrays
│  ├─ A. Is a native analog Hamiltonian an adequate model of the target? [A]
│  │  ├─ Yes → XI.A–C  Native spin and accessible many-body dynamics [A]
│  │  │        XIV.A–D Magnetism, transport, and supported dissipation [A]
│  │  │        XIII    Selected gauge models with explicit constraints [A]
│  │  └─ Requires other interactions → compile gates or derive a controlled mapping
│  │     └─ Optical-lattice Hubbard simulation requires the corresponding hardware
│  ├─ B. Are the required physical gates, readout, and controls available? [Q/H]
│  │  ├─ Yes → bounded circuit or essential quantum–classical implementations
│  │  │  ├─ I.C       Specified quantum walks and QCA transport models [Q]
│  │  │  ├─ I.A–B     QLBM/QLGA kernels with explicit collision and boundary treatment [Q/H]
│  │  │  ├─ III.A–C   VQS, local-domain QITE, and VQLS [H]
│  │  │  ├─ III.D     Specified variational differential-equation residual methods [H; R]
│  │  │  ├─ III.C     Variational sparse-system workflows [H]
│  │  │  │  └─ Check operator access, ansatz, residual estimation, and optimizer cost
│  │  │  ├─ III.E     QCPMD with classical nuclei and force estimation [H]
│  │  │  ├─ III.F     Compiled TFD preparation or variational thermal methods [Q/H]
│  │  │  ├─ IV.A,D–F  Bounded evolution, scattering, and thermal correlators [Q/H]
│  │  │  ├─ V.A,E     Resource-limited QAE or thermal ensemble methods [Q/H]
│  │  │  ├─ VI.A–D    Channels, trajectories, and specified stationary-state searches [Q/H]
│  │  │  ├─ X.C–E     VQE, QSE, quantum Krylov, QSCI, SQD, and suitable SKQD [H]
│  │  │  │  ├─ Quantum state preparation and projected-matrix estimation [Q]
│  │  │  │  └─ Optimization or sampled/projected sparse diagonalization [C]
│  │  │  └─ XVI       Impurity, active-space, and embedding subproblems [H]
│  │  └─ Missing operations → choose compatible circuits or another execution route
│  ├─ C. Are universal logical computation and sufficient resources available? [Q/H]
│  │  ├─ Check logical gates, state preparation, measurement, and repeated correction
│  │  ├─ Check required non-Clifford resources or another universal logical route
│  │  ├─ Check atom loss, leakage, movement, decoder latency, and logical error rates
│  │  ├─ If the complete workload fits → resource-qualified coherent candidates
│  │  │  ├─ II.A–B    QLSA and linear differential equation algorithms [Q]
│  │  │  ├─ II.A      Sparse-access HHL and block-encoded QSVT linear solvers [Q]
│  │  │  │  └─ Apply the sparse-access and output checks in branch 1.D
│  │  │  ├─ II.C–D    Schrödingerisation and digital LCHS constructions [Q]
│  │  │  ├─ II.E      Preconditioned FEM and encoded linear systems [Q/H]
│  │  │  │  └─ Specified Additive Schwarz construction [Q; P26, ref. 5]
│  │  │  ├─ II.F      Specified Lindbladian embeddings of classical linear ODEs [Q]
│  │  │  ├─ IV.A–D    Product formulas, LCU, qubitization, QSP, and interaction picture [Q]
│  │  │  ├─ IV.C      Sparse-Hamiltonian simulation with coherent operator access [Q]
│  │  │  ├─ IV.E,IX.F Sparse shifted-system resolvents [Q]
│  │  │  │  └─ Check spectral broadening, conditioning, solution norm, and overlaps
│  │  │  ├─ IV.E–F    Scattering and high-precision thermal response [Q/H]
│  │  │  ├─ V.A–D     QAE, quantum walks, Metropolis, and Gibbs constructions [Q]
│  │  │  ├─ VI        Specified channel and open-system constructions [Q/H]
│  │  │  ├─ VII.E     Linear Itô dilation and second-moment evolution [Q; P26, ref. 6]
│  │  │  ├─ VIII–IX   Encoded Dirac and Schrödinger dynamics in a physical sector [Q]
│  │  │  ├─ X.A–B     QPE, spectral filtering, and coherent state preparation [Q]
│  │  │  ├─ X.D       Block-encoded quantum Krylov/Lanczos workflows [Q/H]
│  │  │  ├─ XII–XIII  Encoded bosonic modes and truncated interacting field models [Q]
│  │  │  └─ XV.A–B    Compatible oscillator and wave-equation encodings [Q]
│  │  └─ Insufficient logical capability or budget → retain qualified bounded workloads
│  └─ D. Do the sparse-solver access, spectral, and output assumptions hold? [Q/H]
│     ├─ Specify the matrix, basis, dimensions, and row/column sparsity
│     ├─ Distinguish matrix-entry sparsity from Pauli-term sparsity and low rank
│     ├─ Construct coherent nonzero-location and matrix-entry queries or a block encoding
│     │  ├─ Structured operator → compile reversible index and value computation
│     │  └─ Stored data → include coherent-access construction, loading, and memory costs
│     │     └─ Classical CSR/CSC storage alone does not provide a quantum oracle
│     ├─ Include condition number, block normalization, precision, and success probability
│     ├─ Include source-state preparation and preconditioner construction/application
│     ├─ Non-Hermitian or singular systems → use a justified embedding or pseudoinverse
│     ├─ Normalized solution state → selected observables or coherent downstream use
│     │  └─ Recover required norms and overlaps for physical response quantities
│     ├─ Full classical vector → include reconstruction and compare with branch 6 [C]
│     └─ Missing construction or unfavorable total cost → reformulate or flag the gap [R]
│
├─ 2. Coprocessor: quantum photonic integrated circuit (QPIC)
│  ├─ A. What states, transformations, and measurements does it support? [Q/A]
│  │  ├─ Passive interferometers with suitable sources and detectors
│  │  │  ├─ I.C      Accessible path, time-bin, or frequency-bin walks [Q/A]
│  │  │  ├─ XII      Mode transformations and restricted photonic simulations [Q/A]
│  │  │  └─ XVII     Interference, sampling, and accessible observables [Q/A]
│  │  ├─ Squeezing plus Gaussian operations and Gaussian measurements
│  │  │  └─ XII.C    Quadratic bosonic dynamics within the supported Gaussian model [A]
│  │  └─ Entangled or non-Gaussian resources with suitable adaptive measurements
│  │     ├─ Specify photonic encoding, entangling operations, and feedforward
│  │     └─ Compile supported circuit, fusion-based, or measurement-based algorithms [Q]
│  ├─ B. Does the module support the complete proposed quantum subroutine? [Q/H]
│  │  ├─ Yes → III, V.F, X, XVI  Specified variational, sampling, or embedding tasks [H]
│  │  └─ Only a restricted operation → assign that operation explicitly [Q/A]
│  │     ├─ QAE requires a coherent oracle and the selected estimation protocol's controls
│  │     └─ Sparse QLSA requires operator queries, source preparation, and inverse filtering
│  └─ C. Is universal fault-tolerant photonic computation available? [Q]
│     ├─ Check resource-state production, loss handling, decoding, and adaptive control
│     ├─ Check logical resources, routing, synchronization, and required buffering
│     ├─ Yes → the same universal circuit algorithm families are candidates [Q/H]
│     │  ├─ Recompile costs for photonic encoding and architecture
│     │  └─ Apply branch 1.D to sparse QLSA, sparse evolution, and resolvent assignments
│     └─ Otherwise → retain qualified restricted photonic and hybrid roles [Q/H/A]
│
├─ 3. Intermodule communication and quantum interfaces
│  ├─ A. Is the payload a classical measurement record or parameter? [C]
│  │  └─ Yes → classical links carry outcomes, gradients, configurations, and metadata
│  │     ├─ VQS/VQLS optimization data and QITE-derived circuit parameters
│  │     ├─ SQD configurations and explicit classical trial-wavefunction descriptions
│  │     ├─ Lindblad jump records, heralds, syndromes, and Pauli-frame information
│  │     └─ Sparse matrix descriptions and compiler inputs, followed by local encoding
│  │        └─ Classical communication does not transfer coherent amplitudes
│  ├─ B. Is the payload a coherent quantum state or entanglement resource?
│  │  ├─ Neutral-atom module ↔ optical network
│  │  │  └─ Requires an atom–photon interface and compatible optical encoding
│  │  ├─ Superconducting module ↔ optical network
│  │  │  └─ Requires coherent microwave–optical transduction or a specified alternative
│  │  ├─ Photonic network ↔ encoded memory or processor
│  │  │  └─ Requires mode matching, capture or heralded entanglement, and code mapping
│  │  └─ Select a concrete protocol
│  │     ├─ Direct coherent transfer with characterized channel errors
│  │     └─ Heralded entanglement plus teleportation and classical feedforward
│  └─ C. Does the complete link meet the computation's requirements?
│     ├─ Check fidelity, loss, added noise, entanglement rate, bandwidth, and latency
│     ├─ Check photon modes, wavelengths, logical encodings, and register sizes
│     ├─ Check local decoding, link-error handling, and memory waiting errors
│     ├─ Include repeated remote operator queries or remote gates if the algorithm uses them
│     ├─ Yes → distribute the specified logical subroutine or storage operation
│     └─ Otherwise → keep coherent subroutines local and exchange classical outputs
│
├─ 4. Superconducting bosonic module: cat memory and optional local processing
│  ├─ A. Is the cat encoding stabilized and its logical error characterized?
│  │  ├─ Yes → store encoded information within its lifetime and error budget
│  │  └─ Otherwise → treat as a physical bosonic-memory prototype
│  ├─ B. Does the module correct all relevant logical error channels?
│  │  ├─ Check code-specific bias, residual errors, syndrome circuits, and leakage
│  │  └─ Use active correction or concatenation appropriate to the chosen encoding
│  ├─ C. Does the source processor coherently access this memory?
│  │  ├─ Yes → candidate storage assignments supporting the specified solver
│  │  │  ├─ II,IV,X    Encoded solution states and algorithm work registers
│  │  │  │  └─ Include the complete multi-qubit register and its code-mapping overhead
│  │  │  ├─ III.F,V    Purification registers or thermal-state intermediates
│  │  │  ├─ XVI        Impurity or active-space states when coherent reuse is useful
│  │  │  └─ Distributed protocols: entangled resources awaiting consumption
│  │  └─ Interface unavailable → restrict memory to locally connected circuits
│  └─ D. Does the bosonic module also implement the required operations? [Q/A]
│     ├─ Native oscillator controls → XII  Supported bosonic or spin–boson models [A]
│     ├─ Specialized compiled controls → II.D  Oscillator–qubit LCHS [Q/A; P26, ref. 4]
│     │  └─ Check non-Gaussian preparation, truncation, gates, and postselection overhead
│     ├─ Universal logical controls → resource-qualified circuit-solver assignments [Q]
│     │  └─ Apply branch 1.D for sparse linear systems and encoded operator queries
│     └─ Essential classical optimization or embedding → classify the full workflow H
│
├─ 5. Local superconducting ancillas and gate interfaces
│  ├─ A. Transmons or other auxiliaries coupled to the bosonic module
│  │  ├─ Code-specific syndrome extraction, reset, readout, and conditional control
│  │  ├─ Characterized dispersive interactions and oscillator gate implementations
│  │  └─ SNAP or other pulse constructions where the local Hamiltonian supports them
│  ├─ B. Does the complete gate preserve the intended cat-qubit noise bias?
│  │  ├─ Yes → use a decoder and logical construction matched to that error model
│  │  └─ Otherwise → include resulting error channels and protection overhead
│  └─ C. Operations on atom or remote photonic registers
│     └─ Require their local gates or a qualified remote-gate protocol via branch 3
│
├─ 6. Classical control, CPUs, GPUs, and RAM [C]
│  ├─ A. Host software: solver selection, compilation, scheduling, and resource estimation
│  ├─ B. Real-time electronics: laser, optical, microwave, and readout control
│  │  └─ FPGA, CMOS, cryogenic CMOS, or SFQ elements selected by bandwidth and heat budget
│  ├─ C. Error processing: decoding, atom-loss tracking, herald handling, and feedforward
│  ├─ D. Essential classical components of quantum–classical workflows [C]
│  │  ├─ III,X      Optimization, QITE coefficient solves, and subspace diagonalization
│  │  ├─ V.F        Classical AFQMC supplied with specified quantum-derived information
│  │  ├─ XVI        DMFT/DMET self-consistency, molecular forces, and domain interfaces
│  │  ├─ XVI.E      Classical coarse solvers with quantum fine corrections [H; P26, ref. 7]
│  │  └─ XVII       Spectra, uncertainty estimates, and parameter inference
│  ├─ E. Classical sparse-solver and simulation comparisons: XIX [C]
│  │  ├─ Sparse LU/Cholesky and preconditioned CG/GMRES for appropriate linear systems
│  │  ├─ Lanczos, Arnoldi, and shift-invert methods for sparse eigenproblems
│  │  ├─ Sparse NEGF/resolvents, Lindblad solves, QMC, and tensor-network methods
│  │  └─ Compare matched accuracy, outputs, loading, memory, and total execution cost
│  └─ F. RAM stores classical matrices, distributions, meshes, samples, and records [C]
│     ├─ CSR/CSC entries feed classical solvers or a specified quantum-loading procedure
│     └─ Coherent amplitude-encoded states require quantum storage and coherent access
│
└─ 7. Unresolved problem-specific constructions and architecture extensions [R]
   ├─ A. Nonlinear, nonlocal, and stochastic extensions beyond specified constructions [Q/H; R]
   │  ├─ VII.A–B    Carleman, Koopman, and Liouville mappings outside established bounds
   │  ├─ VII.C–D    Fractional and phase-space models without efficient operator access
   │  ├─ VII.E–F    Stochastic models beyond the cited linear or system–field construction
   │  └─ VII.G–H    Unresolved uncertainty, AMR, hp-element, and multiphysics workflows
   ├─ B. Sparse operator and stationary-state gaps [Q/H; R]
   │  ├─ II.A,E     Missing access circuits, useful preconditioners, or loading analysis
   │  ├─ VI.D       Sparse Liouvillian null-space or trace-constrained proposals
   │  │  └─ Check uniqueness, conditioning, positivity, and physical-state recovery
   │  └─ IX.F,X.D   Unresolved resolvent or eigensolver constructions for the target model
   ├─ C. Field and multiscale extensions [Q/H; R]
   │  ├─ XV.C–D     Maxwell/plasma mappings with unresolved sources or constraints
   │  └─ XVI.D–F    Unspecified DFT subproblems, homogenization, and domain interfaces
   ├─ D. Hardware and distributed-computation extensions [Q/H/A; R]
   │  └─ Missing native interactions, logical controls, or qualified coherent interfaces
   └─ E. Reassign a target only after its construction and full resource assessment: XVIII
      ├─ Include loading, preparation, truncation, gates, links, readout, and accuracy
      ├─ Fit demonstrated capabilities and budget → assign Q, H, A, or C as appropriate
      └─ P26 remains citation provenance; it does not automatically assign R
```

<a id="sparse-solver-routing-guide"></a>

### Sparse-Solver Routing Guide

| Mathematical task | Label | Taxonomy branch | Execution route | Required checks |
| --- | :---: | --- | --- | --- |
| Sparse linear-system solution | Q/H | II.A | Q, or H for a specified essential quantum–classical workflow | Coherent operator access, source preparation, conditioning, inverse approximation, and output recovery. |
| Variational sparse-system solution | H | III.C | H | Operator decomposition or query construction, residual estimation, ansatz expressivity, classical optimization, and measurement cost. |
| Sparse-Hamiltonian evolution | Q | IV.C | Q | Coherent sparse access or another explicit block encoding, normalization, evolution time, and precision. |
| Sparse resolvent or Green-function evaluation | Q/H | IV.E and IX.F | Q/H | Shifted operator access, broadening, conditioning, solution norms, and matrix-element estimation. |
| Quantum Krylov/Lanczos eigenvalue estimation | Q/H | X.D | Q/H | Operator access, initial overlap, subspace preparation, spectral gaps, projected-matrix conditioning, and sampling. |
| QPU-selected configuration diagonalization | H/C | X.E | H overall; C for diagonalization | Quantum configuration generation, selected-space accuracy, classical sparse-matrix construction, and classical eigenproblem cost. |
| Sparse Liouvillian stationary-state proposal | Q/H; R | VI.D | Q/H; R for an unresolved construction | Singular/non-Hermitian treatment, trace constraint, uniqueness, conditioning, positivity, and usable physical-state recovery. |
| Classical sparse simulation | C | XIX.C–D | C | Appropriate solver, preconditioning, discretization error, memory, and matched output accuracy. |

[Back to contents](#contents)

---

<a id="references"></a>

## References

1. Wang, B., Meng, Z., Zhao, Y. and Yang, Y. (2025) *Quantum lattice Boltzmann method for simulating nonlinear fluid dynamics*. [Preprint]. arXiv:2502.16568. Available at: [https://arxiv.org/abs/2502.16568](https://arxiv.org/abs/2502.16568).
2. Georgescu, C.A., Schalkers, M.A. and Möller, M. (2024) *qlbm – A Quantum Lattice Boltzmann Software Framework*. [Preprint]. arXiv:2411.19439. Available at: [https://arxiv.org/abs/2411.19439](https://arxiv.org/abs/2411.19439).
3. Meyer, D.A. (1996) 'From quantum cellular automata to quantum lattice gases', *Journal of Statistical Physics*, 85, pp. 551–574. Available at: [https://doi.org/10.1007/BF02199356](https://doi.org/10.1007/BF02199356).
4. Strauch, F.W. (2006) 'Relativistic quantum walks', *Physical Review A*, 73(5), 054302. Available at: [https://doi.org/10.1103/PhysRevA.73.054302](https://doi.org/10.1103/PhysRevA.73.054302).
5. Arnault, P., Di Molfetta, G., Brachet, M. and Debbasch, F. (2016) 'Quantum walks and non-Abelian discrete gauge theory', *Physical Review A*, 94(1), 012335. Available at: [https://doi.org/10.1103/PhysRevA.94.012335](https://doi.org/10.1103/PhysRevA.94.012335).
6. Márquez-Martín, I., Arnault, P., Di Molfetta, G. and Pérez, A. (2018) 'Electromagnetic lattice gauge invariance in two-dimensional discrete-time quantum walks', *Physical Review A*, 98(3), 032333. Available at: [https://doi.org/10.1103/PhysRevA.98.032333](https://doi.org/10.1103/PhysRevA.98.032333).
7. Harrow, A.W., Hassidim, A. and Lloyd, S. (2009) 'Quantum algorithm for linear systems of equations', *Physical Review Letters*, 103(15), 150502. Available at: [https://doi.org/10.1103/PhysRevLett.103.150502](https://doi.org/10.1103/PhysRevLett.103.150502).
8. Berry, D.W., Childs, A.M., Ostrander, A. and Wang, G. (2017) 'Quantum algorithm for linear differential equations with exponentially improved dependence on precision', *Communications in Mathematical Physics*, 356, pp. 1057–1081. Available at: [https://doi.org/10.1007/s00220-017-3002-y](https://doi.org/10.1007/s00220-017-3002-y).
9. Wu, H.-C., Wang, J. and Li, X. (2024) *Quantum algorithms for non-linear dynamics: Revisiting Carleman linearization with no dissipative conditions*. [Preprint]. arXiv:2405.12714. Available at: [https://arxiv.org/abs/2405.12714](https://arxiv.org/abs/2405.12714).
10. Low, G.H. and Chuang, I.L. (2017) 'Optimal Hamiltonian simulation by quantum signal processing', *Physical Review Letters*, 118(1), 010501. Available at: [https://doi.org/10.1103/PhysRevLett.118.010501](https://doi.org/10.1103/PhysRevLett.118.010501).
11. Childs, A.M., Kothari, R. and Somma, R.D. (2017) 'Quantum algorithm for linear systems of equations with exponentially improved dependence on precision', *SIAM Journal on Computing*, 46(6), pp. 1920–1950. Available at: [https://doi.org/10.1137/16M1087072](https://doi.org/10.1137/16M1087072).
12. Brassard, G., Høyer, P., Mosca, M. and Tapp, A. (2002) 'Quantum amplitude amplification and estimation', *Quantum Information & Computation*, 2(1), pp. 53–74. Available at: [https://arxiv.org/abs/quant-ph/0005055](https://arxiv.org/abs/quant-ph/0005055).
13. Suzuki, S., et al. (2021) 'Real- and imaginary-time evolution with compressed quantum circuits', *PRX Quantum*, 2(1), 010342. Available at: [https://doi.org/10.1103/PRXQuantum.2.010342](https://doi.org/10.1103/PRXQuantum.2.010342).
14. McArdle, S., et al. (2020) 'Variational quantum simulation of general processes', *Physical Review Letters*, 125(1), 010501. Available at: [https://doi.org/10.1103/PhysRevLett.125.010501](https://doi.org/10.1103/PhysRevLett.125.010501).
15. Kuroiwa, K., Ohkuma, T., Sato, H. and Imai, R. (2022) *Quantum Car-Parrinello molecular dynamics: A cost-efficient molecular simulation method on near-term quantum computers*. [Preprint]. arXiv:2212.11921. Available at: [https://arxiv.org/abs/2212.11921](https://arxiv.org/abs/2212.11921).
16. Arrighi, P., Forets, M. and Nesme, V. (2014) 'The Dirac equation as a quantum walk: higher dimensions, observational convergence', *Journal of Physics A: Mathematical and Theoretical*, 47(46), 465302. Available at: [https://doi.org/10.1088/1751-8113/47/46/465302](https://doi.org/10.1088/1751-8113/47/46/465302).
17. Huerta Alderete, C., et al. (2020) 'Quantum walks and Dirac cellular automata on a programmable trapped-ion quantum computer', *Nature Communications*, 11, 3720. Available at: [https://doi.org/10.1038/s41467-020-17519-4](https://doi.org/10.1038/s41467-020-17519-4).
18. Ding, Z., Li, X. and Lin, L. (2024) 'Simulating open quantum systems using Hamiltonian simulations', *PRX Quantum*, 5(2), 020332. Available at: [https://doi.org/10.1103/PRXQuantum.5.020332](https://doi.org/10.1103/PRXQuantum.5.020332).
19. Cleve, R. and Wang, C. (2016) *Efficient quantum algorithms for simulating Lindblad evolution*. [Preprint]. arXiv:1612.09512. Available at: [https://arxiv.org/abs/1612.09512](https://arxiv.org/abs/1612.09512).
20. Yung, M.-H., Whitfield, J.D., Tempel, D.G., Aspuru-Guzik, A. and Lloyd, S. (2014) 'Computational complexity of time-dependent density functional theory', *New Journal of Physics*, 16(8), 083035. Available at: [https://doi.org/10.1088/1367-2630/16/8/083035](https://doi.org/10.1088/1367-2630/16/8/083035).
21. Cavaglia, A., et al. (2020) 'An algorithm for quantum computation of particle decays and cross-sections', *Physical Review D*, 102(9), 094505. Available at: [https://doi.org/10.1103/PhysRevD.102.094505](https://doi.org/10.1103/PhysRevD.102.094505).
22. Rong, X., et al. (2024) *Quantum multigrid algorithm for finite-element problems*. [Preprint]. arXiv:2404.07466. Available at: [https://arxiv.org/abs/2404.07466](https://arxiv.org/abs/2404.07466).
23. Alkadri, A.M., Kharazi, T.D. and Whaley, K.B. (2025) *A quantum algorithm for the finite-element method*. [Preprint]. arXiv:2510.18150. Available at: [https://arxiv.org/abs/2510.18150](https://arxiv.org/abs/2510.18150).
24. Shaviner, G. (2025) *Quantum singular-value transformation for solving linear systems of differential equations*. [Preprint]. arXiv:2507.09686. Available at: [https://arxiv.org/abs/2507.09686](https://arxiv.org/abs/2507.09686).
25. Grinko, D., Gacon, J., Zoufal, C. and Woerner, S. (2021) 'Iterative quantum amplitude estimation', *npj Quantum Information*, 7, 52. Available at: [https://doi.org/10.1038/s41534-021-00379-1](https://doi.org/10.1038/s41534-021-00379-1).
26. Zhou, Y., et al. (2025) *Adaptive mesh-refinement quantum algorithm for Maxwell’s equations*. [Preprint]. arXiv:2504.01646. Available at: [https://arxiv.org/abs/2504.01646](https://arxiv.org/abs/2504.01646).
27. Yu, Y. (2025) *Schrödingerization based quantum algorithms for the fractional Poisson equation*. [Preprint]. arXiv:2505.01602. Available at: [https://arxiv.org/abs/2505.01602](https://arxiv.org/abs/2505.01602).
28. Ayral, T. (2025) *Dynamical mean-field theory with quantum computing*. [Preprint]. arXiv:2508.00118. Available at: [https://arxiv.org/abs/2508.00118](https://arxiv.org/abs/2508.00118).
29. Georgescu, C.A. (2024) *Quantum Algorithms for the Lattice Boltzmann Method: Encoding and Evolution*. PhD thesis, TU Delft. Available at: [https://resolver.tudelft.nl/a7b20729-46b7-42d1-aaf7-c001fc93efd9](https://resolver.tudelft.nl/a7b20729-46b7-42d1-aaf7-c001fc93efd9).
30. Klco, N., Savage, M.J. and Stryker, J.R. (2020) 'SU(2) non-Abelian gauge field theory in one dimension on digital quantum computers', *Physical Review D*, 101(7), 074512. Available at: [https://doi.org/10.1103/PhysRevD.101.074512](https://doi.org/10.1103/PhysRevD.101.074512).
31. García-Molina, P., Rodríguez-Mediavilla, J. and García-Ripoll, J.J. (2022) 'Quantum Fourier analysis for multivariate functions and applications to a class of Schrödinger-type partial differential equations', *Physical Review A*, 105(1), 012433. Available at: [https://doi.org/10.1103/PhysRevA.105.012433](https://doi.org/10.1103/PhysRevA.105.012433).
32. Kyriienko, O. (2023) *Quantum Chebyshev transform: mapping, embedding, learning and sampling distributions*. [Preprint]. arXiv:2306.17026. Available at: [https://arxiv.org/abs/2306.17026](https://arxiv.org/abs/2306.17026).
33. Zhao, T., et al. (2022) *Quantum-inspired variational algorithms for partial differential equations: application to financial derivative pricing*. [Preprint]. arXiv:2207.10838. Available at: [https://arxiv.org/abs/2207.10838](https://arxiv.org/abs/2207.10838).
34. Zhou, Y., et al. (2024) *Quantum algorithm for partial differential equations of non-conservative systems with spatially varying parameters*. [Preprint]. arXiv:2407.05019. Available at: [https://arxiv.org/abs/2407.05019](https://arxiv.org/abs/2407.05019).
35. Foulkes, W.M.C., Mitas, L., Needs, R.J. and Rajagopal, G. (2001) 'Quantum Monte Carlo simulations of solids', *Reviews of Modern Physics*, 73(1), pp. 33–83. Available at: [https://doi.org/10.1103/RevModPhys.73.33](https://doi.org/10.1103/RevModPhys.73.33).
36. Stenger, M., et al. (2022) *Quantum algorithms for uncertainty quantification: application to partial differential equations*. [Preprint]. arXiv:2209.11220. Available at: [https://arxiv.org/abs/2209.11220](https://arxiv.org/abs/2209.11220).
37. Bianucci, P., Miquel, C., Paz, J.P. and Saraceno, M. (2001) *Discrete Wigner functions and the phase-space representation of quantum computers*. [Preprint]. arXiv:quant-ph/0106091. Available at: [https://arxiv.org/abs/quant-ph/0106091](https://arxiv.org/abs/quant-ph/0106091).
38. Kitaev, A.Y. (1995) *Quantum measurements and the Abelian stabilizer problem*. [Preprint]. arXiv:quant-ph/9511026. Available at: [https://arxiv.org/abs/quant-ph/9511026](https://arxiv.org/abs/quant-ph/9511026).
39. Arute, F., et al. (2019) 'Quantum supremacy using a programmable superconducting processor', *Nature*, 574, pp. 505–510. Available at: [https://doi.org/10.1038/s41586-019-1666-5](https://doi.org/10.1038/s41586-019-1666-5).
40. Lloyd, S. (1996) 'Universal quantum simulators', *Science*, 273(5278), pp. 1073–1078. Available at: [https://doi.org/10.1126/science.273.5278.1073](https://doi.org/10.1126/science.273.5278.1073).
41. Bausch, J., et al. (2023) *Quantum algorithms for fractional-order operators*. [Preprint]. arXiv:2303.06703. Available at: [https://arxiv.org/abs/2303.06703](https://arxiv.org/abs/2303.06703).
42. Brown, K.R., et al. (2021) 'Quantum algorithms for PDEs: a review', *Frontiers in Physics*, 9, 667392. Available at: [https://doi.org/10.3389/fphy.2021.667392](https://doi.org/10.3389/fphy.2021.667392).
43. Barends, R., et al. (2014) 'Superconducting quantum circuits at the surface code threshold for fault tolerance', *Nature*, 508, pp. 500–503. Available at: [https://doi.org/10.1038/nature13171](https://doi.org/10.1038/nature13171).
44. Rotentg, M., et al. (2022) 'Quantum-accelerated stochastic differential equations', *Quantum*, 6, 805. Available at: [https://doi.org/10.22331/q-2022-06-09-805](https://doi.org/10.22331/q-2022-06-09-805).
45. Gottesman, D. (1998) *The Heisenberg representation of quantum computers*. [Preprint]. arXiv:quant-ph/9807006. Available at: [https://arxiv.org/abs/quant-ph/9807006](https://arxiv.org/abs/quant-ph/9807006).
46. Babbush, R., et al. (2018) 'Low-depth quantum simulation of materials', *Physical Review X*, 8(1), 011044. Available at: [https://doi.org/10.1103/PhysRevX.8.011044](https://doi.org/10.1103/PhysRevX.8.011044).
47. Syamlal, M., Copen, C., Takahashi, M. and Hall, B. (2024) *Computational Fluid Dynamics on Quantum Computers*. [Preprint]. arXiv:2406.18749. Available at: [https://arxiv.org/abs/2406.18749](https://arxiv.org/abs/2406.18749).
48. Penuel, J., et al. (2024) *Feasibility of accelerating incompressible computational fluid dynamics simulations with fault-tolerant quantum computers*. [Preprint]. arXiv:2406.06323. Available at: [https://arxiv.org/abs/2406.06323](https://arxiv.org/abs/2406.06323).
49. Li, J., et al. (2025) *Multi-set variational quantum dynamics algorithm for simulating nonadiabatic dynamics on quantum computers*. [Preprint]. arXiv:2503.07388. Available at: [https://arxiv.org/abs/2503.07388](https://arxiv.org/abs/2503.07388).
50. Duriez, A., et al. (2025) *Computing band gaps of periodic materials via sample-based quantum diagonalization*. [Preprint]. arXiv:2503.10901. Available at: [https://arxiv.org/abs/2503.10901](https://arxiv.org/abs/2503.10901).
51. Arora, A., Ward, B.M. and Oskay, C. (2024) *An implementation of the finite element method in hybrid classical/quantum computers*. [Preprint]. arXiv:2411.09038. Available at: [https://arxiv.org/abs/2411.09038](https://arxiv.org/abs/2411.09038).
52. Ghisoni, F., Scala, F., Bajoni, D. and Gerace, D. (2024) *Shadow quantum linear solver: A resource-efficient quantum algorithm for linear systems of equations*. [Preprint]. arXiv:2409.08929. Available at: [https://arxiv.org/abs/2409.08929](https://arxiv.org/abs/2409.08929).
53. Kahanamoku-Meyer, G.D. (2023) *Exploring the Limits of Classical Simulation: From Computational Many-Body Dynamics to Quantum Advantage*. PhD thesis, University of California, Berkeley. Available at: [https://gregkm.me/files/Kahanamoku-Meyer_dissertation.pdf](https://gregkm.me/files/Kahanamoku-Meyer_dissertation.pdf).
54. Beverland, M.E. et al. (2022) *Assessing requirements to scale to practical quantum advantage*. [Preprint]. arXiv:2211.07629. Available at: [https://doi.org/10.48550/arXiv.2211.07629](https://doi.org/10.48550/arXiv.2211.07629).
55. Gilyén et al. (2018) *Quantum singular value transformation and beyond*. [Preprint]. Available at: [https://arxiv.org/abs/1806.01838](https://arxiv.org/abs/1806.01838). Scope: QSVT, matrix functions, and quantum linear algebra primitives.
56. Berry, Childs, Ostrander, and Wang (2017) *Quantum algorithm for linear differential equations with exponentially improved dependence on precision*. Available at: [https://arxiv.org/abs/1701.03684](https://arxiv.org/abs/1701.03684). Scope: LDE solution-state preparation and correct author attribution.
57. Krovi (2023) *Improved quantum algorithms for linear and nonlinear differential equations*. Available at: [https://quantum-journal.org/papers/q-2023-02-02-913/](https://quantum-journal.org/papers/q-2023-02-02-913/); Shang et al. (2025) *Designing a Nearly Optimal Quantum Algorithm for Linear Differential Equations via Lindbladians*. Available at: [https://link.aps.org/doi/10.1103/cvl9-97qg](https://link.aps.org/doi/10.1103/cvl9-97qg). Scope: Linear and Carleman-based nonlinear algorithms; an open-system ODE route.
58. Jin, Liu, and Yu (2022) *Quantum simulation of partial differential equations via Schrödingerisation*. [Preprint]. Available at: [https://arxiv.org/abs/2212.13969](https://arxiv.org/abs/2212.13969). Scope: Enlarged-space unitary representations of linear differential equations.
59. An et al. (2026) *Quantum Algorithm for Linear Non-unitary Dynamics with Near-Optimal Dependence on All Parameters*. Available at: [https://link.springer.com/article/10.1007/s00220-025-05509-w](https://link.springer.com/article/10.1007/s00220-025-05509-w). Scope: Improved LCHS representations and precision dependence under stated assumptions.
60. Wang et al. (2025) *Quantum lattice Boltzmann method for simulating nonlinear fluid dynamics*. Available at: [https://arxiv.org/abs/2502.16568](https://arxiv.org/abs/2502.16568). Scope: Node-level ensemble formulation; numerical benchmark sizes should be separated from QPU hardware demonstrations.
61. Deiml and Peterseim (2024) *Quantum Realization of the Finite Element Method*. [Preprint]. Available at: [https://arxiv.org/abs/2403.19512](https://arxiv.org/abs/2403.19512). Scope: BPX preconditioning and selected solution functionals for a specified elliptic FEM setting.
62. Fressart, Nowak, and Spillane (2026) *Quantum Domain Decomposition for Preconditioning the Finite Element Method*. [Preprint]. Available at: [https://arxiv.org/abs/2605.26090](https://arxiv.org/abs/2605.26090). Scope: Additive Schwarz preconditioning and quantum block-encoding costs.
63. Bravo-Prieto et al. (2023) *Variational Quantum Linear Solver*. Available at: [https://quantum-journal.org/papers/q-2023-11-22-1188/](https://quantum-journal.org/papers/q-2023-11-22-1188/). Scope: VQLS prepares a normalized linear-system solution.
64. Motta et al. (2019) *Determining eigenstates and thermal states on a quantum computer using quantum imaginary time evolution*. [Preprint]. Available at: [https://arxiv.org/abs/1901.07653](https://arxiv.org/abs/1901.07653). Scope: QITE, quantum Lanczos, and thermal extensions.
65. Kuroiwa et al. (2022) *Quantum Car–Parrinello Molecular Dynamics*. [Preprint]. Available at: [https://arxiv.org/abs/2212.11921](https://arxiv.org/abs/2212.11921). Scope: Hybrid nuclear and electronic dynamics.
66. Piccinelli et al. (2025) *Quantum chemistry with provable convergence via randomized sample-based Krylov quantum diagonalization*. [Preprint]. Available at: [https://arxiv.org/abs/2508.02578](https://arxiv.org/abs/2508.02578). Scope: SQD, SKQD, and randomized propagation; convergence requires stated state-concentration assumptions.
67. van Rossum et al. (2026) *Improved quantum sampling methods for molecular simulations*. [Preprint]. Available at: [https://arxiv.org/abs/2608.11569](https://arxiv.org/abs/2608.11569). Scope: Controls on classical diagonalization size and measurement-basis effects in sampling comparisons.
68. Danilov et al. (2025) *Enhancing the accuracy and efficiency of sample-based quantum diagonalization with phaseless auxiliary-field quantum Monte Carlo*. Available at: [https://arxiv.org/abs/2503.05967](https://arxiv.org/abs/2503.05967). Scope: QPU-derived SQD trials supplied to classical phaseless AFQMC.
69. Montanaro (2015) *Quantum speedup of Monte Carlo methods*. Available at: [https://arxiv.org/abs/1504.06987](https://arxiv.org/abs/1504.06987). Scope: Quantum expectation estimation and near-quadratic oracle advantages under appropriate assumptions.
70. Temme et al. (2009) *Quantum Metropolis Sampling*. [Preprint]. Available at: [https://arxiv.org/abs/0911.3635](https://arxiv.org/abs/0911.3635). Scope: Quantum thermal sampling using energy eigenstates and quantum transitions.
71. Chen, Kastoryano, and Gilyén (2023) *An efficient and exact noncommutative quantum Gibbs sampler*. [Preprint]. Available at: [https://arxiv.org/abs/2311.09207](https://arxiv.org/abs/2311.09207). Scope: Detailed-balanced Lindbladian Gibbs preparation with mixing-time dependence.
72. (2026) *Quantum simulation algorithms based on quantum trajectories*. Available at: [https://quantum-journal.org/papers/q-2026-04-13-2063/](https://quantum-journal.org/papers/q-2026-04-13-2063/). Scope: Additive jump-operator query dependence for a specified class of Lindbladians.
73. Babbush et al. (2023) *Exponential quantum speedup in simulating coupled classical oscillators*. Available at: [https://arxiv.org/abs/2303.13012](https://arxiv.org/abs/2303.13012). Scope: Harmonic-system encoding and selected observable estimation with efficient input access.
74. Das et al. (2026) *Gate-Level Quantum Simulation of Nonunitary Linear Dynamics with Hybrid Oscillator-Qubit Architecture*. [Preprint]. Available at: [https://arxiv.org/abs/2605.10708](https://arxiv.org/abs/2605.10708). Scope: Hybrid oscillator–qubit LCHS construction.
75. Wu and Li (2026) *Universal Dilation of Linear Itô SDEs: Quantum Trajectories and Lindblad Simulation of Second Moments*. [Preprint]. Available at: [https://arxiv.org/abs/2601.05928](https://arxiv.org/abs/2601.05928). Scope: Linear stochastic equations, trajectory dilation, and second moments.
76. Uhrig (2024) *Landau–Lifshitz damping from Lindbladian dissipation in quantum magnets*. [Preprint]. Available at: [https://arxiv.org/abs/2406.10613](https://arxiv.org/abs/2406.10613). Scope: LL reduction with local mean field, weak fields, and a specified adaptive dissipator.
77. Meth et al. (2025) *Simulating two-dimensional lattice gauge theories on a qudit quantum computer*. Available at: [https://www.nature.com/articles/s41567-025-02797-w](https://www.nature.com/articles/s41567-025-02797-w). Scope: A concrete qudit gauge-theory experiment.
78. Babbush et al. (2019) *Quantum simulation of chemistry with sublinear scaling in basis size*. Available at: [https://www.nature.com/articles/s41534-019-0199-y](https://www.nature.com/articles/s41534-019-0199-y). Scope: First quantization and interaction-picture electronic-structure simulation.
79. Balazi, Deiml, and Peterseim (2026) *Quantum Enhanced Numerical Homogenization*. [Preprint]. Available at: [https://arxiv.org/abs/2603.28521](https://arxiv.org/abs/2603.28521). Scope: Local fine-scale quantum subproblems coupled to a classical coarse model using selected measurements.
80. Wu and Hsieh (2018) *Variational Thermal Quantum Simulation via Thermofield Double States*. [Preprint]. Available at: [https://arxiv.org/abs/1811.11756](https://arxiv.org/abs/1811.11756). Scope: Alternating evolution for variational TFD preparation.
81. Zhu et al. (2019) *Generation of Thermofield Double States and Critical Ground States with a Quantum Computer*. Available at: [https://arxiv.org/abs/1906.02699](https://arxiv.org/abs/1906.02699). Scope: Trapped-ion thermal-state preparation and model-specific circuit-depth effects under noise.
82. Verdon et al. (2019) *Quantum Hamiltonian-Based Models and the Variational Quantum Thermalizer Algorithm*. [Preprint]. Available at: [https://arxiv.org/abs/1910.02071](https://arxiv.org/abs/1910.02071). Scope: VQT and hybrid models for thermal states.
83. Faílde et al. (2023) *Hamiltonian Forging of a Thermofield Double*. [Preprint]. Available at: [https://arxiv.org/abs/2311.10566](https://arxiv.org/abs/2311.10566). Scope: Engineered TFD Hamiltonians and entanglement forging with smaller circuits.
84. Cottrell et al. (2018) *How to Build the Thermofield Double State*. [Preprint]. Available at: [https://arxiv.org/abs/1811.11528](https://arxiv.org/abs/1811.11528). Scope: Parent-Hamiltonian construction under stated physical assumptions.
85. Chen et al. (2024) *Minimally Entangled Typical Thermal States for Classical and Quantum Simulation of 1+1-Dimensional Z2 Lattice Gauge Theory at Finite Temperature and Density*. [Preprint]. Available at: [https://arxiv.org/abs/2407.11949](https://arxiv.org/abs/2407.11949). Scope: Classical and quantum METTS approaches to a specified gauge model.
86. Martyn and Swingle (2018) *Product Spectrum Ansatz and the Simplicity of Thermal States*. [Preprint]. Available at: [https://arxiv.org/abs/1812.01015](https://arxiv.org/abs/1812.01015). Scope: Variational thermal preparation using a product spectrum and an entangling circuit.
87. Sugiura and Shimizu (2013) *Canonical Thermal Pure Quantum State*. Available at: [https://arxiv.org/abs/1302.3138](https://arxiv.org/abs/1302.3138). Scope: Canonical thermal typicality; QPU preparation requires a specified quantum algorithm.
88. Bluvstein, D., et al. (2023) *Logical quantum processor based on reconfigurable atom arrays*. Available at: [https://www.nature.com/articles/s41586-023-06927-3](https://www.nature.com/articles/s41586-023-06927-3). Scope: Encoded neutral-atom processing, reconfiguration, and logical circuit demonstrations.
89. (2025) *A fault-tolerant neutral-atom architecture for universal quantum computation*. Available at: [https://www.nature.com/articles/s41586-025-09848-5](https://www.nature.com/articles/s41586-025-09848-5). Scope: Architecture-level ingredients for universal fault-tolerant neutral-atom processing.
90. Bartolucci, S., et al. (2023) *Fusion-based quantum computation*. Available at: [https://www.nature.com/articles/s41467-023-36493-1](https://www.nature.com/articles/s41467-023-36493-1). Scope: Universal computation using resource states and entangling measurements with photonic error models.
91. Covey, J. P., Weinfurter, H., and Bernien, H. (2023) *Quantum networks with neutral atom processing nodes*. [Preprint]. Available at: [https://arxiv.org/abs/2304.02088](https://arxiv.org/abs/2304.02088). Scope: Atom–photon interfaces, remote entanglement, and network-node requirements.
92. Putterman, H., et al. (2025) *Hardware-efficient quantum error correction via concatenated bosonic qubits*. Available at: [https://www.nature.com/articles/s41586-025-08642-7](https://www.nature.com/articles/s41586-025-08642-7). Scope: Cat-qubit protection combined with an outer code and transmon syndrome ancillas.
93. Puri, S., et al. (2019) *Bias-preserving gates with stabilized cat qubits*. [Preprint]. Available at: [https://arxiv.org/abs/1905.00450](https://arxiv.org/abs/1905.00450). Scope: Encoding-specific bias-preserving gate constructions.
94. (2023) *Optical readout of a superconducting qubit using a piezo-optomechanical transducer*. [Preprint]. Available at: [https://arxiv.org/abs/2310.06026](https://arxiv.org/abs/2310.06026). Scope: Optical access to superconducting-qubit readout; measurement access is distinct from coherent logical state transfer.
95. Grinko, D., et al. (2019) *Iterative Quantum Amplitude Estimation*. [Preprint]. Available at: [https://arxiv.org/abs/1912.05559](https://arxiv.org/abs/1912.05559). Scope: Coherent amplification requirements for iterative estimation.
96. Das, A., et al. (2026) *Gate-Level Quantum Simulation of Nonunitary Linear Dynamics with Hybrid Oscillator-Qubit Architecture*. [Preprint]. Available at: [https://arxiv.org/abs/2605.10708](https://arxiv.org/abs/2605.10708). Scope: Specialized oscillator–qubit LCHS construction, P26.
97. Fressart, M., Nowak, M., and Spillane, N. (2026) *Quantum Domain Decomposition for Preconditioning the Finite Element Method*. [Preprint]. Available at: [https://arxiv.org/abs/2605.26090](https://arxiv.org/abs/2605.26090). Scope: Specified additive Schwarz construction, P26.
98. Wu, Y., and Li, Y. (2026) *Universal Dilation of Linear Itô SDEs*. [Preprint]. Available at: [https://arxiv.org/abs/2601.05928](https://arxiv.org/abs/2601.05928). Scope: Stochastic dilation and second-moment construction, P26.
99. Mirhosseini, M., et al. (2020) *Superconducting qubit to optical photon transduction*. Available at: [https://www.nature.com/articles/s41586-020-3038-6](https://www.nature.com/articles/s41586-020-3038-6). Scope: Microwave excitation to optical photon conversion as an interface ingredient; full coherent logical transfer requires additional characterization.
100. Harrow, Hassidim, and Lloyd, *Quantum algorithm for solving linear systems of equations*: [arXiv:0811.3171](https://arxiv.org/abs/0811.3171).
101. Gilyén, Su, Low, and Wiebe, *Quantum singular value transformation and beyond*: [arXiv:1806.01838](https://arxiv.org/abs/1806.01838).
102. Low and Chuang, *Optimal Hamiltonian Simulation by Quantum Signal Processing*: [Physical Review Letters 118, 010501](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.118.010501).
103. Low and Chuang, *Hamiltonian Simulation by Qubitization*: [Quantum 3, 163](https://quantum-journal.org/papers/q-2019-07-12-163/).
104. Bravo-Prieto et al., *Variational Quantum Linear Solver*: [Quantum 7, 1188](https://quantum-journal.org/papers/q-2023-11-22-1188/).
105. Kirby, Motta, and Mezzacapo, *Exact and efficient Lanczos method on a quantum computer*: [Quantum 7, 1018](https://quantum-journal.org/papers/q-2023-05-23-1018/).
106. Zhang et al., *Measurement-efficient quantum Krylov subspace diagonalisation*: [Quantum 8, 1438](https://quantum-journal.org/papers/q-2024-08-13-1438/).
107. Das et al., *Gate-Level Quantum Simulation of Nonunitary Linear Dynamics with Hybrid Oscillator-Qubit Architecture*: [arXiv:2605.10708v3](https://arxiv.org/abs/2605.10708v3). Cited for II.D. The initial preprint used the title *Quantum Differential Equation Solver via Hybrid Oscillator-Qubit Linear Combination of Hamiltonian Simulations*.
108. Fressart, Nowak, and Spillane, *Quantum Domain Decomposition for Preconditioning the Finite Element Method*: [arXiv:2605.26090](https://arxiv.org/abs/2605.26090). Cited for II.E, with a specified Poisson finite-element construction.
109. Wu and Li, *Universal Dilation of Linear Itô SDEs: Quantum Trajectories and Lindblad Simulation of Second Moments*: [arXiv:2601.05928v4](https://arxiv.org/abs/2601.05928v4); also published in [Quantum 10, 2223, 1 October 2026](https://quantum-journal.org/papers/q-2026-10-01-2223/). Cited for VII.E.
110. Balazi, Deiml, and Peterseim, *Quantum Enhanced Numerical Homogenization*: [arXiv:2603.28521v2](https://arxiv.org/abs/2603.28521v2). Cited for XVI.E, integrating classical coarse-scale solution with quantum fine-scale corrections.
111. Harrow, Hassidim, and Lloyd, *Quantum algorithm for solving linear systems of equations*: [arXiv:0811.3171](https://arxiv.org/abs/0811.3171). Supports the solution-state/observable distinction for sparse QLSAs.
112. Gilyén, Su, Low, and Wiebe, *Quantum singular value transformation and beyond*: [arXiv:1806.01838](https://arxiv.org/abs/1806.01838). Supports block-encoded transformations and pseudoinverse routes.
113. Kirby, Motta, and Mezzacapo, *Exact and efficient Lanczos method on a quantum computer*: [Quantum 7, 1018](https://quantum-journal.org/papers/q-2023-05-23-1018/). Supports block-encoded quantum Krylov/Lanczos workflows.
114. Das et al., *Gate-Level Quantum Simulation of Nonunitary Linear Dynamics with Hybrid Oscillator-Qubit Architecture*: [arXiv:2605.10708v3](https://arxiv.org/abs/2605.10708v3). P26 reference for branch 4.D. The initial title described oscillator–qubit LCHS.
115. Fressart, Nowak, and Spillane, *Quantum Domain Decomposition for Preconditioning the Finite Element Method*: [arXiv:2605.26090](https://arxiv.org/abs/2605.26090). P26 reference for the specified Additive Schwarz construction in branch 1.C.
116. Wu and Li, *Universal Dilation of Linear Itô SDEs: Quantum Trajectories and Lindblad Simulation of Second Moments*: [arXiv:2601.05928v4](https://arxiv.org/abs/2601.05928v4); also [Quantum 10, 2223](https://quantum-journal.org/papers/q-2026-10-01-2223/). P26 identifies the cited arXiv manuscript, which also has a journal publication dated 1 October 2026.
117. Balazi, Deiml, and Peterseim, *Quantum Enhanced Numerical Homogenization*: [arXiv:2603.28521v2](https://arxiv.org/abs/2603.28521v2). P26 reference for the specified essential quantum–classical workflow in branch 6.D.

[Back to contents](#contents)
