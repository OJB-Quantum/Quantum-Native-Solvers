# Quantum-Native-Solvers
A compilation of quantum-native solver techniques that can be used to map and run on a quantum computer. Compiled by Onri Jay Benally. 

---

The purpose of this repository is to monitor computational techniques (over time) that can be used to determine whether a working mathematical or computational framework of interest may be eligible for quantum simulation. 

Interestingly, some models such as the [Landau-Lifshitz-Gilbert (LLG) equation](https://iopscience.iop.org/article/10.1088/1367-2630/ae115c) (used in micromagnetism studies) can be systematically derived from [Lindbladian](https://en.wikipedia.org/wiki/Lindbladian) dynamics, which are based on the general form of Markovian master equations used to describe open quantum systems. In this example alone, LLG solvers are typically implemented on classical computing resources, especially that of general-purpose graphics processing units (GPUs). However, through careful derivation, supported by literature, such a method can transcend the classical description. Although this is not always the case for other classical or semi-classical frameworks and models, examples like these that are physics-informed or physics-supported should encourage one to explore the limits. 

---

- **Quantum-native** (adj.): designed for, and inherently dependent upon, quantum-mechanical resources, so that its core function, scaling, or correctness requires nonclassical phenomena (for example, superposition, interference, entanglement), rather than merely imitating them on classical hardware. 

- **quantum**: “Borrowed from Latin *quantum*,” historically ‘how much; as much as’ (neuter of *quantus*), later specialized to a discrete physical amount. - *Oxford English Dictionary*; see also Oxford’s gloss tying *quantum* to *quantus*. 
- **native**: “From Latin *nativus* (‘inborn; produced by birth’), via Middle English/French,” yielding senses such as ‘innate, natural; belonging by birth.’ - *Oxford English Dictionary*. 

---

## Classification Tree for Quantum-Native Simulation 

```
Quantum‑Native Simulation Techniques 
├─ I. Lattice‑kinetic/ Automata family
│   ├─ A. Quantum Lattice Boltzmann (QLBM)
│   │   ├─ A1. “Quantum-walk‑equivalent” formulations (QLBM ≈ quantum walk)
│   │   ├─ A2. Carleman‑linearized LB (LB-Carleman, fault‑tolerant oriented)
│   │   ├─ A3. Multi‑circuit QLBM for noisy devices
│   │   └─ A4. Early hardware demos (advection-diffusion on QPU)
│   ├─ B. Quantum Lattice Gas Automata (QLGA)
│   │   ├─ B1. Measurement‑based QLG for Navier-Stokes
│   │   ├─ B2. Efficient QLGA single‑step schemes
│   │   └─ B3. Fully‑quantum LGA building blocks (MHD/NS‑ready)
│   └─ C. Quantum Walks/ Quantum Cellular Automata (QCA)
│       ├─ C1. Dirac‑type dynamics (transport, diffusion, barriers & tunneling)
│       ├─ C2. Schrödinger‑type dynamics (continuous‑time QW ↔ free‑particle propagation,
│       │      discrete‑time QW approximating Laplacian)
│       └─ C3. Gauge‑field‑coupled QW/QCA (U(1), SU(2) links for both Dirac & Schrödinger)
│
├─ II. Quantum PDE & Linear‑Differential Equation solvers
│   ├─ D. Quantum Linear‑System Algorithms (HHL & block‑encoding, Poisson)
│   ├─ E. Linear Differential Equations (LDE) oracles (Berry-Childs-Kothari)
│   ├─ F. Carleman linearization for nonlinear ODE/PDE (Navier-Stokes regimes)
│   ├─ G. Quantum Finite Elements (Qu‑FEM/ Q‑FEM) + q‑Multigrid
│   ├─ H. Dirac‑PDE solvers
│   │   ├─ H1. QW‑based Dirac first‑order hyperbolic solvers
│   │   ├─ H2. QLGA spinor kernels for relativistic fluids
│   │   └─ H3. LCU/ qubitization of the Dirac operator (mass + kinetic terms)
│   └─ I. Schrödinger‑PDE solvers
│       ├─ I1. Spectral (Fourier) discretization + Trotter/LCU propagation
│       ├─ I2. Chebyshev‑polynomial and interaction‑picture schemes
│       └─ I3. Variational linear‑solver (VQLS) for stationary Schrödinger equation
│
├─ III. Variational & Hybrid Dynamics
│   ├─ J. Variational Quantum Simulation (VQS: real & imaginary time)
│   ├─ K. Quantum Imaginary‑Time Evolution (QITE) + accelerators
│   ├─ L. Variational Quantum Linear Solver (VQLS) for scattering/PDE
│   ├─ M. Quantum Car‑Parrinello Molecular Dynamics (QCPMD, NISQ)
│   ├─ N. Variational Dirac solvers
│   │   ├─ N1. Real‑time VQS for Dirac spinor dynamics
│   │   └─ N2. QITE for massive Dirac ground‑state preparation
│   └─ O. Variational Schrödinger solvers
│       ├─ O1. Real‑time VQS with hardware‑efficient ansätze for non‑relativistic dynamics
│       └─ O2. Imaginary‑time VQS/ QITE for ground‑state Schrödinger problems
│
├─ IV. Hamiltonian Simulation & Scattering
│   ├─ P. Real‑space electron‑dynamics (LCU/ qubitization/ Trotter)
│   ├─ Q. Scattering & cross‑section algorithms (Green’s functions, S‑matrix)
│   ├─ R. Quantum‑walk tunneling/ barrier transport
│   ├─ S. Dirac‑Hamiltonian simulation
│   │   ├─ S1. Product‑formula (Trotter‑Suzuki) for γ·p + m terms
│   │   ├─ S2. LCU/ qubitization of relativistic kinetic operator
│   │   └─ S3. Interaction‑picture and Dyson‑series methods for time‑dependent Dirac fields
│   └─ T. Schrödinger‑Hamiltonian simulation
│       ├─ T1. Finite‑difference/plane‑wave discretization + Trotter
│       ├─ T2. LCU/ qubitization of kinetic + potential operators
│       └─ T3. Interaction‑picture schemes for driven potentials
│
├─ V. Stochastic & Monte Carlo family
│   ├─ O. Quantum Monte Carlo (classically executed, physics‑native)
│   │   ├─ O1. Variational Monte Carlo (VMC; |Ψ|² sampling)
│   │   ├─ O2. Diffusion Monte Carlo (DMC; imaginary‑time projection, fixed‑node)
│   │   ├─ O3. Path‑Integral QMC (PIMC; finite‑T & ground‑state PIGS)
│   │   ├─ O4. Auxiliary‑Field QMC (AFQMC; Hubbard-Stratonovich)
│   │   └─ O5. Stochastic Series Expansion (SSE; loop/cluster updates)
│   ├─ P. Quantum‑accelerated Monte Carlo (hardware‑native)
│   │   ├─ P1. Quantum Amplitude Estimation (QAE; quadratic sampling speedups)
│   │   ├─ P2. Quantum Metropolis sampling (thermal states without sign problem)
│   │   └─ P3. Hybrid QMC w/ QC‑generated trials (e.g., QSCI → AFQMC)
│   └─ Q. Quantum Trajectories (Monte Carlo wave function, stochastic Schrödinger)
│       ├─ Q1. Quantum jumps/ MCWF unraveling of Lindblad
│       └─ Q2. Adiabatic‑master‑equation trajectories; restricted‑MCWF diagnostics
│   ├─ R. Schrödinger‑QMC variants
│   │   ├─ R1. Diffusion Monte Carlo with quantum‑enhanced trial‑wave‑function optimization
│   │   └─ R2. Path‑Integral QMC for real‑time (Keldysh) Schrödinger evolution
│   └─ S. Dirac‑QMC variants
│       ├─ S1. Stochastic Dirac equation (stochastic quantization of relativistic fields)
│       └─ S2. Quantum‑walk based Monte Carlo for relativistic barrier tunneling
│
├─ VI. Open‑System (Lindblad) solvers & Magnetization links
│   ├─ R. Lindblad simulators on quantum hardware
│   │   ├─ R1. LCU/ Stinespring‑dilation & qubitization‑style algorithms
│   │   ├─ R2. Randomized product‑formula & sampling compilers for GKSL
│   │   └─ R3. Quantum‑trajectory simulators with additive O(T + log 1/ε) scaling (class‑dependent)
│   ├─ S. Classical‑HPC Lindblad solvers with quantum trajectories (large‑N pure‑state sampling)
│   └─ T. Lindblad → LL/LLG mappings for magnetization dynamics
│       ├─ T1. LL from Lindbladian dynamics (scale‑separated local mean‑field regimes)
│       ├─ T2. Microscopic LLG coefficients via NEGF/scattering formulations
│       └─ T3. Quantum‑LLG analogs and constraints from entanglement/ non‑Markovianity
│
├─ VII. Emerging quantum‑native simulation families
│   ├─ A. Quantum Tensor‑Network Simulators (MPS, PEPS, TTN) for PDEs & many‑body states
│   ├─ B. Quantum Signal Processing/ QSVT‑based PDE solvers
│   │        (Chebyshev and polynomial transformations for evolution and linear systems)
│   ├─ C. Fractional & Non‑local Operator Solvers
│   │        (fractional Laplacian, Riesz derivatives, etc.)
│   ├─ D. Quantum Stochastic Differential Equations (QSDE) &
│   │        non‑Markovian open‑system dynamics
│   ├─ E. Quantum Lattice‑Gauge‑Theory Engines
│   │        (U(1), SU(2), SU(3) gauge fields, plaquette unitaries)
│   ├─ F. Quantum‑Accelerated Adaptive‑Mesh‑Refinement (AMR)
│   │        (amplitude‑estimation‑driven error indicators)
│   ├─ G. Quantum‑Enhanced Real‑Time Time‑Dependent Density‑Functional Theory (RT‑TDDFT)
│   ├─ H. Quantum Wigner‑Function/ Phase‑Space Solvers
│   │        (discrete Wigner representation + FFT‑based evolution)
│   ├─ I. Quantum Uncertainty Quantification (UQ) for PDEs
│   │        (parameter‑sampling via QAE)
│   ├─ J. Quantum Spectral‑Element/ hp‑Finite‑Element Methods
│   │        (high‑order basis encoded in qubit registers)
│   ├─ K. Quantum Metropolis‑Hastings for Thermodynamic Ensembles of PDE Solutions
│   ├─ L. Hybrid Quantum‑Classical Domain Decomposition (Schwarz) for large‑scale PDEs
│   ├─ M. Quantum‑Accelerated Sparse‑Matrix Preconditioners
│   │        (quantum BiCGSTAB, quantum multigrid)
│   └─ N. Quantum Embedding & Multi‑Scale Methods
│           (DMFT, QM/MM, impurity solvers, quantum‑accelerated self‑energy evaluation)
│
├─ VIII. Dirac Equation Solvers (relativistic quantum dynamics)
│   ├─ A. Quantum‑Walk‑based Dirac solvers
│   │   ├─ A1. 1‑D split‑step Dirac QW (continuous limit → Dirac equation)
│   │   ├─ A2. 2‑D/ 3‑D multi‑step Dirac QWs (including spin‑orbit coupling)
│   │   ├─ A3. Gauge‑field‑coupled Dirac QWs (U(1) electromagnetic, SU(2) weak)
│   │   └─ A4. Multi‑circuit Dirac QWs for NISQ devices (error‑mitigated implementations)
│   ├─ B. Quantum Lattice‑Gas Automata for Dirac dynamics
│   │   ├─ B1. Spinor QLGA kernels (γ‑matrix encoding on qubits)
│   │   ├─ B2. Dirac-Maxwell QLGA (self‑consistent coupling of spinor and EM fields)
│   │   └─ B3. Relativistic fluid‑LGA (Dirac‑MHD, relativistic Navier-Stokes)
│   ├─ C. Hamiltonian‑simulation of the Dirac operator
│   │   ├─ C1. Trotter‑Suzuki product formulas for γ·p + m terms
│   │   ├─ C2. LCU/ qubitization of the Dirac Hamiltonian (including mass‑gap)
│   │   ├─ C3. Interaction‑picture/ Dyson‑series Dirac simulators for time‑dependent fields
│   │   └─ C4. Spectral‑method (Fourier) Dirac simulation (momentum‑space discretization)
│   ├─ D. Variational & Imaginary‑time Dirac solvers
│   │   ├─ D1. Real‑time VQS for Dirac spinors (hardware‑efficient ansätze)
│   │   ├─ D2. QITE for massive Dirac ground‑state preparation
│   │   └─ D3. Adaptive‑VQE style Dirac dynamics (ADAPT‑VQD)
│   ├─ E. Carleman‑linearized Dirac‑type nonlinear models
│   │   ├─ E1. Dirac-Klein‑Gordon coupling (scalar‑field interactions)
│   │   ├─ E2. Non‑linear Dirac (Thirring, Gross‑Neveu) via Carleman embedding
│   │   └─ E3. Dirac-Maxwell-Higgs hybrid systems
│   ├─ F. Quantum Finite‑Element/ Multigrid for Dirac
│   │   ├─ F1. QFEM‑Dirac (mesh‑based spinor discretization)
│   │   └─ F2. Dirac‑multigrid preconditioners realized with quantum subroutines
│   ├─ G. Scattering & S‑matrix for relativistic particles
│   │   ├─ G1. Green‑function based Dirac cross‑section algorithms
│   │   ├─ G2. Relativistic quantum‑walk tunneling analysis (Klein paradox)
│   │   └─ G3. Dirac‑time‑dependent scattering (time‑varying potentials)
│   └─ H. Hybrid classical‑quantum Dirac workflows
│       ├─ H1. Classical spectral Dirac solver + quantum subroutines for high‑frequency modes
│       └─ H2. Domain‑decomposition: QC solves interior relativistic region, classical for exterior
│
└─ IX. Schrödinger Equation Solvers (non‑relativistic quantum dynamics)
    ├─ A. Direct Hamiltonian‑simulation of Schrödinger dynamics
    │   ├─ A1. Spatial discretization (finite‑difference, plane‑wave) + Trotter‑Suzuki
    │   ├─ A2. LCU/ qubitization of kinetic (∇²) + potential V(x) operators
    │   ├─ A3. Interaction‑picture/ Dyson‑series approaches for time‑dependent potentials
    │   └─ A4. High‑order product formulas (e.g., Forest‑Ruth, Suzuki‑23) for improved accuracy
    ├─ B. Quantum‑PDE solvers specialized for Schrödinger
    │   ├─ B1. Linear‑Differential‑Equation oracle tailored to time‑dependent Schrödinger equation
    │   ├─ B2. Spectral (Fourier) method via Quantum Fourier Transform (QFT) - plane‑wave basis
    │   ├─ B3. Chebyshev‑polynomial expansion of the propagator e^{-iHt}
    │   └─ B4. Pseudo‑spectral split‑operator schemes (FFT‑based) on quantum hardware
    ├─ C. Variational & Hybrid Schrödinger solvers
    │   ├─ C1. Real‑time VQS with hardware‑efficient ansätze (e.g., hardware‑efficient, ADAPT‑VQE)
    │   ├─ C2. Imaginary‑time VQS/ QITE for ground‑state preparation of Schrödinger Hamiltonians
    │   ├─ C3. Variational Quantum Linear Solver (VQLS) for stationary Schrödinger equation (Ax = b with A = H−E)
    │   └─ C4. Adaptive‑VQD for excited‑state dynamics
    ├─ D. Quantum‑Walk implementations of Schrödinger dynamics
    │   ├─ D1. Continuous‑time quantum walk mapping to free‑particle Schrödinger (H = −Δ)
    │   ├─ D2. Discrete‑time quantum walk reproducing Laplacian kinetic term via coin‑shift construction
    │   └─ D3. Quantum‑walk based tunneling simulations for arbitrary barrier profiles
    ├─ E. Quantum Cellular Automata for Schrödinger equation
    │   ├─ E1. Local unitary update rules approximating the Laplacian operator
    │   └─ E2. Multi‑step QCA with on‑site potential encoding
    ├─ F. Quantum Monte Carlo methods for Schrödinger equation
    │   ├─ F1. Diffusion Monte Carlo (imaginary‑time projection) with QC‑accelerated trial‑wave‑function refinement
    │   ├─ F2. Path‑Integral QMC for real‑time evolution (Keldysh contour) using quantum‑enhanced sampling
    │   ├─ F3. Variational Monte Carlo with quantum‑generated ansätze (QANSAT)
    │   └─ F4. Reptation QMC with quantum‑assisted move proposals
    ├─ G. Many‑body Schrödinger solvers (second‑quantized)
    │   ├─ G1. Electronic‑structure Hamiltonian simulation (quantum chemistry) via LCU/ qubitization
    │   ├─ G2. Quantum Dynamical Mean‑Field Theory (QDMFT) for lattice models
    │   ├─ G3. Real‑time Green‑function methods (Kadanoff‑Baym) on quantum processors
    │   └─ G4. Tensor‑network‑inspired hybrid algorithms (e.g., VQE‑DMRG hybrids)
    ├─ H. Hybrid Classical‑Quantum Schrödinger solvers
    │   ├─ H1. Classical spectral (Fourier) solver + quantum subroutine for high‑frequency phase propagation
    │   ├─ H2. Domain‑decomposition: QC handles sub‑grid potentials; classical handles bulk
    │   └─ H3. Multi‑scale coupling of quantum‑accelerated short‑range dynamics with classical long‑range Poisson solver
    ├─ I. Scattering & S‑matrix for non‑relativistic particles
    │   ├─ I1. Green‑function based cross‑section computation (Lippmann‑Schwinger equation)
    │   ├─ I2. Quantum‑walk tunneling analysis for arbitrary 1‑D/ 2‑D barrier profiles
    │   └─ I3. Real‑time S‑matrix extraction via phase‑estimation on scattered wave‑packets
    └─ J. Quantum Finite‑Element/ Multigrid for Schrödinger
        ├─ J1. Mesh‑based Schrödinger solvers with quantum enforcement of continuity/ boundary conditions
        ├─ J2. Quantum‑accelerated multigrid preconditioners for the Laplacian‑plus‑potential operator
        └─ J3. Adaptive mesh refinement driven by quantum‑measured error estimates
```

---

## **Quantum‑Native Simulation Techniques - Ordered by Hardware‑Readiness**  


## I. NISQ‑ready (shallow‑depth, few‑qubit) techniques  
These algorithms can be executed today on IBM Q, Rigetti, IonQ, or any other gate‑model device with error‑mitigation.  All required primitives are already available in Qiskit ≥ 2.2 (e.g. `QuantumWalkCircuit`, `VQSDynamics`, `QITE`, `AmplitudeEstimation`, `LindbladEvolution`).  

| Category | Techniques  |
|----------|-----------------------|
| **A. Quantum walks & quantum cellular automata** | 1. Dirac‑type dynamics (split‑step quantum walk) <br> 2. Schrödinger‑type dynamics (continuous‑time QW + discrete‑time Laplacian QW) <br> 3. Simple U(1) gauge‑field coupling (single‑link) |
| **B. Quantum lattice‑Boltzmann/ lattice‑gas (toy lattices)** | 1. “Quantum-walk‑equivalent’’ QLBM <br> 2. Early hardware demos (advection-diffusion on a few‑site lattice) <br> 3. Efficient single‑step QLGA for 1‑D/2‑D Navier-Stokes (few sites) |
| **C. Variational & hybrid dynamics** | 1. Real‑time Variational Quantum Simulation (VQS) <br> 2. Quantum Imaginary‑Time Evolution (QITE) <br> 3. Variational Quantum Linear Solver (VQLS) for small linear systems <br> 4. Real‑time VQS for Dirac or Schrödinger spinors with hardware‑efficient ansätze <br> 5. Imaginary‑time VQS/ QITE for Dirac or Schrödinger ground‑state preparation |
| **D. Direct Hamiltonian‑simulation (small grids)** | 1. Spectral (Fourier) discretization + Trotter for 1‑D Schrödinger <br> 2. QW‑based Dirac first‑order hyperbolic solver (few lattice sites) <br> 3. Quantum‑walk tunnelling/ barrier transport (single‑step QW) |
| **E. Simple linear‑system/ HHL demos** | 1. HHL for a 2 × 2 or 4 × 4 Poisson‑type matrix (demonstration only) |
| **F. Quantum‑accelerated Monte Carlo (basic)** | 1. Iterative Amplitude Estimation (IAE) for expectation‑value speed‑up <br> 2. Quantum jump/ Monte‑Carlo wave‑function (MCWF) for a single Lindblad trajectory (few qubits) |
| **G. Minimal Lindblad simulators** | 1. Randomised product‑formula simulation of short‑time GKSL dynamics (≤ 5 qubits) |
| **H. Other NISQ‑ready families** | 1. Quantum tensor‑network simulators (MPS/PEPS with shallow circuits) <br> 2. QSP/QSVT for low‑degree polynomial approximations (e.g. low‑order Chebyshev filters) <br> 3. Quantum algorithms for fractional Laplacian on very small lattices (Fourier‑space LCU) <br> 4. Stochastic Schrödinger‑equation discretizations for a few modes (QSDE prototype) |

---  

## II. Near‑term/ hybrid (requires modest error‑correction, multi‑circuit layouts, or classical‑quantum co‑processing)  
These approaches are still limited on today’s noisy devices but become practical with modest error‑mitigation, circuit parallelism, or when used as sub‑routines inside a classical workflow (e.g. hybrid QMC, domain‑decomposition).  

| Category | Techniques  |
|----------|-----------------------|
| **A. Multi‑circuit lattice‑kinetic methods** | 1. Multi‑circuit QLBM for noisy devices (parallel streams) <br> 2. Measurement‑based QLGA for Navier-Stokes (classical feedback after each step) |
| **B. Linear‑differential‑equation & Carleman‑linearization prototypes** | 1. LDE oracles (Berry-Childs-Kothari) as proof‑of‑concept <br> 2. Carleman linearization for low‑order Navier-Stokes regimes (small lifted dimension) |
| **C. Quantum finite‑element/ multigrid (tiny meshes)** | 1. Qu‑FEM/ Q‑FEM for 2‑D Poisson on ≤ 4 × 4 meshes <br> 2. QFEM‑Dirac for a 2‑D spinor mesh (≤ 8 qubits) |
| **D. LCU/ qubitization for modest problem sizes** | 1. LCU/ qubitization of the Dirac operator on a 2 × 2 lattice <br> 2. VQLS for stationary Schrödinger on ≤ 8‑dimensional basis |
| **E. Hamiltonian simulation (intermediate depth)** | 1. Trotter‑Suzuki product formula for Dirac kinetic + mass on a 3‑site lattice <br> 2. Finite‑difference Schrödinger Trotter on a 4‑site grid |
| **F. Hybrid quantum‑accelerated Monte Carlo** | 1. Prototype Quantum Metropolis (single‑spin Ising) <br> 2. Hybrid QMC where the quantum computer supplies trial wavefunctions for AFQMC <br> 3. QAE‑based variance reduction inside Diffusion Monte Carlo <br> 4. Stochastic Dirac‑equation sampling on a few momentum modes |
| **G. Quantum trajectories with modest scaling** | 1. Additive O(T + log 1/ε) trajectory algorithm for a 2‑qubit Lindbladian (research prototype) |
| **H. Quantum Car‑Parrinello Molecular Dynamics (NISQ prototype)** | 1. QCPMD for a single‑atom, few‑electron system (small basis) |
| **I. Other near‑term families** | 1. Quantum lattice‑gauge‑theory engines for 1‑D U(1) link models (few links) <br> 2. Quantum‑accelerated adaptive‑mesh‑refinement on a 2‑D grid (error estimate via IAE) <br> 3. Quantum‑enhanced real‑time TDDFT for a 2‑electron molecule (VQE‑derived Kohn‑Sham potential) <br> 4. Wigner‑function evolution on a 2‑point phase‑space (QFT‑based) <br> 5. Quantum‑accelerated uncertainty quantification for a 1‑D diffusion PDE (QAE sampling of input parameters) |

---  

## III. Fault‑tolerant eligible (requires error‑corrected hardware, large ancilla registers, deep circuits)  
These algorithms deliver asymptotic speed‑ups or polynomial‑time reductions that become advantageous only when a full‑scale fault‑tolerant quantum computer is available.  Qiskit provides the low‑level building blocks (`LinearCombinationOfUnitaries`, `SelectOracle`, `PhaseEstimation`, `QSVT`), but high‑level wrappers are still under development.  

| Category | Techniques  |
|----------|-----------------------|
| **A. Large‑scale linear‑system solvers** | 1. Full‑scale HHL/ block‑encoding for Poisson matrices of size 2ⁿ × 2ⁿ <br> 2. Fault‑tolerant VQLS for high‑dimensional Schrödinger eigenproblems |
| **B. Dirac & Schrödinger Hamiltonian simulation (qubitization)** | 1. Qubitization of the relativistic kinetic operator for 3‑D lattices <br> 2. Qubitization of kinetic + potential for high‑resolution Schrödinger grids <br> 3. Full LCU implementation of the Dirac Hamiltonian (mass + gauge coupling) <br> 4. LCU/ qubitization of Schrödinger kinetic + potential |
| **C. Scattering & S‑matrix algorithms** | 1. Green’s‑function based cross‑section computation using a quantum resolvent (QPE or QSVT) <br> 2. Dirac scattering (Klein‑paradox) with QSVT‑based resolvent <br> 3. Non‑relativistic Lippmann‑Schwinger solver via QSVT |
| **D. Quantum signal‑processing/ QSVT frameworks** | 1. High‑degree Chebyshev and polynomial approximations for time‑evolution and matrix inversion (generic PDEs) <br> 2. QSP‑based fractional‑operator solvers for non‑local dynamics |
| **E. Quantum lattice‑gauge‑theory (full models)** | 1. Digital simulation of 2‑D/3‑D U(1) and SU(2) gauge fields with plaquette operators (requires many qubits and T‑gate depth) |
| **F. Quantum stochastic differential equations & non‑Markovian open systems** | 1. QSDE simulation using Stinespring dilation and phase‑estimation (large ancilla) <br> 2. LCU/ Stinespring dilation of general GKSL generators |
| **G. Quantum‑accelerated multigrid & preconditioners** | 1. Quantum multigrid V‑cycle as a sub‑routine for linear‑system solvers (requires fault‑tolerant VQLS) <br> 2. Quantum‑accelerated sparse‑matrix preconditioners (quantum BiCGSTAB, quantum multigrid) |
| **H. Quantum‑enhanced adaptive‑mesh‑refinement** | 1. AMR driven by quantum‑estimated error norms (QAE required at each refinement step) |
| **I. High‑order spectral‑element/ hp‑FEM** | 1. Quantum spectral‑element method with hp‑refinement (mesh encoded in qubits, block‑encoded stiffness matrices) |
| **J. Quantum tensor‑network simulation at scale** | 1. Large‑scale MPS/PEPS evolution using quantum circuits (requires fault‑tolerant resources for bond‑dimension growth) |
| **K. Quantum Metropolis‑Hastings for generic distributions** | 1. Full‑scale Quantum Metropolis algorithm (detailed‑balance via phase estimation) |
| **L. Hybrid quantum‑classical domain decomposition** | 1. Schwarz‑type decomposition where each subdomain is solved with a fault‑tolerant quantum linear‑solver and the interface is handled classically |
| **M. Quantum‑enhanced real‑time TDDFT & many‑body dynamics** | 1. Fully fault‑tolerant RT‑TDDFT where the Kohn‑Sham potential and the electron density are updated via quantum sub‑routines (qubitization of the KS Hamiltonian) |
| **N. Wigner‑function/ phase‑space quantum dynamics** | 1. Exact quantum‑Fourier‑transform‑based evolution of high‑dimensional Wigner functions (large QFT registers required) |
| **O. Quantum uncertainty quantification for large‑scale PDEs** | 1. Propagation of input‑parameter distributions through quantum‑accelerated forward models (QAE + QSVT) |
| **P. Quantum‑accelerated scattering with many‑body final states** | 1. Dirac time‑dependent scattering with multi‑particle production (requires fault‑tolerant many‑body state preparation) |
| **Q. Quantum‑enhanced Linear‑Landau‑Lifshitz‑Gilbert (LLG) mapping** | 1. Extraction of microscopic LLG coefficients from entanglement‑aware quantum simulations (requires full open‑system simulation + phase estimation) |

---  

## An Example Architecture of What a Fault-Tolerant Heterogenous/ Hybrid Quantum System Could Look Like - by Onri 

```
Heterogeneous Quantum Computer (Architected for Fault-Tolerant Compatibility)
├─ Main QPU: Neutral-Atom (Rydberg/ tweezer)
│   ├─ Fast parallel CZ gates (~99.5%); long coherence (≈12.6 s, hyperfine)
│   ├─ Reconfigurable layouts; mid-circuit measurement & erasure
│   └─ On-chip nanophotonics for coupling/ imaging
├─ Co-Processor: Quantum Photonic IC (QPIC)
│   ├─ Time-bin/ cluster-state generation; fusion operations
│   ├─ Waveguide arrays, thin-film LiNbO₃ modulators, PNR detectors
│   └─ Fiber network to other racks/ fridges
├─ Translator(s): μw ↔ optical
│   ├─ Electro-optic (LiNbO₃), opto-mechanical, Rydberg ensembles
│   ├─ Targets: internal η ≥ 0.1-0.5, added noise ≲ 1 photon
│   └─ With JPA/ JTWPA pre-amps (paramps), pump-noise filtering
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
    ├─ Cryo-CMOS SRAM/ FBRAM/ MRAM/ GC-eDRAM near 4-12 K; microcode/ waveform cache
    ├─ Single-Flux-Quantum Digital (4 K stage; higher-integration families)
    │   ├─ RSFQ (DC-biased; ultrafast legacy baseline; static-power overhead); reference-grade timing/ waveform source or local clocks
    │   ├─ eSFQ/ ERSFQ (DC-biased, zero static power; preferred for scalable SFQ logic & SFQ-DACs/ JAWS)
    │   └─ RQL (AC-powered; no on-chip static power; multi-phase AC clock/ power; AC/ DC converters as needed)
    │       • SFQ-based AWG/ DAC for qubit drive (JAWS); isolate to mitigate quasiparticles; near-deterministic timing
    └─ Compiler for biased/ erasure noise and transduction-aware routing
```

---

## Decision Tree to Determine Which Quantum-Native Simulation Techniques Could Run on an Proposed Fault-Tolerant Heterogenous Quantum Computer 

```
Fault‑tolerant heterogeneous/hybrid quantum system
├─ Main QPU: Neutral‑Atom (Rydberg/ tweezer)
│   ├─ NISQ‑ready techniques (shallow circuits, few qubits)
│   │   ├─ I.C.1  Dirac‑type quantum‑walk/ QCA
│   │   ├─ I.C.2  Schrödinger‑type quantum‑walk/ QCA
│   │   ├─ I.C.3  Simple gauge‑field‑coupled QW/QCA (U(1) link)
│   │   ├─ I.A.1  “QW‑equivalent’’ Quantum Lattice‑Boltzmann (QLBM)
│   │   ├─ I.B.3  Fully‑quantum Lattice‑Gas Automata (MHD/NS‑ready)
│   │   ├─ III.J  Variational Quantum Simulation (VQS, real‑time)
│   │   ├─ III.K  Quantum Imaginary‑Time Evolution (QITE)
│   │   ├─ III.L  Variational Quantum Linear Solver (VQLS, small systems)
│   │   ├─ III.N.1 Real‑time VQS for Dirac spinors
│   │   ├─ III.O.1 Real‑time VQS for Schrödinger spinors
│   │   ├─ IV.P   Real‑space electron‑dynamics (short‑depth Trotter)
│   │   ├─ IV.R   Quantum‑walk tunnelling/ barrier transport
│   │   ├─ V.P1   Iterative Amplitude Estimation (IAE)
│   │   └─ VI.R2  Randomised product‑formula GKSL simulation (≤ 5 qubits)
│   │
│   ├─ Near‑term/ hybrid techniques (moderate error‑correction, multi‑circuit)
│   │   ├─ I.A.3  Multi‑circuit QLBM for noisy devices (parallel streams)
│   │   ├─ I.B.1  Measurement‑based QLGA for Navier-Stokes (mid‑circuit readout)
│   │   ├─ II.E   Linear‑Differential‑Equation (LDE) oracles (Berry‑Childs‑Kothari)
│   │   ├─ II.F   Carleman‑linearized ODE/PDE prototypes (low‑order Navier-Stokes)
│   │   ├─ II.G   Quantum Finite‑Element/ Q‑FEM on tiny meshes (≤ 4×4)
│   │   ├─ II.H.1 QW‑based Dirac first‑order hyperbolic solver (few sites)
│   │   ├─ II.I.1 Spectral Schrödinger solver (small Fourier grid)
│   │   ├─ III.M   Quantum Car‑Parrinello MD (prototype, few electrons)
│   │   ├─ IV.S.1 Trotter‑Suzuki Dirac kinetic + mass (3‑site lattice)
│   │   ├─ IV.T.1 Finite‑difference Schrödinger Trotter (4‑site grid)
│   │   ├─ V.P2   Prototype Quantum Metropolis sampler (single‑spin Ising)
│   │   ├─ V.P3   Hybrid QMC - QC‑generated trial wavefunctions for AFQMC
│   │   └─ VI.R3   Additive O(T + log 1/ε) quantum‑trajectory algorithm (research demo)
│   │
│   └─ Fault‑tolerant‑eligible techniques (require logical qubits, bias‑preserving gates)
│       ├─ II.D    Full‑scale HHL/ block‑encoding linear‑system solver
│       ├─ II.E    Large‑scale LDE oracle (high‑precision phase estimation)
│       ├─ II.F    Complete Carleman‑linearization for nonlinear PDEs
│       ├─ II.G    Full Qu‑FEM + quantum multigrid preconditioner
│       ├─ IV.S.2  Qubitization of Dirac kinetic operator (3‑D lattice)
│       ├─ IV.T.2  Qubitization of Schrödinger kinetic + potential
│       ├─ IV.Q    Scattering & S‑matrix via Green’s‑function resolvent (QSVT/QSP)
│       ├─ IV.S.3  High‑order Dirac product‑formula (Forest‑Ruth, Suzuki‑23)
│       ├─ IV.T.3  Interaction‑picture Schrödinger simulation (time‑dependent V)
│       ├─ V.S.1   Stochastic Dirac‑equation (QSDE) Monte Carlo
│       ├─ V.R.1   Diffusion Monte Carlo with QC‑enhanced trial wavefunctions
│       ├─ VI.R1   LCU/ Stinespring‑dilation implementation of GKSL Lindbladians
│       ├─ VI.T1‑T3  Lindblad → LL/ LLG mappings (microscopic coefficient extraction)
│       ├─ VII.A.*  All Dirac‑walk families (A1-A4)
│       ├─ VII.B.*  Dirac QLGA kernels (B1-B3)
│       ├─ VII.C.*  Dirac Hamiltonian simulation (C1-C4)
│       ├─ VII.D.*  Variational & imaginary‑time Dirac solvers (D1-D3)
│       ├─ VII.E.*  Carleman‑linearized Dirac nonlinear models (E1-E3)
│       ├─ VII.F.*  Dirac FEM/ multigrid (F1-F2)
│       ├─ VII.G.*  Dirac scattering & S‑matrix (G1-G3)
│       ├─ VII.H.*  Hybrid classical‑quantum Dirac workflows (H1-H2)
│       ├─ VIII.A.*  Schrödinger Hamiltonian simulation (A1-A4)
│       ├─ VIII.B.*  Schrödinger‑specific QPDE solvers (B1-B4)
│       ├─ VIII.C.*  Variational Schrödinger techniques (C1-C4)
│       ├─ VIII.D.*  Quantum‑walk implementations of Schrödinger dynamics (D1-D3)
│       ├─ VIII.E.*  Quantum Cellular Automata for Schrödinger (E1-E2)
│       ├─ VIII.F.*  Schrödinger Quantum Monte Carlo (F1-F4)
│       ├─ VIII.G.*  Many‑body Schrödinger solvers (G1-G4)
│       ├─ VIII.H.*  Hybrid classical‑quantum Schrödinger workflows (H1-H3)
│       ├─ VIII.I.*  Scattering & S‑matrix for non‑relativistic particles (I1-I3)
│       └─ VIII.J.*  Schrödinger FEM/ multigrid (J1-J3)
│
├─ Co‑Processor: Quantum Photonic IC (QPIC)
│   ├─ NISQ‑ready photonic techniques
│   │   ├─ I.C.1   Dirac‑type quantum walk in time‑bin encoding
│   │   ├─ I.C.2   Schrödinger‑type continuous‑time quantum walk
│   │   ├─ I.B.1   Measurement‑based QLGA (cluster‑state generation)
│   │   └─ V.P1    Iterative Amplitude Estimation with single‑photon detectors
│   │
│   ├─ Near‑term/ hybrid photonic techniques
│   │   ├─ V.P2    Quantum Metropolis sampler realised with linear‑optical interferometers
│   │   ├─ V.P3    Hybrid QMC - photonic circuit produces trial wavefunctions for AFQMC
│   │   └─ II.E    Prototype photonic LDE oracle (cluster‑state linear ODE)
│   │
│   └─ Fault‑tolerant‑eligible photonic techniques
│       ├─ VI.R1   LCU/ Stinespring‑dilation of GKSL Lindbladians using bosonic cat encodings
│       └─ VIII.F.1 Bosonic Diffusion Monte Carlo (photon‑number‑encoded walkers)
│
├─ Translator(s)  μw ↔ optical
│   └─ Enables cross‑platform operations:
│       ├─ Swap neutral‑atom‑prepared states into the photonic QPIC for measurement‑based QLGA
│       ├─ Transfer photonic trial wavefunctions into the neutral‑atom QPU for VQS/ QITE loops
│       └─ Carry Lindblad jump outcomes between the two layers for hybrid trajectory simulations
│
├─ Quantum Memory: Superconducting Cat (bosonic)
│   ├─ Near‑term/ hybrid use
│   │   └─ Store intermediate probability distributions for multi‑circuit QLBM (I.A.3) between Trotter steps
│   └─ Fault‑tolerant‑eligible use
│       ├─ Logical storage for large‑scale HHL/VQLS registers (II.D, III.L)
│       ├─ Buffer for Dirac/ Schrödinger qubitization registers (IV.S.2, IV.T.2)
│       ├─ Hold error‑estimate registers for quantum‑driven AMR (VII.F.2, VIII.J.3)
│       └─ Preserve many‑body wavefunctions for quantum‑dynamical mean‑field theory (VIII.G.2)
│
├─ Ancilla Layer: Transmons (bias‑preserving, syndrome extraction)
│   └─ Provide fault‑tolerant CZ/ZZ and SNAP‑enabled gates used by all LCU/ qubitization kernels in the FT‑eligible branch
│
└─ Classical Control & RAM
    ├─ Cryogenic‑CMOS/ SFQ waveform engines compile bias‑aware, erasure‑aware circuits for every technique
    ├─ Run the outer optimization loops for VQS/ QITE/ VQLS (III.J-III.O)
    ├─ Dispatch hybrid QMC/ Metropolis proposals to the photonic co‑processor
    └─ Coordinate domain‑decomposition (VIII.H, VII.H) and adaptive‑mesh‑refinement decisions (VII.F.2, VIII.J.3)
```

---

## References

1.  Wang, B., Meng, Z., Zhao, Y. and Yang, Y. (2025) *Quantum lattice Boltzmann method for simulating nonlinear fluid dynamics*. \[Preprint]. arXiv:2502.16568. Available at: [https://arxiv.org/abs/2502.16568](https://arxiv.org/abs/2502.16568).
2.  Georgescu, C.A., Schalkers, M.A. and Möller, M. (2024) *qlbm – A Quantum Lattice Boltzmann Software Framework*. \[Preprint]. arXiv:2411.19439. Available at: [https://arxiv.org/abs/2411.19439](https://arxiv.org/abs/2411.19439).
3.  Meyer, D.A. (1996) 'From quantum cellular automata to quantum lattice gases', *Journal of Statistical Physics*, 85, pp. 551–574. Available at: [https://doi.org/10.1007/BF02199356](https://doi.org/10.1007/BF02199356).
4.  Strauch, F.W. (2006) 'Relativistic quantum walks', *Physical Review A*, 73(5), 054302. Available at: [https://doi.org/10.1103/PhysRevA.73.054302](https://doi.org/10.1103/PhysRevA.73.054302).
5.  Arnault, P., Di Molfetta, G., Brachet, M. and Debbasch, F. (2016) 'Quantum walks and non-Abelian discrete gauge theory', *Physical Review A*, 94(1), 012335. Available at: [https://doi.org/10.1103/PhysRevA.94.012335](https://doi.org/10.1103/PhysRevA.94.012335).
6.  Márquez-Martín, I., Arnault, P., Di Molfetta, G. and Pérez, A. (2018) 'Electromagnetic lattice gauge invariance in two-dimensional discrete-time quantum walks', *Physical Review A*, 98(3), 032333. Available at: [https://doi.org/10.1103/PhysRevA.98.032333](https://doi.org/10.1103/PhysRevA.98.032333).
7.  Harrow, A.W., Hassidim, A. and Lloyd, S. (2009) 'Quantum algorithm for linear systems of equations', *Physical Review Letters*, 103(15), 150502. Available at: [https://doi.org/10.1103/PhysRevLett.103.150502](https://doi.org/10.1103/PhysRevLett.103.150502).
8.  Berry, D.W., Childs, A.M., Ostrander, A. and Wang, G. (2017) 'Quantum algorithm for linear differential equations with exponentially improved dependence on precision', *Communications in Mathematical Physics*, 356, pp. 1057–1081. Available at: [https://doi.org/10.1007/s00220-017-3002-y](https://doi.org/10.1007/s00220-017-3002-y).
9.  Wu, H.-C., Wang, J. and Li, X. (2024) *Quantum algorithms for non-linear dynamics: Revisiting Carleman linearization with no dissipative conditions*. \[Preprint]. arXiv:2405.12714. Available at: [https://arxiv.org/abs/2405.12714](https://arxiv.org/abs/2405.12714).
10. Low, G.H. and Chuang, I.L. (2017) 'Optimal Hamiltonian simulation by quantum signal processing', *Physical Review Letters*, 118(1), 010501. Available at: [https://doi.org/10.1103/PhysRevLett.118.010501](https://doi.org/10.1103/PhysRevLett.118.010501).
11. Childs, A.M., Kothari, R. and Somma, R.D. (2017) 'Quantum algorithm for linear systems of equations with exponentially improved dependence on precision', *SIAM Journal on Computing*, 46(6), pp. 1920–1950. Available at: [https://doi.org/10.1137/16M1087072](https://doi.org/10.1137/16M1087072).
12. Brassard, G., Høyer, P., Mosca, M. and Tapp, A. (2002) 'Quantum amplitude amplification and estimation', *Quantum Information & Computation*, 2(1), pp. 53–74. Available at: [https://arxiv.org/abs/quant-ph/0005055](https://arxiv.org/abs/quant-ph/0005055).
13. Suzuki, S., et al. (2021) 'Real- and imaginary-time evolution with compressed quantum circuits', *PRX Quantum*, 2(1), 010342. Available at: [https://doi.org/10.1103/PRXQuantum.2.010342](https://www.google.com/search?q=https://doi.org/10.1103/PRXQuantum.2.010342).
14. McArdle, S., et al. (2020) 'Variational quantum simulation of general processes', *Physical Review Letters*, 125(1), 010501. Available at: [https://doi.org/10.1103/PhysRevLett.125.010501](https://doi.org/10.1103/PhysRevLett.125.010501).
15. Kuroiwa, K., Ohkuma, T., Sato, H. and Imai, R. (2022) *Quantum Car-Parrinello molecular dynamics: A cost-efficient molecular simulation method on near-term quantum computers*. \[Preprint]. arXiv:2212.11921. Available at: [https://arxiv.org/abs/2212.11921](https://arxiv.org/abs/2212.11921).
16. Arrighi, P., Forets, M. and Nesme, V. (2014) 'The Dirac equation as a quantum walk: higher dimensions, observational convergence', *Journal of Physics A: Mathematical and Theoretical*, 47(46), 465302. Available at: [https://doi.org/10.1088/1751-8113/47/46/465302](https://doi.org/10.1088/1751-8113/47/46/465302).
17. Huerta Alderete, C., et al. (2020) 'Quantum walks and Dirac cellular automata on a programmable trapped-ion quantum computer', *Nature Communications*, 11, 3720. Available at: [https://doi.org/10.1038/s41467-020-17519-4](https://doi.org/10.1038/s41467-020-17519-4).
18. Ding, Z., Li, X. and Lin, L. (2024) 'Simulating open quantum systems using Hamiltonian simulations', *PRX Quantum*, 5(2), 020332. Available at: [https://doi.org/10.1103/PRXQuantum.5.020332](https://doi.org/10.1103/PRXQuantum.5.020332).
19. Cleve, R. and Wang, C. (2016) *Efficient quantum algorithms for simulating Lindblad evolution*. \[Preprint]. arXiv:1612.09512. Available at: [https://arxiv.org/abs/1612.09512](https://arxiv.org/abs/1612.09512).
20. Yung, M.-H., Whitfield, J.D., Tempel, D.G., Aspuru-Guzik, A. and Lloyd, S. (2014) 'Computational complexity of time-dependent density functional theory', *New Journal of Physics*, 16(8), 083035. Available at: [https://doi.org/10.1088/1367-2630/16/8/083035](https://doi.org/10.1088/1367-2630/16/8/083035).
21. Cavaglia, A., et al. (2020) 'An algorithm for quantum computation of particle decays and cross-sections', *Physical Review D*, 102(9), 094505. Available at: [https://doi.org/10.1103/PhysRevD.102.094505](https://doi.org/10.1103/PhysRevD.102.094505).
22. Rong, X., et al. (2024) *Quantum multigrid algorithm for finite-element problems*. \[Preprint]. arXiv:2404.07466. Available at: [https://arxiv.org/abs/2404.07466](https://arxiv.org/abs/2404.07466).
23. Alkadri, A.M., Kharazi, T.D. and Whaley, K.B. (2025) *A quantum algorithm for the finite-element method*. \[Preprint]. arXiv:2510.18150. Available at: [https://arxiv.org/abs/2510.18150](https://arxiv.org/abs/2510.18150).
24. Shaviner, G. (2025) *Quantum singular-value transformation for solving linear systems of differential equations*. \[Preprint]. arXiv:2507.09686. Available at: [https://arxiv.org/abs/2507.09686](https://arxiv.org/abs/2507.09686).
25. Grinko, D., Gacon, J., Zoufal, C. and Woerner, S. (2021) 'Iterative quantum amplitude estimation', *npj Quantum Information*, 7, 52. Available at: [https://doi.org/10.1038/s41534-021-00379-1](https://doi.org/10.1038/s41534-021-00379-1).
26. Zhou, Y., et al. (2025) *Adaptive mesh-refinement quantum algorithm for Maxwell’s equations*. \[Preprint]. arXiv:2504.01646. Available at: [https://arxiv.org/abs/2504.01646](https://arxiv.org/abs/2504.01646).
27. Yu, Y. (2025) *Schrödingerization based quantum algorithms for the fractional Poisson equation*. \[Preprint]. arXiv:2505.01602. Available at: [https://arxiv.org/abs/2505.01602](https://arxiv.org/abs/2505.01602).
28. Ayral, T. (2025) *Dynamical mean-field theory with quantum computing*. \[Preprint]. arXiv:2508.00118. Available at: [https://arxiv.org/abs/2508.00118](https://arxiv.org/abs/2508.00118).
29. Georgescu, C.A. (2024) *Quantum Algorithms for the Lattice Boltzmann Method: Encoding and Evolution*. PhD thesis, TU Delft. Available at: [https://resolver.tudelft.nl/a7b20729-46b7-42d1-aaf7-c001fc93efd9](https://resolver.tudelft.nl/a7b20729-46b7-42d1-aaf7-c001fc93efd9).
    3In. Klco, N., Savage, M.J. and Stryker, J.R. (2020) 'SU(2) non-Abelian gauge field theory in one dimension on digital quantum computers', *Physical Review D*, 101(7), 074512. Available at: [https://doi.org/10.1103/PhysRevD.101.074512](https://doi.org/10.1103/PhysRevD.101.074512).
30. García-Molina, P., Rodríguez-Mediavilla, J. and García-Ripoll, J.J. (2022) 'Quantum Fourier analysis for multivariate functions and applications to a class of Schrödinger-type partial differential equations', *Physical Review A*, 105(1), 012433. Available at: [https://doi.org/10.1103/PhysRevA.105.012433](https://doi.org/10.1103/PhysRevA.105.012433).
31. Kyriienko, O. (2023) *Quantum Chebyshev transform: mapping, embedding, learning and sampling distributions*. \[Preprint]. arXiv:2306.17026. Available at: [https://arxiv.org/abs/2306.17026](https://arxiv.org/abs/2306.17026).
32. Zhao, T., et al. (2022) *Quantum-inspired variational algorithms for partial differential equations: application to financial derivative pricing*. \[Preprint]. arXiv:2207.10838. Available at: [https://arxiv.org/abs/2207.10838](https://arxiv.org/abs/2207.10838).
33. Zhou, Y., et al. (2024) *Quantum algorithm for partial differential equations of non-conservative systems with spatially varying parameters*. \[Preprint]. arXiv:2407.05019. Available at: [https://arxiv.org/abs/2407.05019](https://arxiv.org/abs/2407.05019).
34. Foulkes, W.M.C., Mitas, L., Needs, R.J. and Rajagopal, G. (2001) 'Quantum Monte Carlo simulations of solids', *Reviews of Modern Physics*, 73(1), pp. 33–83. Available at: [https://doi.org/10.1103/RevModPhys.73.33](https://doi.org/10.1103/RevModPhys.73.33).
35. Stenger, M., et al. (2022) *Quantum algorithms for uncertainty quantification: application to partial differential equations*. \[Preprint]. arXiv:2209.11220. Available at: [https://arxiv.org/abs/2209.11220](https://arxiv.org/abs/2209.11220).
36. Bianucci, P., Miquel, C., Paz, J.P. and Saraceno, M. (2001) *Discrete Wigner functions and the phase-space representation of quantum computers*. \[Preprint]. arXiv:quant-ph/0106091. Available at: [https://arxiv.org/abs/quant-ph/0106091](https://arxiv.org/abs/quant-ph/0106091).
37. Kitaev, A.Y. (1995) *Quantum measurements and the Abelian stabilizer problem*. \[Preprint]. arXiv:quant-ph/9511026. Available at: [https://arxiv.org/abs/quant-ph/9511026](https://arxiv.org/abs/quant-ph/9511026).
38. Arute, F., et al. (2019) 'Quantum supremacy using a programmable superconducting processor', *Nature*, 574, pp. 505–510. Available at: [https://doi.org/10.1038/s41586-019-1666-5](https://doi.org/10.1038/s41586-019-1666-5).
39. Lloyd, S. (1996) 'Universal quantum simulators', *Science*, 273(5278), pp. 1073–1078. Available at: [https://doi.org/10.1126/science.273.5278.1073](https://doi.org/10.1126/science.273.5278.1073).
40. Bausch, J., et al. (2023) *Quantum algorithms for fractional-order operators*. \[Preprint]. arXiv:2303.06703. Available at: [https://arxiv.org/abs/2303.06703](https://arxiv.org/abs/2303.06703).
41. Brown, K.R., et al. (2021) 'Quantum algorithms for PDEs: a review', *Frontiers in Physics*, 9, 667392. Available at: [https://doi.org/10.3389/fphy.2021.667392](https://doi.org/10.3389/fphy.2021.667392).
42. Barends, R., et al. (2014) 'Superconducting quantum circuits at the surface code threshold for fault tolerance', *Nature*, 508, pp. 500–503. Available at: [https://doi.org/10.1038/nature13171](https://doi.org/10.1038/nature13171).
43. Rotentg, M., et al. (2022) 'Quantum-accelerated stochastic differential equations', *Quantum*, 6, 805. Available at: [https://doi.org/10.22331/q-2022-06-09-805](https://doi.org/10.22331/q-2022-06-09-805).
44. Gottesman, D. (1998) *The Heisenberg representation of quantum computers*. \[Preprint]. arXiv:quant-ph/9807006. Available at: [https://arxiv.org/abs/quant-ph/9807006](https://arxiv.org/abs/quant-ph/9807006).
45. Babbush, R., et al. (2018) 'Low-depth quantum simulation of materials', *Physical Review X*, 8(1), 011044. Available at: [https://doi.org/10.1103/PhysRevX.8.011044](https://doi.org/10.1103/PhysRevX.8.011044).
46. Syamlal, M., Copen, C., Takahashi, M. and Hall, B. (2024) *Computational Fluid Dynamics on Quantum Computers*. \[Preprint]. arXiv:2406.18749. Available at: [https://arxiv.org/abs/2406.18749](https://arxiv.org/abs/2406.18749).
47. Penuel, J., et al. (2024) *Feasibility of accelerating incompressible computational fluid dynamics simulations with fault-tolerant quantum computers*. \[Preprint]. arXiv:2406.06323. Available at: [https://arxiv.org/abs/2406.06323](https://arxiv.org/abs/2406.06323).
48. Li, J., et al. (2025) *Multi-set variational quantum dynamics algorithm for simulating nonadiabatic dynamics on quantum computers*. \[Preprint]. arXiv:2503.07388. Available at: [https://arxiv.org/abs/2503.07388](https://arxiv.org/abs/2503.07388).
49. Duriez, A., et al. (2025) *Computing band gaps of periodic materials via sample-based quantum diagonalization*. \[Preprint]. arXiv:2503.10901. Available at: [https://arxiv.org/abs/2503.10901](https://arxiv.org/abs/2503.10901).
50. Arora, A., Ward, B.M. and Oskay, C. (2024) *An implementation of the finite element method in hybrid classical/quantum computers*. \[Preprint]. arXiv:2411.09038. Available at: [https://arxiv.org/abs/2411.09038](https://arxiv.org/abs/2411.09038).
51. Ghisoni, F., Scala, F., Bajoni, D. and Gerace, D. (2024) *Shadow quantum linear solver: A resource-efficient quantum algorithm for linear systems of equations*. \[Preprint]. arXiv:2409.08929. Available at: [https://arxiv.org/abs/2409.08929](https://arxiv.org/abs/2409.08929).
52. Kahanamoku-Meyer, G.D. (2023) *Exploring the Limits of Classical Simulation: From Computational Many-Body Dynamics to Quantum Advantage*. PhD thesis, University of California, Berkeley. Available at: [https://gregkm.me/files/Kahanamoku-Meyer\_dissertation.pdf](https://gregkm.me/files/Kahanamoku-Meyer_dissertation.pdf). 
