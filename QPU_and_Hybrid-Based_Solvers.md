# QPU and Hybrid Solver Techniques for Quantum, Atomistic, Molecular, and Nanoscale Simulation

This taxonomy maps mathematical solver families to applications and execution roles across quantum and classical hardware.  

## Primer and execution legend

Begin solver selection by defining the physical model and the observables needed from the calculation. This taxonomy covers two approaches:

- **Quantum many-body simulation** represents physical quantum systems and estimates their states, dynamics, and observables.
- **Quantum numerical simulation** encodes mathematical problems, such as classical partial differential equations (PDEs), into quantum states for numerical processing.

The physical representation, data access, and requested outputs determine the computational cost of each approach. A nanoscale device workflow often combines quantum electronic structure, classical nuclear dynamics, and continuum electromagnetic fields through a hybrid model.

| Label | Execution role | Interpretation |
| --- | --- | --- |
| Q | Gate-based QPU | Quantum state preparation, evolution, interference, sampling, or operator estimation performs the central quantum subroutine. Control electronics supply pulses, readout, and sequencing. |
| H | Essential quantum–classical hybrid | Classical optimization, diagonalization, self-consistency, trajectories, or reconstruction is part of the mathematical algorithm. |
| AQ | Programmable analog quantum simulation | Native quantum interactions implement a specified Hamiltonian or channel, with finite control range and calibration error. |
| C | Classical CPU, GPU, or accelerator | Performs numerical computation on classical representations, even when the modeled physics is quantum. |
| CA | Classical analog computation | A classical electronic, photonic, or other analog device performs a numerical operation, subject to calibration, precision, and conversion costs. |
| R | Target-specific research extension | The stated application requires a target-specific construction, cost analysis, or hardware validation. The label identifies the development needed to apply the underlying primitive. |
| QA | Quantum annealing | An accessible annealing Hamiltonian encodes an optimization objective; classical embedding, discretization, and result processing remain part of the workflow. QA denotes optimization; AQ denotes simulation of a target physical system. |

Discrete-variable and continuous-variable encodings describe the information representation. Digital and analog simulation describe how evolution is implemented. These axes combine independently: an analog spin simulator uses discrete spin degrees of freedom, and a continuous-variable system also admits gate-based operations.

## Overview tree

The five application branches belong to one taxonomy. Detailed leaves and execution roles follow below.

```text
QPU and hybrid simulation taxonomy
├── Electronic and material eigenstates
│   ├── Variational preparation
│   │   └── VQE, ADAPT-VQE, VQD
│   │       └── Energies and reduced density matrices
│   ├── Subspace methods
│   │   └── QSE, Krylov, Lanczos, SQD, QSCI, SKQD
│   │       └── Selected eigenstates and excitation energies
│   └── Spectral preparation
│       └── QPE, QITE, filtering, adiabatic paths
│           └── Energy-resolved states and eigenvalues
├── Dynamics and spectroscopy
│   ├── Hamiltonian evolution
│   │   └── Trotter, qDRIFT, LCU, QSP, qubitization, VQS
│   │       └── Wavepackets, reactions and spin dynamics
│   ├── Interferometric observables
│   │   └── Hadamard tests, overlap estimation, correlators, resolvents
│   │       └── Interference, INS spectra and electronic response
│   └── Open and native quantum systems
│       └── Channels, trajectories, spin and bosonic simulators
│           └── Decoherence, thermal response and vibronic dynamics
├── Field equations and kinetic transport
│   ├── Spatial discretizations
│   │   └── FEM, FVM and stationary finite differences with QLSA or VQLS
│   │       └── Electrostatics, fields and selected solution functionals
│   ├── Lattice kinetic models
│   │   └── QLBM, QLGA, quantum walks and QCA
│   │       └── Fluid and transport observables within model validity
│   └── Time and nonlinear formulations
│       └── Quantum time-domain finite differences, ODE embeddings, Schrodingerisation and LCHS
│           └── Dissipative evolution and bounded nonlinear models
├── Multiscale and ensemble workflows
│   ├── Embedded quantum subproblems
│   │   └── DMFT, DMET, active spaces, EWF and QM/MM
│   │       └── Correlated materials and molecular fragments
│   ├── Nuclear and coarse dynamics
│   │   └── QPU-assisted forces, QCPMD and fine-scale corrections
│   │       └── Molecular motion and coupled nanoscale models
│   └── Thermal and stochastic methods
│       └── TFD, VQT, QMETTS, Gibbs sampling, QAE and AFQMC interfaces
│           └── Temperature-dependent observables and expectations
└── Computational characterization, reconstruction, and inverse design
    ├── Inverse reconstruction [Q/H/QA; R for target-specific extensions]
    │   ├── Electron tomography, 4D-STEM ptychography, and field-map inversion
    │   └── Linearized subproblems, constrained optimization, and selected parameter inference
    ├── Forward signal prediction [Q/H/AQ]
    │   ├── ARPES, EELS, NMR, interferometric amplitudes, and microscopic response
    │   └── Quantum correlations combined with classical sample and instrument models
    └── Experimental and process design [H; R outside specific constructions]
        ├── EUV resist spectroscopy, mask design, probes, and acquisition settings
        └── Selected objectives with explicit output, uncertainty, and resource accounting
```

## Temporal formulation and explicit quantum FDTD placement

Temporal formulation is an additional classification axis, independent of the spatial discretization and execution hardware. For example, a finite-element model admits both transient and frequency-domain formulations. FDTD specifies a combination of finite-difference spatial and temporal discretization. Hamiltonian simulation of a spatial finite-difference operator uses a quantum evolution scheme related to FDTD. The temporal update determines whether the construction implements conventional finite-difference time marching or another propagator.

| Temporal formulation | Mathematical task | Representative quantum routes | Interpretation |
| --- | --- | --- | --- |
| Stationary or steady-state | Solve a time-independent field equation, eigenproblem, or stationary channel | QLSA, VQLS, eigensolvers, dissipative preparation | Choose a field equation, eigenproblem, or stationary-channel formulation for the requested observable. |
| Frequency-domain or time-harmonic | Solve a driven system at a specified frequency | Shifted linear systems, resolvents, QLSA, VQLS | Finite-difference frequency-domain and FEM frequency-domain models have frequency-dependent operators and boundary data. |
| Real-time transient | Propagate an initial state or field, with optional time-dependent sources | Hamiltonian simulation, VQS, Schrödingerisation, LCHS, ODE algorithms | Covers pulses, wavepackets, relaxation, and transient transport; a valid FDTD mapping belongs here. |
| Periodically driven or Floquet | Analyze a periodic generator or one-period propagator | Time-dependent simulation, spectral estimation of the period propagator | Floquet analysis uses the periodic generator or one-period propagator; single-frequency linear response applies under the corresponding response assumptions. |
| Imaginary-time or thermal preparation | Apply normalized exponential filters or prepare ensembles | QITE, variational imaginary time, filtering, purification | Imaginary time parametrizes exponential filtering for state preparation or inference; real time parametrizes physical evolution. |

```text
PDE solver temporal formulations
├── Stationary and frequency-domain
│   └── FDFD/ FEM operators, resolvents, QLSA and VQLS
├── Real-time transient
│   └── Quantum time-domain finite-difference formulations
│       ├── Maxwell/ Yee-type mappings
│       ├── Schrodinger and other wave equations
│       └── Coupled light-matter models with explicit QPU subproblems
└── Imaginary-time preparation
    └── QITE, spectral filters and thermal-state preparation
```

The transient branch cross-references detailed branch V for numerical PDEs and branch II for physical Hamiltonian evolution. Electronic wavefunction evolution predicts particle observables, while amplitude-encoded electromagnetic evolution predicts classical field quantities; each uses the output interpretation of its physical model.

| Example | What is implemented or proposed | Evidence and scope |
| --- | --- | --- |
| Jin, Liu, and Ma, Maxwell Schrödingerisation [19] | Yee-based Maxwell discretization and alternative spectral/upwind formulations mapped to unitary evolution; PEC, impedance, and interface treatments | Numerical algorithm study covering specified Maxwell formulations, with a continuous-variable quantum formulation also discussed. General hardware deployment requires validation for the intended geometry, boundaries, and outputs. |
| Ma et al., Maxwell circuits with time-dependent sources [20] | Explicit circuits for PEC boundaries and driven Maxwell evolution using Schrödingerisation and autonomization | Circuit construction and complexity analysis; scaling follows the stated operator-access and evolution assumptions. |
| Sharma et al., time-domain Maxwell hardware study [21] | Finite-difference operators, Schrödingerisation, Bell-basis Trotter blocks, and signed-field recovery | The 2026 preprint reports two-dimensional IonQ QPU benchmarks and three-dimensional simulator results; the hardware and simulator results provide evidence for their respective implementations. |
| Na and Chew, quantum electromagnetic FDTD [22] | Classical numerical propagation of quantized electromagnetic-field propagators, including a Hong–Ou–Mandel example | Classical numerical execution of a quantized electromagnetic-field model, categorized as C. |
| Finite-difference electronic wavepacket evolution | Discretize the Schrödinger Hamiltonian, prepare a wavepacket, evolve it, and estimate selected observables | Quantum propagation uses a discretized spatial Hamiltonian; a finite-difference temporal update specifies a literal quantum FDTD construction. |
| Hybrid Maxwell–Schrödinger or Maxwell–Bloch dynamics | A classical field solver exchanges specified source or material-response information with a quantum subproblem | Research extension requiring specified coupling, measurement, stability, and implementation. A classical field calculation is C; an explicit quantum subproblem defines the hybrid workflow. |

A quantum FDTD implementation requires coherent operator access, prepared initial fields and sources, absorbing-boundary treatment, and preservation of field constraints. Its error budget includes spatial and temporal discretization, propagation approximations, boundary treatment, and signed-field recovery. Conventional explicit FDTD obeys the Courant–Friedrichs–Lewy (CFL) stability criterion, while each quantum evolution construction requires a stability analysis derived from its own update rule. Selected field measurements and full electromagnetic mesh reconstruction impose different readout costs, which belong in the total resource estimate. At the nanoscale, the continuum Maxwell model also requires material-response functions that represent the relevant physical effects.

## Detailed technique tree

```text
QPU and hybrid solver techniques
├── I. Electronic ground states, excited states, and material spectra
│   ├── Variational eigenstate solvers [H]
│   │   ├── VQE
│   │   │   └── the QPU estimates a parameterized state's energy; a CPU or GPU minimizes that estimate.
│   │   ├── ADAPT-VQE
│   │   │   └── measured operator gradients guide classical selection of ansatz operators.
│   │   └── VQD and state-specific variational methods
│   │       └── overlap penalties or state constraints target excited states, with sampling and optimization
│   │           overhead.
│   ├── Quantum subspace solvers [Q/H]
│   │   ├── QSE, quantum Krylov, and quantum Lanczos
│   │   │   └── quantum preparation and matrix-element estimation build a projected problem; classical
│   │   │       generalized diagonalization returns approximate eigenvalues.
│   │   ├── QSCI and SQD
│   │   │   └── quantum configuration samples select classical configuration-interaction subspaces; the
│   │   │       eigenproblem is solved classically.
│   │   ├── SKQD and randomized propagation variants
│   │   │   └── evolved-state samples enrich the selected classical subspace; convergence depends on the
│   │   │       state distribution and subspace size.
│   │   └── Extended SQD
│   │       └── extensions enlarge or refine the sample-selected electronic problem, including embedded
│   │           fragments in the FLiBe study [1, 2].
│   ├── Spectral and state-preparation routes [Q/H/AQ]
│   │   ├── QPE and iterative spectral estimation
│   │   │   └── coherent evolution resolves eigenenergies; initial-state overlap determines which
│   │   │       eigenstates are sampled.
│   │   ├── QSP/QSVT spectral filters
│   │   │   └── block-encoded functions suppress unwanted spectral components, with normalization and
│   │   │       state-preparation costs.
│   │   ├── QITE and variational imaginary time
│   │   │   └── quantum measurements determine state-dependent updates, usually accompanied by classical
│   │   │       coefficient solves.
│   │   └── Adiabatic preparation and engineered cooling
│   │       └── a controlled Hamiltonian path or dissipative process prepares an accessible low-energy
│   │           state; gaps and mixing govern cost.
│   └── Periodic-material extensions [H; model-specific]
│       ├── Momentum-resolved models and periodic active spaces
│       │   └── solve selected sectors or embedded periodic problems.
│       ├── Charged excitation energies and Green-function spectra
│       │   └── extract addition/removal energies or spectral poles for band gaps and quasiparticle
│       │       dispersion.
│       └── Scope
│           └── band dispersion requires momentum-resolved electronic information. Kohn–Sham eigenvalues
│               describe an auxiliary one-particle model; addition/removal spectra encode many-body charged excitations.
├── II. Real-time Hamiltonian evolution and computational scattering
│   ├── Digital Hamiltonian simulation [Q]
│   │   ├── Lie–Trotter, Strang, Suzuki, and qDRIFT formulas
│   │   │   └── approximate evolution using simpler accessible terms.
│   │   ├── LCU, truncated Taylor, and Dyson methods
│   │   │   └── combine controlled operations to simulate static or time-dependent dynamics.
│   │   ├── Qubitization and QSP
│   │   │   └── implement block-encoded Hamiltonian evolution; QSVT extends the matrix-function framework
│   │   │       [8].
│   │   ├── Interaction-picture algorithms
│   │   │   └── treat an efficiently simulated dominant term separately from the remaining interactions.
│   │   └── Sparse-Hamiltonian and quantum-walk constructions
│   │       └── use explicit coherent access to nonzero entries or a suitable block encoding.
│   ├── Variational quantum dynamics [H]
│   │   ├── Real-time VQS and adaptive variants
│   │   │   └── the QPU estimates projected equations of motion; classical linear algebra updates circuit
│   │   │       parameters.
│   │   └── Nonadiabatic multi-state dynamics
│   │       └── couple electronic-state amplitudes and nuclear degrees of freedom using a specified ansatz
│   │           and propagation rule.
│   ├── Spatial and occupation encodings [Q/AQ]
│   │   ├── First quantization
│   │   │   └── spatial grids, plane waves, or orbital bases represent particle coordinates.
│   │   ├── Second quantization
│   │   │   └── orbital occupation registers use Jordan–Wigner, Bravyi–Kitaev, or another justified
│   │   │       fermionic encoding.
│   │   ├── Split-operator evolution
│   │   │   └── quantum Fourier transformations exchange position and momentum representations when the
│   │   │       discretization permits efficient kinetic and potential steps.
│   │   └── Qudit and bosonic encodings
│   │       └── truncated occupations represent phonons, photons, magnons, or vibrational modes.
│   └── Scattering outputs [Q/H]
│       ├── Channel-resolved wavepacket propagation
│       │   └── evolving prepared incoming states and measuring outgoing channels yields probabilities or
│       │       transition amplitudes.
│       ├── Interferometric amplitude estimation
│       │   └── coherent reference branches resolve complex overlaps and relative phases [9].
│       └── Phase shifts, cross sections, and rates
│           └── classical analysis supplies channel normalization, flux factors, and spectral
│               reconstruction.
├── III. Computational interferometry, response functions, and spectroscopy
│   ├── Complex overlaps and phase-sensitive observables [Q/H]
│   │   ├── Hadamard tests
│   │   │   └── ancilla measurements estimate the real or imaginary component of a controlled-unitary
│   │   │       expectation.
│   │   ├── Reference-state interference and related overlap protocols
│   │   │   └── reconstruct selected transition amplitudes when the required state preparation is available.
│   │   ├── SWAP and fidelity tests
│   │   │   └── estimate squared overlap magnitudes; phase-sensitive overlaps require a reference-interference
│   │   │       protocol.
│   │   └── Loschmidt amplitudes and echo probabilities
│   │       └── estimate complex return amplitudes through phase-sensitive protocols and echo probabilities
│   │           through squared return magnitudes.
│   ├── Correlation functions and spectra [Q/H/AQ]
│   │   ├── Two-time spin, density, and current correlators
│   │   │   └── quantum evolution and measurement supply time-dependent response data.
│   │   ├── Dynamical structure factors
│   │   │   └── classical spatial and temporal transforms of measured spin correlations produce momentum-
│   │   │       and frequency-resolved spectra.
│   │   ├── Kubo response
│   │   │   └── equilibrium correlators determine specified linear-response coefficients with thermal-state
│   │   │       preparation and integration errors.
│   │   └── Green functions and shifted resolvents
│   │       └── evaluate selected matrix elements, including scale recovery for normalized quantum solution
│   │           states.
│   ├── Inelastic neutron scattering prediction [Q/H/AQ]
│   │   ├── Spin Hamiltonian preparation and evolution
│   │   │   └── represent the relevant magnet or molecular nanomagnet.
│   │   ├── Spectral reconstruction
│   │   │   └── obtain the magnetic dynamical structure factor and apply polarization, magnetic form
│   │   │       factors, geometry, and instrument resolution.
│   │   └── Interpretation
│   │       └── the QPU simulates the material response; scattering and instrument models convert that response
│   │           into predicted neutron intensities. Hardware examples include molecular spin models and
│   │           the KCuF3 comparison [3, 4].
│   └── Molecular and solid-state spectroscopies [Q/H/AQ]
│       ├── Electronic transitions
│       │   └── excitation energies plus dipole or other transition matrix elements yield selected spectral
│       │       intensities.
│       ├── Vibrational and vibronic spectra
│       │   └── bosonic encodings or native oscillator systems represent nuclear modes and
│       │       electron–vibration coupling.
│       └── OTOCs and contour-ordered observables
│           └── specified operator orderings and circuits evaluate out-of-time-order or contour-ordered
│               correlations; the selected ordering determines the physical response.
├── IV. Open systems, quantum baths, and transport
│   ├── Channel and Lindblad evolution [Q/H/AQ]
│   │   ├── Stinespring dilation and Kraus circuits
│   │   │   └── auxiliary registers represent a specified completely positive channel.
│   │   ├── Collision models, resets, and engineered dissipation
│   │   │   └── repeated system–environment interactions implement a target open-system process.
│   │   └── Lindblad product formulas, LCU, or block encodings
│   │       └── simulate Markovian generators under their construction-specific assumptions.
│   ├── Trajectory and memory methods [Q/H/AQ]
│   │   ├── Quantum jumps and diffusive trajectories
│   │   │   └── quantum evolution generates stochastic records; classical averaging estimates ensemble
│   │   │       observables.
│   │   ├── Explicit baths, pseudomodes, and reaction coordinates
│   │   │   └── additional quantum degrees of freedom represent selected non-Markovian memory.
│   │   └── Process-tensor workflows
│   │       └── specified memory representations require storage and circuit-cost accounting.
│   └── Stationary states and electronic transport [Q/H; R for targets requiring construction]
│       ├── Dissipative or variational steady-state preparation
│       │   └── assess uniqueness, relaxation, and physical-state validity.
│       ├── Trace-constrained Liouvillian solves
│       │   └── handle singularity and non-Hermitian structure explicitly. Physical density-matrix recovery
│       │       requires trace normalization, Hermiticity, positivity, and a compatible preparation step.
│       ├── QPU-assisted Green functions or impurity self-energies
│       │   └── feed measured quantum quantities into classical NEGF or transport calculations.
│       └── Scope
│           └── classical NEGF, Landauer transport, and GPU Lindblad solves perform numerical transport tasks [C].
├── V. Quantum numerical PDE solvers, finite elements, and finite volumes
│   ├── Linear-system primitives [Q/H]
│   │   ├── HHL and subsequent QLSAs
│   │   │   └── prepare a state proportional to an inverse action under spectral and access assumptions [7].
│   │   ├── QSVT inverse or pseudoinverse filters
│   │   │   └── transform a block-encoded matrix, with conditioning and support requirements [8].
│   │   ├── VQLS
│   │   │   └── quantum residual or cost estimates drive classical optimization of a normalized solution
│   │   │       state [6].
│   │   └── Quantum projected/Krylov approaches
│   │       └── estimate subspace operators, then solve the reduced algebraic problem classically.
│   ├── Discretization-specific constructions [Q/H]
│   │   ├── Quantum FEM
│   │   │   └── map stiffness or generalized eigenvalue matrices, load vectors, and boundary conditions to
│   │   │       the chosen primitive.
│   │   ├── Quantum FVM
│   │   │   └── map conservative cell balances and flux equations to a specified quantum linear or nonlinear
│   │   │       workflow [11].
│   │   ├── Finite-difference and spectral methods
│   │   │   └── construct coherent stencil or operator access, including the interface to stored matrix data
│   │   │       and its implementation cost.
│   │   └── Preconditioning
│   │       └── encode BPX, domain-decomposition, or another justified preconditioner and include its
│   │           construction and application costs [5].
│   ├── Time-dependent and dissipative numerical evolution [Q/H/AQ]
│   │   ├── Quantum finite-difference time-domain and related evolution formulations
│   │   │   └── explicit Maxwell/Yee-type mappings, driven-source circuits, and spatial finite-difference
│   │   │       Hamiltonian evolution; the temporal update specifies finite-difference time marching or a
│   │   │       quantum propagator [19–21].
│   │   ├── FDTD-related applications
│   │   │   └── electromagnetic pulse propagation, specified dielectric-interface scattering, quantum
│   │   │       wavepackets, and proposed coupled light-matter workflows. Physical sources, losses, and
│   │   │       absorbing boundaries require construction-specific handling.
│   │   ├── Linear ODE embeddings
│   │   │   └── reformulate time discretization as a linear system or use a dedicated evolution algorithm.
│   │   ├── Schrödingerisation
│   │   │   └── enlarge the representation so suitable linear dynamics is recovered from unitary evolution
│   │   │       [12].
│   │   ├── LCHS
│   │   │   └── represent suitable nonunitary evolution through weighted Hamiltonian simulations.
│   │   └── Hybrid oscillator–qubit LCHS
│   │       └── an ancillary quantum oscillator represents the kernel within a hybrid oscillator–qubit
│   │           quantum architecture [13].
│   └── Application scope [model-specific]
│       ├── Poisson, Helmholtz, Maxwell, elasticity, diffusion, and reaction–transport problems
│       │   └── boundary treatment, stability, physical constraints, and requested outputs determine
│       │       eligibility.
│       ├── Nano applicability
│       │   └── continuum constitutive assumptions determine the physical content of the calculation; include
│       │       relevant nanoscale effects in the model supplied to the solver.
│       └── Output scope
│           └── selected functionals require targeted estimation; full mesh recovery requires reconstruction
│               of every requested value.
├── VI. Quantum lattice kinetics, fluid dynamics, and nonlinear equations
│   ├── QLBM [Q/H; AQ for an explicit native mapping]
│   │   ├── Reversible streaming
│   │   │   └── encoded populations are moved between lattice sites.
│   │   ├── Collision implementations
│   │   │   └── dilation, measurement, feedback, or a specified ensemble construction represents relaxation
│   │   │       or collision.
│   │   ├── Carleman formulations
│   │   │   └── lifted polynomial dynamics requires a finite truncation and convergence bounds.
│   │   └── Node-level ensemble formulations
│   │       └── enlarge the local description to implement a specified nonlinear fluid model [10].
│   ├── QLGA, quantum walks, and QCA [Q/AQ]
│   │   ├── Quantum lattice gas automata
│   │   │   └── local occupation registers undergo collisions and streaming.
│   │   ├── Coined, split-step, and continuous-time walks
│   │   │   └── simulate controlled transport or wave propagation with model-specific continuum limits.
│   │   └── Quantum cellular automata
│   │       └── local updates represent specified reversible lattice dynamics.
│   ├── Extensions to kinetic or nonlinear models [Q/H; R outside established constructions]
│   │   ├── Carleman, Koopman, and Liouville representations
│   │   │   └── linearize a lifted description while retaining truncation and conditioning costs.
│   │   ├── Wigner or Boltzmann kinetics
│   │   │   └── specify phase-space encoding, signed quasiprobabilities, scattering kernels, and
│   │   │       observables.
│   │   ├── Nonlinear Schrödinger and Gross–Pitaevskii equations
│   │   │   └── require feedback, a justified lifting, or an explicit many-body mean-field limit.
│   │   └── Stochastic and uncertainty workflows
│   │       └── quantum expectation estimation requires a coherently implementable model or sampling oracle.
│   └── Physical scale and validation
│       ├── Fluid QLBM
│       │   └── describes mesoscopic kinetic or hydrodynamic populations under the selected fluid closure.
│       ├── Confined nanofluids
│       │   └── boundary scattering, Knudsen effects, molecular structure, and quantum statistics determine
│       │       whether the selected fluid closure remains valid.
│       └── Interpretation
│           └── classical circuit simulation provides numerical algorithm validation; physical-QPU execution
│               supplies hardware evidence.
├── VII. Programmable analog and mixed digital–analog quantum simulation
│   ├── Spin and fermionic lattice models [AQ/Q]
│   │   ├── Native Ising, XY, and accessible Heisenberg interactions
│   │   │   └── simulate selected quantum magnetic Hamiltonians.
│   │   ├── Optical-lattice Bose–Hubbard and Fermi–Hubbard models
│   │   │   └── represent interacting lattice particles subject to experimental controls.
│   │   ├── Rydberg, ion, and superconducting analog platforms
│   │   │   └── match available interactions, geometry, and observables to the target model.
│   │   └── Digital–analog sequences
│   │       └── interleave native interaction blocks with gates, including approximation and calibration
│   │           errors.
│   ├── Bosonic and oscillator models [AQ/Q]
│   │   ├── Gaussian mode transformations
│   │   │   └── simulate quadratic oscillator dynamics.
│   │   ├── Anharmonic or interacting mode dynamics
│   │   │   └── require suitable non-Gaussian operations or encoded digital interactions.
│   │   └── Spin–boson and vibronic models
│   │       └── couple quantum spins or electronic states to phonon, photon, or molecular vibrational modes.
│   └── Annealing and gauge-model extensions [AQ/Q; model-specific]
│       ├── Annealing
│       │   └── searches low-energy states of accessible Hamiltonians; chemistry requires an explicit useful
│       │       mapping.
│       ├── Truncated gauge fields and quantum links
│       │   └── require constraint preservation and truncation-error assessment.
│       └── Scope
│           └── native quantum simulators use controlled quantum interactions to implement the target
│               Hamiltonian or channel.
├── VIII. Embedded, atomistic, and multiscale hybrid simulation
│   ├── Quantum impurity and fragment solvers [H]
│   │   ├── DMFT
│   │   │   └── QPU impurity observables or Green functions enter a classical dynamical mean-field
│   │   │       self-consistency loop.
│   │   ├── DMET
│   │   │   └── a quantum fragment solver supplies correlated information to a classical density-matrix
│   │   │       embedding loop.
│   │   ├── Active-space and EWF methods
│   │   │   └── classically construct orbitals and fragments; solve selected correlated electronic problems
│   │   │       using VQE, SQD, QPE, or another compatible method.
│   │   ├── QM/MM
│   │   │   └── the quantum-mechanical region has an explicit QPU subproblem, while classical molecular
│   │   │       mechanics represents the environment.
│   │   └── DFT interfaces
│   │       └── a specified QPU correction or impurity task supplies correlated inputs to a classical DFT
│   │           workflow.
│   ├── Nuclear trajectories and reactions [H]
│   │   ├── QPU-assisted Born–Oppenheimer MD
│   │   │   └── quantum electronic estimates and force information drive classical nuclear integration.
│   │   ├── QCPMD
│   │   │   └── electronic circuit parameters and classical nuclear coordinates evolve through coupled
│   │   │       equations, updating the electronic representation during nuclear propagation [14].
│   │   ├── Nonadiabatic dynamics
│   │   │   └── represent electronic transitions and nuclear coupling explicitly; nuclear quantum effects
│   │   │       require a compatible nuclear representation.
│   │   └── Force recovery
│   │       └── include derivative operators, basis-response terms when required, and sampling error in
│   │           trajectory stability.
│   └── Fine-to-coarse coupling [H; R for extensions requiring construction]
│       ├── Quantum numerical homogenization
│       │   └── selected fine-scale quantum solution functionals modify a classical coarse PDE construction
│       │       [17].
│       ├── Domain decomposition
│       │   └── communicate explicitly defined interface data with convergence and reconstruction costs.
│       ├── Quantum-derived material parameters
│       │   └── electronic or spin observables supply exchange, response, or constitutive inputs to
│       │       classical continuum models.
│       └── Micromagnetic interfaces
│           └── QPU-derived spin information supplies parameters for classical LLG models through a justified
│               reduction from microscopic spin dynamics to magnetization dynamics.
├── IX. Thermal states, quantum sampling, and Monte Carlo interfaces
│   ├── Quantum thermal preparation [Q/H/AQ]
│   │   ├── TFD and purification
│   │   │   └── enlarge the quantum system so tracing out an auxiliary register yields the intended thermal
│   │   │       state.
│   │   ├── VQT and product-spectrum ansätze
│   │   │   └── minimize free-energy objectives with an explicit entropy evaluation or tractable entropy
│   │   │       model.
│   │   ├── QITE, QMETTS, and TPQ routes
│   │   │   └── prepare approximate thermal states or quantum thermal ensembles with preparation and
│   │   │       averaging costs.
│   │   └── Quantum Metropolis and dissipative Gibbs sampling
│   │       └── energy resolution, detailed balance, and mixing determine validity and cost.
│   ├── Quantum expectation estimation [Q]
│   │   ├── QAE and iterative variants
│   │   │   └── estimate expectations through coherent preparation and amplification; assess oracle calls
│   │   │       together with their implementation cost to determine runtime [15].
│   │   └── Quantum walks for sampling
│   │       └── improvements depend on accessible transitions and spectral gaps.
│   └── QPU-assisted classical Monte Carlo [H]
│       ├── SQD/QSCI trial states for phaseless AFQMC
│       │   └── quantum-generated information enters a classical stochastic solver [16].
│       ├── Quantum-generated proposals
│       │   └── acceptance and stationary-distribution checks are essential.
│       └── Scope
│           └── VMC, DMC, PIMC, AFQMC, and tensor-network calculations use classical representations [C];
│               a specified QPU subroutine defines a quantum–classical hybrid [H].
├── X. Supporting hardware and data operations, shared by every branch
│   ├── CPU/GPU mathematical coprocessing [C within H]
│   │   ├── Before QPU execution
│   │   │   └── basis construction, molecular integrals, active spaces, meshes, preprocessing, and circuit
│   │   │       compilation.
│   │   ├── Inside hybrid loops
│   │   │   └── optimization, reduced diagonalization, configuration recovery, self-consistency,
│   │   │       trajectories, and classical sparse solves.
│   │   └── After QPU execution
│   │       └── spectra, Fourier transforms, uncertainty estimates, instrument models, and parameter
│   │           inference.
│   ├── ASIC/FPGA control and acceleration [C]
│   │   ├── Control/readout
│   │   │   └── waveform generation, sequencing, digitization, discrimination, reset, and feedforward
│   │   │       support QPU execution directly.
│   │   ├── Error processing
│   │   │   └── a specified ASIC or FPGA decoder processes error syndromes for an error-corrected
│   │   │       implementation of the scientific solver.
│   │   ├── Numerical acceleration
│   │   │   └── a dedicated classical accelerator performs an explicit matrix, sampling, or reduction task
│   │   │       when the design supports it.
│   │   └── Scope
│   │       └── ASICs support control, decoding, or numerical stages according to their implemented design.
│   ├── Classical analog coprocessing [CA; R for the proposed combined target]
│   │   ├── Analog matrix-vector operations
│   │   │   └── proposed electronic or photonic accelerators support an explicit classical linear-algebra
│   │   │       stage.
│   │   ├── Classical oscillator or field networks
│   │   │   └── a calibrated physical model supplies a classical reduced response.
│   │   └── Hybrid validity
│   │       └── conversion, noise, drift, residual correction, and precision costs must fit the complete
│   │           error budget.
│   └── Validation and resource accounting
│       ├── Matched baselines
│       │   └── compare the same model, observable, precision, and total processing cost against appropriate
│       │       CPU/GPU methods.
│       ├── Error budget
│       │   └── separate physical-model, discretization, truncation, quantum-algorithm, sampling, hardware,
│       │       and hybrid-coupling errors.
│       ├── Data access
│       │   └── coherent matrix or QRAM access requires an explicit quantum interface to the data; include
│       │       its construction and access costs.
│       └── Output and advantage
│           └── state preparation, normalization, postselection, repetitions, classical processing, and
│               full-field recovery count toward total cost.
└── XI. Computational characterization, reconstruction, and inverse design
    ├── Inverse-problem primitives [Q/H/QA; model-specific]
    │   ├── Regularized linear reconstruction [Q/H]
    │   │   └── QLSA, QSVT, VQLS, or projected methods target a coherently accessible linear subproblem. Include
    │   │       input loading, conditioning, preconditioning, normalization, and selected-observable or
    │   │       full-image recovery [6–8].
    │   ├── Discrete reconstruction and model selection [QA/H; Q/H for a specified QAOA mapping]
    │   │   └── Encode a bounded integer or binary objective as QUBO/Ising variables. Quantum annealing or
    │   │       gate-based optimization processes the explicit objective; discretization, penalties,
    │   │       connectivity, and classical work determine total cost [23].
    │   └── Nonlinear reconstruction and parameter inference [H; R for targets requiring construction]
    │       └── A classical outer loop updates sample and nuisance parameters while a specified quantum
    │           subroutine evaluates selected forward predictions, derivatives, or linearized updates.
    ├── Electron tomography and atomic electron tomography [H; R for electron targets requiring validation]
    │   ├── Projection-based reconstruction
    │   │   └── Under a justified projection approximation, discretized line-integral data define a regularized
    │   │       inverse problem. Multiple scattering and nonlinear image formation require a forward model
    │   │       that represents those interactions.
    │   ├── Regional annealing refinement
    │   │   └── A 2026 study uses a D-Wave hybrid solver for compact regional QUBO updates. Validation
    │   │       covers CT phantoms and a chest slice; electron tomography is a proposed application [23].
    │   └── Atomic-coordinate and potential refinement
    │       └── Proposed quantum subproblems refine a reduced structural description or selected potential
    │           coefficients; classical alignment, segmentation, and instrument modeling remain explicit.
    │           Additional measurements and justified priors constrain the missing-wedge region.
    ├── 4D-STEM ptychography and ptychographic electron tomography [H/R]
    │   ├── Complex object and probe reconstruction
    │   │   └── Overlapping diffraction intensities constrain complex transmission, probe parameters, positions,
    │   │       and coherence. Multislice formulations describe thicker specimens; the complete reconstruction
    │   │       is generally nonlinear [24].
    │   ├── Candidate QPU subproblems
    │   │   └── Quantum propagation, inverse action on a linearized update, or reduced parameter estimation
    │   │       requires a target-specific construction. Classical CPU/GPU multislice propagation is a baseline.
    │   ├── Encoding and naming
    │   │   └── 4D-STEM comprises two scan-position and two diffraction coordinates. Quantum-state ptychography
    │   │       reconstructs an unknown quantum state. Electron-image reconstruction uses a sample–probe
    │   │       forward model and a target-specific inverse algorithm [24, 25].
    │   └── Output constraints
    │       └── Intensity constraints, signed or complex-amplitude recovery, probe/object ambiguities, detector
    │           response, and full-image export determine the reconstruction and readout budget [7, 24].
    ├── ARPES, EELS, and other electronic spectroscopies [Q/H]
    │   ├── ARPES spectral prediction
    │   │   └── Quantum electronic dynamics supplies selected removal spectra or spectral functions. Classical
    │   │       photoemission matrix elements, occupations, surface/final-state effects, and resolution map
    │   │       these quantities to measured intensities [26, 27].
    │   ├── ARPES hardware scope
    │   │   └── A 2026 preprint reports a 27-site spectral-function calculation using 54 Quantinuum H2 qubits; the
    │   │       model study supplies hardware evidence. Real-material ARPES requires material-specific
    │   │       electronic structure, photoemission, and instrument models [26, 27].
    │   ├── EELS response prediction
    │   │   └── A quantum algorithm evaluates dynamical structure factors for momentum-resolved core-level
    │   │       spectroscopy; classical beam and scattering models supply the instrument-specific response.
    │   │       Battery-material calculations include fault-tolerant resource estimates [28].
    │   └── Other absorption, emission, and scattering spectra
    │       └── Reuse compatible excitation, transition-matrix-element, or correlation-function constructions
    │           from branches I–III. Optical, X-ray, Raman, and spin-resonance targets require their own
    │           operators, selection rules, linewidths, and evidence assessment.
    ├── NMR, spin spectroscopy, and Hamiltonian inference [Q/H/AQ]
    │   ├── Nuclear-spin spectral prediction
    │   │   └── State preparation, spin dynamics, and correlation measurements yield selected NMR spectra. A
    │   │       trapped-ion demonstration uses four qubits for a small zero-field NMR system [29].
    │   ├── Hamiltonian parameter learning
    │   │   └── A quantum simulator supplies predictions and specified derivatives inside a classical fit to
    │   │       time-resolved measurements; published NMR constructions assess learning of nuclear-spin
    │   │       interactions [30].
    │   └── Ensemble and pulse models
    │       └── Classical pulse descriptions, orientation averaging, calibration, and fitting connect the
    │           simulated spin model to a measurement; electronic-structure calculations provide chemical
    │           shifts when included in the model.
    ├── Computational interferometry and coherent spectroscopy [Q/H]
    │   ├── Phase-sensitive amplitudes
    │   │   └── Hadamard tests or other explicit reference protocols estimate complex overlaps and
    │   │       autocorrelations; ordinary SWAP fidelity estimates omit the complex phase. A photonic-chip study
    │   │       demonstrates generalized computational spectroscopy [31].
    │   └── Instrument connection
    │       └── Classical geometry, reference calibration, phase unwrapping, and detector response convert
    │           selected simulated amplitudes into predicted interference signals. Measured sample phases
    │           supply experimental data for comparison or inference.
    ├── Ellipsometry and polarimetry [H/R for proposed computational targets]
    │   ├── Microscopic optical response
    │   │   └── Selected current or polarization correlators supply compatible optical-response quantities. A
    │   │       classical constitutive model maps these to dielectric/conductivity tensors and sample-specific
    │   │       propagation.
    │   ├── Optical inversion
    │   │   └── Classical Fresnel or transfer-matrix calculations and Jones/Mueller models connect sample
    │   │       parameters to observables. Candidate quantum-assisted fits require an explicit subproblem;
    │   │       depolarization and identifiability must be retained.
    │   └── Quantum sensing role
    │       └── Squeezed or other quantum probe states modify measurement precision in quantum ellipsometry;
    │           this is a sensing resource [32].
    ├── Magnetometry, magnetic imaging, and field reconstruction [H/R]
    │   ├── Microscopic magnetic predictions
    │   │   └── Spin simulation supplies selected magnetization, susceptibility, or noise correlations under a
    │   │       specified Hamiltonian. Coarse-graining and magnetostatic propagation relate these to sensor
    │   │       observables.
    │   ├── Inverse field and source models
    │   │   └── A proposed quantum-assisted subproblem infers a reduced source description or selected model
    │   │       parameters; spatial sampling, sensor transfer functions, boundary assumptions, and source
    │   │       nonuniqueness remain part of the inverse problem.
    │   └── Sensor–processor interface
    │       └── NV centers, atomic magnetometers, and SQUIDs acquire magnetic-field data. A specified
    │           computational subroutine or coherent sensor–processor algorithm connects sensing to a QPU.
    ├── Mass spectrometry and molecular identification [H/R; construction-specific Q]
    │   ├── Quantum chemistry and fragmentation
    │   │   └── Candidate QPU electronic calculations supply ionic energies, ionization channels, or dynamical
    │   │       inputs. Classical nuclear trajectories, collision ensembles, reaction networks, and detector
    │   │       models determine fragment yields; classical QCxMS is a baseline [33].
    │   ├── Identification and candidate search
    │   │   └── A published algorithmic proposal applies quantum graph methods to a metabolite-identification
    │   │       stage [34].
    │   └── Scope
    │       └── Mass-to-charge separation defines the measurement, isotope models predict abundance patterns,
    │           and spectral assignment links observed peaks to candidate species.
    │           CPU/GPU quantum chemistry supplies classical electronic inputs [C]; a specified QPU
    │           subroutine defines the quantum contribution.
    ├── Computational EUV lithography [Q/H; R for general inverse-design extensions]
    │   ├── Resist photoabsorption and photoemission
    │   │   └── Explicit quantum spectral algorithms provide microscopic inputs for a model photoresist; see the
    │   │       readiness table for logical resource estimates [35].
    │   ├── Mask fields and inverse lithography
    │   │   └── Candidate quantum Maxwell or optimization subproblems require an explicit mask/source mapping,
    │   │       boundary treatment, and output objective. Classical inverse EUV lithography already incorporates
    │   │       optical and resist effects [19–21, 36].
    │   └── Multiscale causal connection
    │       └── Microscopic cross sections and emitted-electron energies directly parameterize
    │           absorption/emission events; downstream transport, chemistry, and development mediate their
    │           indirect influence on lithographic blur and pattern fidelity.
    └── Acquisition design, uncertainty, and validation [H/R]
        ├── Selected design objectives
        │   └── Proposed QPU estimates or quantum optimization guide chosen tilts, probe settings, spectral
        │       points, or process parameters through a specified forward model, utility, and uncertainty
        │       construction.
        └── Matched experimental goals
            └── Compare the same reconstruction precision, model, dose, acquisition time, and total computation
                with appropriate classical methods. 
```

## Forward prediction, inverse reconstruction, and measurement interpretation

Forward prediction evaluates a sample and instrument model, while inverse reconstruction infers sample parameters from measured data. These roles are independent of the temporal formulations above: an inverse fit might use static projections, frequency-resolved spectra, or time-dependent signals. A QPU directly evaluates its assigned operator action, quantum correlation, or optimization subproblem; the surrounding physical and instrument models mediate the connection to the final observable. The R placements in branch XI identify proposed applications of available primitives and the target-specific development needed for implementation.

$$
\boldsymbol{\theta}\xrightarrow{\;\mathcal{F}\;}\boldsymbol{y}_{\mathrm{pred}}, \qquad \boldsymbol{y}_{\mathrm{meas}}\longrightarrow\widehat{\boldsymbol{\theta}}.
$$

The parameter vector $\boldsymbol{\theta}$ might describe atomic coordinates, electrostatic potentials, a spin Hamiltonian, optical constants, or a lithographic mask. The forward map $\mathcal{F}$ includes the sample–probe interaction and instrument response. A general regularized inverse formulation is

$$
\widehat{\boldsymbol{\theta}} =\underset{\boldsymbol{\theta}\in\mathcal{C}}{\mathrm{argmin}} \left[D\!\left(\mathcal{F}(\boldsymbol{\theta}),\boldsymbol{y}_{\mathrm{meas}}\right) + \lambda R(\boldsymbol{\theta}) \right].
$$

Here, $D$ is a discrepancy appropriate to the measurement-noise model, $R$ represents prior information, $\lambda$ controls regularization, and $\mathcal{C}$ specifies physical constraints. Weighted least squares is appropriate for a justified Gaussian approximation, whereas low-count measurements often require a Poisson likelihood. A linear quantum solver addresses a compatible linear problem or linearized update [6–8]. For a least-squares normal-equation construction, $A^\dagger A$ squares the spectral condition number when $A$ has full column rank; alternative formulations and preconditioning therefore belong in the resource assessment.

```text
Computational characterization workflow
├── Forward signal prediction
│   ├── Quantum material model: selected states, dynamics, or correlations
│   ├── Classical probe interaction, propagation, and instrument response
│   └── Predicted spectrum, scattering intensity, field, or interference observable
├── Inverse reconstruction
│   ├── Measured data, uncertainty model, constraints, and priors
│   ├── Explicit quantum subproblem inside a classical reconstruction loop
│   └── Requested output: reduced parameters, selected functionals, or full image
└── Experimental and process design
    ├── Defined utility for precision, dose, coverage, or process tolerance
    ├── Candidate settings evaluated with a validated forward model
    └── Total acquisition and computation costs compared with classical baselines
```

For coherent thin-specimen ptychography, a simplified forward intensity model is

$$
I(\boldsymbol{R},\boldsymbol{q})
=\left|
\mathcal{F}_{\boldsymbol{r}\rightarrow\boldsymbol{q}}
\left[P(\boldsymbol{r}-\boldsymbol{R})O(\boldsymbol{r})\right]
\right|^2.
$$

The probe $P$ and object transmission $O$ generate an exit wave whose squared Fourier magnitude determines the measured diffraction intensity. Overlapping probe positions constrain phase recovery, while multislice propagation, partial coherence, positional uncertainty, and detector response refine the forward model [24]. The quantum Fourier transform (QFT) transforms amplitudes encoded in a quantum state; exporting classical transform values requires measurement and reconstruction. A QPU ptychographic workflow therefore includes initial-data loading, nonlinear intensity constraints, complex-amplitude recovery, and full-image readout in its resource budget. Quantum-state ptychography reconstructs unknown quantum states through overlapping projections [25]; electron ptychography reconstructs specimen and probe parameters through the electron-imaging forward model [24].

For equilibrium ARPES under the sudden approximation, the predicted intensity satisfies $I(\boldsymbol{k},\omega)\propto |M(\boldsymbol{k},\omega)|^2 f(\omega)A(\boldsymbol{k},\omega)$ before background and instrument-response processing. A QPU evaluates selected spectral-function quantities $A$, while photoemission matrix elements $M$, occupations $f$, surface/final-state effects, and detector response connect them to measured intensity [26, 27]. Electronic spectral functions, density-response structure factors, and spin-response structure factors use operators matched to their probes. ARPES, EELS, and magnetic INS consequently share mathematical primitives while requiring their respective physical response and instrument models [3, 4, 28]. Quantum-assisted Hamiltonian inference fits model predictions to measured data, connecting forward spectroscopy with inverse characterization [30].

Quantum sensors acquire data through physical probes and measurement protocols, while quantum computers execute assigned computational tasks. An integrated sensor–processor workflow requires a specified interface and a resource estimate for both stages. Squeezed-light ellipsometry uses quantum probe states to improve measurement precision, with classical or quantum-assisted fitting chosen for the inference stage [32]. Full-image reconstruction, selected response coefficients, and microscopic predictions each define their own output requirements, so readiness is assessed for the specified computational task.

## Application-to-solver mapping

| Application | Direct quantum task | Essential or useful classical task | Physical scope and output requirements |
| --- | --- | --- | --- |
| Molecular electronic ground-state energies | VQE, ADAPT-VQE, SQD/QSCI, Krylov, QITE, or QPE | Integrals, orbital selection, optimization or projected diagonalization | Accuracy is specific to basis, active space, preparation, and sampling. |
| Periodic electronic structure and band gaps | Selected eigenstates, charged excitations, or Green functions | Periodic basis, momentum sectors, embedding, spectral fitting | Band dispersion requires momentum-resolved excitation or spectral information. |
| Molten salts and complex molecular environments | Extended SQD for correlated embedded fragments | AIMD snapshots, EWF fragmentation, CPU/GPU diagonalization | Total binding-energy accuracy also depends on fragmentation and embedding errors [2]. |
| Magnetic inelastic neutron scattering | Spin-state preparation, real-time evolution, correlation measurements | Momentum/time transforms, form factors, instrument convolution | Simulates material response; limited time windows affect energy resolution [3, 4]. |
| Computational interferometry | Complex overlaps, controlled phases, propagator matrix elements | Geometry, model assembly, reconstruction, fitting | Squared overlaps quantify fidelity; complex phases require reference interference. |
| Chemical reaction scattering | Wavepacket evolution and transition amplitudes | Potential surfaces, incoming/outgoing channel normalization, rate integration | The cited reaction study reports classical circuit simulations; hardware execution requires implementation and validation [9]. |
| Quantum-lattice-Boltzmann fluid dynamics | Encoded streaming and a specified collision construction | Boundary updates, nonlinear feedback where needed, moment recovery | Mesoscopic populations and the kinetic closure must fit the intended flow regime [10]. |
| Quantum FEM/FVM | Quantum inverse action, residual estimation, or generalized eigenproblem | Mesh assembly, constraints, preconditioners, interface data | Budget targeted measurements for selected functionals and value recovery for full mesh reconstruction [5, 6, 11]. |
| Quantum FDTD and related transient finite-difference evolution | Maxwell/Yee-type Schrödingerisation, driven-source circuits, or discretized wavefunction propagation | Geometry, field preparation, sources, boundary treatment, signed-field recovery | The temporal update specifies the propagation scheme. Evidence includes two-dimensional hardware benchmarks and three-dimensional simulator results [19–21]. |
| Nanodevice electromagnetic or elastic fields | A specified PDE solution-state subroutine | Geometry, constitutive parameters, constraints, verification | Nanoscale constitutive and nonlocal effects require a valid physical model. |
| Molecular dynamics | Electronic energies, forces, or electronic parameter evolution | Nuclear trajectories, thermostats, cell updates | Classical nuclear propagation is an approximation; force noise affects dynamics [14]. |
| Vibronic, phononic, photonic, and magnonic dynamics | Bosonic/qudit evolution or native coupled-mode simulation | Mode fitting, cutoffs, analysis | Anharmonic effects require corresponding interaction terms and compatible evolution operations. |
| Decoherence and quantum-device dissipation | Channel, bath, or trajectory simulation | Ensemble averaging, fitting, validation | The bath model and memory times determine the applicable channel, generator, or explicit-memory construction. |
| Strongly correlated materials | Quantum impurity or fragment solver | DMFT/DMET self-consistency and embedding | The error budget includes both solver and embedding accuracy. |
| Finite-temperature response | Thermal state preparation and correlations | Ensemble statistics, transforms, thermodynamic analysis | Entropy, thermalization, and mixing costs are essential. |
| Quantum-assisted Monte Carlo | Trial-state information or coherent expectation subroutine | AFQMC or another explicitly coupled stochastic algorithm | Classical bias and oracle costs remain part of the result [15, 16]. |
| Multiscale homogenization and spintronics | Fine-scale solution functionals or microscopic correlations | Coarse PDE or micromagnetic evolution | Coupling and reduction errors require independent validation [17]. |
| Electron tomography and atomic electron tomography | A suitable quantum linear subproblem or discrete reconstruction objective [Q/H/QA; R for electron targets requiring validation] | Alignment, projection or multiple-scattering models, priors, coordinate refinement, and volume recovery | Regional hybrid CT refinement supplies a starting construction. Atomic electron tomography requires validation of electron image formation, angular coverage, structural accuracy, and volume recovery [23]. |
| 4D-STEM ptychography and ptychographic electron tomography | Candidate quantum propagation, linearized inverse action, or reduced parameter inference [H/R] | Probe/object optimization, multislice propagation, position/coherence correction, and detector models | Nonlinear intensity constraints and complex-image recovery require a target-specific electron-imaging construction. Quantum-state ptychography supplies methods for quantum-state reconstruction [24, 25]. |
| ARPES | Selected removal spectra, Green functions, or momentum-resolved spectral functions [Q/H] | Matrix elements, occupations, final-state/surface effects, background, and resolution | A 54-qubit, 27-site model supplies hardware evidence; real-material intensities require material-specific spectra and photoemission/instrument modeling [26, 27]. |
| EELS and electronic inelastic scattering | Density-response dynamical structure factors and core-excitation dynamics [Q/H] | Beam propagation, scattering kinematics, geometry, and detector acceptance | The battery-cluster framework supplies a quantum algorithm and fault-tolerant resource estimates for its selected EELS target [28]. |
| NMR and spin spectroscopy | Spin-Hamiltonian evolution, correlators, and specified Hamiltonian-learning derivatives [Q/H/AQ] | Pulses, orientation averages, spectral reconstruction, calibration, and fitting | Small-system hardware spectra provide evidence; larger networks require assessment of dynamical accuracy and parameter identifiability [29, 30]. |
| Ellipsometry | Candidate microscopic optical-response quantities or an explicit fitting subproblem [H/R] | Fresnel/transfer-matrix calculations, dielectric models, calibration, and thickness/optical-constant inference | Parameter nonuniqueness and constitutive assumptions remain; quantum-enhanced probe precision is a sensing result [32]. |
| Polarimetry | Candidate polarization/current response predictions or specified parameter estimation [H/R] | Jones/Mueller propagation, depolarization models, calibration, and Stokes-data processing | Quantum acceleration requires a specified microscopic-response or inverse subproblem with a favorable total cost relative to classical polarization processing. |
| Magnetometry and magnetic imaging | Candidate spin correlations, susceptibility, magnetization, or a reduced inverse subproblem [H/R] | Magnetostatics, sensor transfer functions, spatial reconstruction, and uncertainty analysis | Sensor sampling and source nonuniqueness constrain reconstruction. Quantum sensors acquire data; QPU subroutines process specified model or inverse tasks. |
| Mass spectrometry and MS/MS identification | Candidate ionic electronic/dynamical inputs or a specified graph/search algorithm [H/R; construction-specific Q] | Fragmentation trajectories, collision ensembles, kinetics, isotope patterns, and instrument response | Evidence includes classical fragmentation simulation [C] and an algorithmic proposal for quantum-assisted identification; full-spectrum QPU prediction requires a complete construction [33, 34]. |
| EUV photoresist spectroscopy and microscopic process inputs | Selected microscopic spectra [Q/H] | Electron cascades, chemistry, development, and pattern statistics | Model-specific resources are listed below; process prediction requires multiscale coupling [35]. |
| EUV mask, source, and inverse process design | Candidate quantum electromagnetic or optimization subproblems [H/R] | Mask geometry, imaging, resist effects, process windows, and manufacturing constraints | A full-chip workflow requires explicit construction and end-to-end benchmarking. Resist spectra supply microscopic process inputs; mask optimization targets the resulting pattern objective [19–21, 36]. |
| Acquisition and model design for characterization | Proposed selected-response estimates or explicit quantum optimization of settings [H/R] | Utility definition, calibration, statistical design, and experimental execution | Assess dose reduction and quantum advantage at matched precision, including acquisition and total computation costs. |

## Readiness interpretation

| Family | Evidence or implementation level | Scaling interpretation |
| --- | --- | --- |
| VQE and related variational methods | Small hardware demonstrations and extensive algorithm studies | Optimization, ansatz error, conditioning, and measurements limit larger targets. |
| SQD and selected-space methods | Hardware electronic-structure examples with substantial classical computation [1, 2] | Classical subspace size and coverage of important configurations govern convergence and cost. |
| Dynamical structure factors and INS | Hardware model studies; the 2026 KCuF3 preprint reports up to 50 qubits [3, 4] | Time window, depth, fidelity, and model mismatch govern spectral accuracy. |
| QLSA/QSVT/QPE | Established algorithmic primitives with small demonstrations for selected constructions | Large scientific targets generally motivate error-corrected execution and explicit coherent access. |
| Quantum FEM | Specific preconditioned elliptic construction includes hardware experiments [5] | Evidence covers the specified preconditioned elliptic construction; larger FEM targets require validation of their operators, boundaries, accuracy, and outputs. |
| Quantum time-domain Maxwell finite differences | Specific numerical constructions and a 2026 hardware preprint with two-dimensional IonQ benchmarks [19–21] | Assess the intended three-dimensional geometry, lossy boundaries, full-field readout, and total cost at matched accuracy. |
| QLBM and quantum FVM | Formulation-dependent theory, numerical benchmarks, and small prototypes | Report grid size together with execution platform, accuracy, and total cost; physical-QPU validation and matched classical comparisons establish hardware performance. |
| Analog quantum models | Experimental programmable simulation of selected interactions | Model matching, control range, observable access, and calibration are decisive. |
| QCPMD, general multiscale coupling, and oscillator–qubit LCHS | Research constructions with different evidence levels [13, 14, 17] | Each proposed target needs its own accuracy and total-resource assessment. |
| QPU plus classical analog numerical coprocessor | Proposed placement in this taxonomy | Each proposed hybrid needs a defined numerical stage, conversion/error budget, and comparison of total runtime with a classical baseline. |
| Quantum-assisted tomographic reconstruction | Small annealing studies and regional D-Wave hybrid refinement for discrete CT examples [23] | Electron-tomography compatibility is a proposed extension; quantify classical work, discretization, total runtime, and matched image accuracy. |
| Quantum-assisted 4D-STEM ptychographic reconstruction | Target-specific research placement; established classical electron ptychographic tomography supplies a baseline [24, 25] | Data loading, nonlinear updates, coherent operator access, and full complex-image recovery require a complete construction. |
| ARPES spectral-function calculation | A 2026 preprint reports a 54-qubit hardware calculation for a 27-site model [26] | Real-material prediction requires material-specific model validation, state preparation, sampling, and photoemission/instrument components. |
| Core-level EELS quantum simulation | Algorithm and resource estimates for an 18-active-orbital battery-material cluster: approximately 100 logical qubits, circuit depth reported as 3.25 × 10^8 T gates, and roughly 10^4 shots [28] | These logical-qubit and circuit estimates specify fault-tolerant requirements; hardware deployment also requires the associated physical-qubit and error-correction budget. |
| NMR spectral simulation and Hamiltonian learning | Four-qubit trapped-ion zero-field NMR demonstration and algorithmic constructions for learning spin interactions [29, 30] | Larger spin networks require adequate evolution fidelity, ensemble handling, sampling, and parameter identifiability. |
| Generalized computational spectroscopy | Ancilla-assisted autocorrelation spectroscopy demonstrated on a programmable silicon-photonic processor [31] | Target Hamiltonians, state preparation, controlled dynamics, and observable access determine applicability. |
| EUV photoresist microscopic spectroscopy | A 2026 preprint, revised August 2026, estimates about 200 logical qubits, 10^9 non-Clifford gates per absorption circuit, and 10^3 shots; its photoemission model needs several thousand logical qubits, at least 10^14 gates, and 10^4 shots [35] | The estimates specify fault-tolerant microscopic resist calculations. Full-chip inverse lithography requires coupling these inputs to imaging, transport, chemistry, and development models. |
| Computational ellipsometry, polarimetry, and magnetometry extensions | Candidate microscopic-response or reduced inverse subproblems; quantum ellipsometric theory addresses probe measurement precision [32] | Assess QPU acceleration for an explicit microscopic or inverse subproblem, including its interface to classical fitting or sensor data. |
| Quantum-assisted mass-spectrum prediction and identification | Established classical fragmentation baselines and a published quantum identification proposal [33, 34] | Assess electronic-input accuracy, fragment-yield accuracy, and identification performance against their corresponding classical baselines. |
| Quantum-assisted mask or acquisition optimization | Application-specific research extensions | Explicit objectives, mappings, classical baselines, uncertainty, and complete resource costs determine readiness. |

A Hamiltonian solver directly estimates electronic energies or spin correlations. An embedding approximation changes the effective Hamiltonian and thereby indirectly modifies those estimates. GPU sparse algebra directly solves the classical projected SQD problem, while ASIC control affects scientific accuracy indirectly through implemented gate and measurement fidelity. A classical analog coprocessor directly approximates a numerical operation; its noise, drift, and conversion errors propagate through the hybrid workflow into the final uncertainty.

Hybrid methods combine physical descriptions through explicit representations and coupling rules. The quantum lattice Boltzmann method (QLBM) adopts the kinetic structure of Boltzmann’s statistical description and implements it through a specified quantum encoding and evolution strategy. Quantum Car–Parrinello molecular dynamics (QCPMD) couples a QPU electronic representation to the electronic–nuclear dynamics framework of CPMD. The portmanteau “vibronic” combines vibrational and electronic and describes coupling between nuclear vibrational modes and electronic states.

A QPU-computed electronic spectral function supplies material-response information whose connection to ARPES intensity is mediated by photoemission matrix elements and instrument convolution [26, 27]. Quantum density correlations similarly supply dynamical structure factors for EELS scattering models [28]. In reconstruction, the QPU directly processes the assigned inverse or optimization subproblem, while alignment, regularization, and calibration modify the inferred structure through the surrounding workflow. Detector response, acquisition geometry, measured information, and justified priors constrain the achievable reconstruction resolution and uncertainty.

Quantum resist spectra [35], spin-Hamiltonian inference [30], thin-film ellipsometry, and magnetic-field inversion share a forward-model/inverse-inference architecture. Each modality specifies its physical operators, transfer functions, data model, and requested outputs. These shared mathematical roles support cross-references among the branches and guide selection of an application-specific quantum subproblem.

The acquisition terms “tomography,” “ptychography,” and “ellipsometry” describe how measurements support reconstruction or parameter inference. Tomography draws on the Greek for section, ptychography on the Greek for fold, and ellipsometry on measurement of polarization ellipses. “4D-STEM” specifies two scan-position coordinates and two diffraction coordinates. “Spectrotomography” combines spectral information with spatial reconstruction; a quantum-assisted implementation requires both a reconstruction model and a spectral model.

## Acronym glossary

| Acronym | Expansion | Role in this taxonomy |
| --- | --- | --- |
| QPU | Quantum processing unit | Executes a quantum subroutine or programmable quantum simulation. |
| ASIC | Application-specific integrated circuit | Dedicated classical control or numerical hardware. |
| FPGA | Field-programmable gate array | Reconfigurable classical control or acceleration hardware. |
| VQE | Variational quantum eigensolver | Estimates energies inside a classical optimization loop. |
| ADAPT-VQE | Adaptive derivative-assembled pseudo-Trotter VQE | Selects circuit operators using measured gradients. |
| VQD | Variational quantum deflation | Targets excited states using overlap penalties. |
| QSE | Quantum subspace expansion | Builds a projected eigenproblem from quantum matrix elements. |
| SQD | Sample-based quantum diagonalization | Selects classical diagonalization subspaces using quantum samples. |
| QSCI | Quantum-selected configuration interaction | Uses quantum configuration samples for a classical electronic eigenproblem. |
| SKQD | Sample-based Krylov quantum diagonalization | Uses quantum evolution samples to form a Krylov-inspired selected subspace. |
| QPE | Quantum phase estimation | Resolves phases of coherent evolution to estimate energies. |
| QITE | Quantum imaginary-time evolution | Approximates normalized imaginary-time state preparation. |
| VQS | Variational quantum simulation | Propagates a parameterized state with measured projected dynamics. |
| QSP | Quantum signal processing | Implements polynomial transformations using controlled quantum operations. |
| QSVT | Quantum singular value transformation | Transforms singular values of a block-encoded matrix. |
| LCU | Linear combination of unitaries | Constructs operator actions from weighted unitary components. |
| LCHS | Linear combination of Hamiltonian simulation | Represents suitable nonunitary evolution through Hamiltonian evolutions. |
| QLSA | Quantum linear system algorithm | Prepares a normalized state representing an inverse action. |
| HHL | Harrow–Hassidim–Lloyd | Foundational quantum linear-system algorithm named after its authors. |
| VQLS | Variational quantum linear solver | Fits a normalized solution state using quantum cost estimates. |
| FEM/ FVM | Finite element/ finite volume method | Discretizes spatial field equations. |
| FDTD/ FDFD | Finite-difference time-domain/ finite-difference frequency-domain | Combines finite-difference discretization with a specified temporal formulation. |
| QLBM | Quantum lattice Boltzmann method | Implements a specified encoded lattice kinetic model. |
| QLGA | Quantum lattice gas automaton | Implements collision and streaming on quantum occupation registers. |
| QCA | Quantum cellular automaton | Applies local quantum update rules. |
| DMFT/ DMET | Dynamical mean-field/ density matrix embedding theory | Embeds quantum impurity or fragment problems in a classical environment. |
| EWF | Embedded wavefunction | Partitions a larger electronic calculation into embedded fragment problems. |
| QM/MM | Quantum mechanics/ molecular mechanics | Couples a quantum-mechanical region to a classical environment. |
| QCPMD | Quantum Car–Parrinello molecular dynamics | Couples quantum electronic parameters to classical nuclear dynamics. |
| TFD | Thermofield double | Purifies a thermal state using an auxiliary quantum system. |
| VQT | Variational quantum thermalizer | Fits a thermal-state model using a free-energy objective. |
| QMETTS | Quantum minimally entangled typical thermal states | Samples a quantum thermal ensemble. |
| TPQ | Thermal pure quantum | Represents thermal observables through pure-state typicality. |
| QAE | Quantum amplitude estimation | Estimates quantum-encoded probabilities or expectations. |
| AFQMC | Auxiliary-field quantum Monte Carlo | Classical stochastic electronic solver with a possible QPU interface. |
| INS | Inelastic neutron scattering | Experimental response connected here to computed dynamical structure factors. |
| NEGF | Nonequilibrium Green function | Electronic transport framework with optional quantum-derived inputs. |
| DFT/ AIMD | Density functional theory/ ab initio molecular dynamics | Classical electronic and atomistic workflow components; a specified QPU subproblem supplies the quantum contribution in a hybrid. |
| QA | Quantum annealing | Optimization using an accessible annealing Hamiltonian. AQ denotes target-system simulation. |
| QAOA | Quantum approximate optimization algorithm | Gate-based optimization of a specified encoded objective; performance is assessed against matched classical solvers. |
| QUBO | Quadratic unconstrained binary optimization | Binary objective used by suitable annealing or gate-based optimization mappings, often with penalty terms. |
| STEM/ 4D-STEM | Scanning transmission electron microscopy/ four-dimensional STEM | Raster scan with diffraction data indexed by two scan and two diffraction coordinates. |
| ET/ AET | Electron tomography/ atomic electron tomography | Reconstructs sample volumes or atom-resolved structure from electron-microscopy measurements under a specified forward model. |
| CT | Computed tomography | Projection-based reconstruction used in the cited hybrid annealing examples. |
| ARPES | Angle-resolved photoemission spectroscopy | Relates electronic removal spectra to measured intensities through a photoemission and instrument model. |
| EELS | Electron energy-loss spectroscopy | Measures energy-transfer response linked to appropriate electronic dynamical structure factors. |
| DSF | Dynamical structure factor | Momentum- and frequency-resolved correlation spectrum; the operator differs between density and spin probes. |
| NMR | Nuclear magnetic resonance | Spin spectroscopy with quantum simulation and Hamiltonian-inference applications. |
| NV/ SQUID | Nitrogen-vacancy center/ superconducting quantum interference device | Quantum sensing platforms that acquire data; a specified computational interface connects them to QPU processing. |
| EUV/ ILT | Extreme ultraviolet/ inverse lithography technology | Lithographic process and inverse pattern-design workflow; quantum resist spectroscopy supplies selected microscopic process inputs. |
| MS/ MS/MS | Mass spectrometry/ tandem mass spectrometry | Mass-to-charge and fragmentation measurements; forward models predict spectra and identification algorithms infer candidate species. |
| FFT/ QFT | Fast Fourier transform/ quantum Fourier transform | Classical array transform and quantum amplitude transform, respectively; their inputs and outputs differ. |
| Jones/ Mueller | Jones polarization amplitudes/ Mueller Stokes-vector transfer matrix | Optical propagation descriptions; Mueller models accommodate depolarization under appropriate physical constraints. |

## References

1. Robledo-Moreno et al., *Chemistry beyond the scale of exact diagonalization on a quantum-centric supercomputer*, Science Advances (2025). [Paper](https://arxiv.org/abs/2405.05068).
2. Das et al., *Quantum Computations on Fusion Blanket Molten Salts* (2026 preprint). [Paper](https://arxiv.org/abs/2606.30402). The abstract reports fragment agreement alongside much larger fragmentation errors in conformational and binding energies.
3. Lee et al., *Benchmarking quantum simulation with neutron-scattering experiments* (2026 preprint). [Paper](https://arxiv.org/abs/2603.15608).
4. Chiesa et al., *Quantum hardware simulating four-dimensional inelastic neutron scattering*, Nature Physics (2019). [Paper](https://www.nature.com/articles/s41567-019-0437-4).
5. Deiml and Peterseim, *Quantum Realization of the Finite Element Method*. [Paper](https://arxiv.org/abs/2403.19512).
6. Bravo-Prieto et al., *Variational Quantum Linear Solver*, Quantum (2023). [Paper](https://quantum-journal.org/papers/q-2023-11-22-1188/).
7. Harrow, Hassidim, and Lloyd, *Quantum algorithm for solving linear systems of equations*. [Paper](https://arxiv.org/abs/0811.3171).
8. Gilyén et al., *Quantum singular value transformation and beyond*. [Paper](https://arxiv.org/abs/1806.01838).
9. *Digital Quantum Simulation of Wavepacket Correlations in a Chemical Reaction*, Entropy (2026). [Paper](https://doi.org/10.3390/e28020144). The reported circuit results are classical simulations.
10. Wang et al., *Quantum lattice Boltzmann method for simulating nonlinear fluid dynamics*. [Paper](https://arxiv.org/abs/2502.16568).
11. Chen et al., *Quantum Finite Volume Method for Computational Fluid Dynamics with Classical Input and Output*. [Paper](https://arxiv.org/abs/2102.03557). Assess its QRAM and classical-output assumptions explicitly.
12. Jin, Liu, and Yu, *Quantum simulation of partial differential equations via Schrödingerisation*. [Paper](https://arxiv.org/abs/2212.13969).
13. Das et al., *Gate-Level Quantum Simulation of Nonunitary Linear Dynamics with Hybrid Oscillator-Qubit Architecture* (2026 preprint). [Paper](https://arxiv.org/abs/2605.10708).
14. Kuroiwa et al., *Quantum Car-Parrinello Molecular Dynamics*. [Paper](https://arxiv.org/abs/2212.11921).
15. Montanaro, *Quantum speedup of Monte Carlo methods*. [Paper](https://arxiv.org/abs/1504.06987).
16. Danilov et al., *Enhancing the accuracy and efficiency of sample-based quantum diagonalization with phaseless auxiliary-field quantum Monte Carlo*. [Paper](https://arxiv.org/abs/2503.05967).
17. Balazi, Deiml, and Peterseim, *Quantum Enhanced Numerical Homogenization* (2026 preprint). [Paper](https://arxiv.org/abs/2603.28521).
18. Motta et al., *Determining eigenstates and thermal states on a quantum computer using quantum imaginary time evolution*. [Paper](https://arxiv.org/abs/1901.07653).
19. Jin, Liu, and Ma, *Quantum simulation of Maxwell's equations via Schrödingerisation*, ESAIM: M2AN (2024). [Paper](https://arxiv.org/abs/2308.08408), [journal version](https://doi.org/10.1051/m2an/2024046).
20. Ma et al., *Schrödingerization based Quantum Circuits for Maxwell's Equation with time-dependent source terms*. [Paper](https://arxiv.org/abs/2411.10999).
21. Sharma et al., *Hardware Realization of a Hamiltonian Simulation Algorithm for Time-Domain Maxwells Equations* (2026 preprint). [Paper](https://arxiv.org/abs/2604.25229).
22. Na and Chew, *Quantum Electromagnetic Finite-Difference Time-Domain Solver*, Quantum Reports (2020). [Paper](https://doi.org/10.3390/quantum2020016). This is a classical numerical method for quantized electromagnetic-field propagation.
23. Lee and Jun, *Quantum-assisted tomographic image refinement with limited qubits for high-resolution imaging*, EPJ Quantum Technology (2026). [Paper](https://doi.org/10.1140/epjqt/s40507-026-00545-4). Regional D-Wave hybrid CT reconstruction.
24. *Solving complex nanostructures with ptychographic atomic electron tomography*, Nature Communications (2023). [Paper](https://doi.org/10.1038/s41467-023-43634-z). Electron-imaging and classical reconstruction baseline.
25. *Ptychography of pure quantum states*, Scientific Reports (2019). [Paper](https://doi.org/10.1038/s41598-019-52415-y). Quantum-state tomography through overlapping projections.
26. Granet et al., *Spectral functions on a quantum computer through system-environment interaction* (2026 preprint). [Paper](https://arxiv.org/abs/2605.01440). Reports a 27-site, 54-qubit Quantinuum H2 model demonstration.
27. *Computational framework chinook for angle-resolved photoemission spectroscopy*, npj Quantum Materials (2019). [Paper](https://doi.org/10.1038/s41535-019-0194-8). Classical photoemission matrix-element framework.
28. Kunitsa et al., *Quantum simulation of electron energy loss spectroscopy for battery materials*, Journal of Chemical Physics (2025). [Paper](https://doi.org/10.1063/5.0300557), [preprint](https://arxiv.org/abs/2508.15935). Quantum DSF algorithm and fault-tolerant resource estimates.
29. Seetharam et al., *Digital quantum simulation of NMR experiments*, Science Advances (2023). [Paper](https://doi.org/10.1126/sciadv.adh2594), [preprint](https://arxiv.org/abs/2109.13298). Four-qubit trapped-ion zero-field NMR demonstration.
30. *Quantum Computation of Molecular Structure Using Data from Challenging-To-Classically-Simulate Nuclear Magnetic Resonance Experiments*, PRX Quantum (2022). [Paper](https://doi.org/10.1103/PRXQuantum.3.030345), [preprint](https://arxiv.org/abs/2109.02163). Quantum-assisted inference of spin-Hamiltonian parameters.
31. Zhai et al., *Generalised quantum computational spectroscopy on a quantum chip*, Nature Communications (2026). [Paper](https://doi.org/10.1038/s41467-026-74936-7). Ancilla-assisted autocorrelation spectroscopy demonstrated on a silicon-photonic processor.
32. Rudnicki et al., *Fundamental quantum limits in ellipsometry*, Optics Letters (2020). [Paper](https://doi.org/10.1364/OL.392955), [preprint](https://arxiv.org/abs/2007.10440). Quantum measurement-precision analysis.
33. Koopman and Grimme, *From QCEIMS to QCxMS: A Tool to Routinely Calculate CID Mass Spectra Using Molecular Dynamics*, Journal of the American Society for Mass Spectrometry (2021). [Paper](https://doi.org/10.1021/jasms.1c00098). Classical quantum-chemical fragmentation simulation.
34. Tsai, Nuckels, and Wang, *Integrating Quantum Computing into De Novo Metabolite Identification*, Journal of Systemics, Cybernetics and Informatics (2023). [Paper](https://iiisci.org/Journal/PDV/sci/pdfs/ZA381UC23.pdf). Algorithmic proposal for a quantum-assisted metabolite-identification stage.
35. Kharazi et al., *Quantum Simulations for Extreme Ultraviolet Photolithography* (2026 preprint, version 2, revised 20 August 2026). [Paper](https://arxiv.org/abs/2602.20234v2).
36. *Gradient-based inverse extreme ultraviolet lithography*, Applied Optics (2015). [Paper](https://doi.org/10.1364/AO.54.007284). Classical inverse-EUV baseline incorporating optical and resist effects.

The complete workflow: physical model → encoded operator/state → quantum subroutine → classical or analog coprocessing → requested observable → validation at matched accuracy.
