# QPU and Hybrid Solver Selection Guide for Quantum, Atomistic, Molecular, and Nanoscale Simulation

Use this guide to map a physical simulation problem and requested output to a quantum or hybrid solver family. The selection router connects each route to its execution hardware, readiness evidence, implementation workflow, and resource checks.

## Quick start: solver selection router

Choose your requested output below and follow its solver branch. Check the [readiness interpretation](#readiness-interpretation) and [hardware–solver fit matrix](#hardware-solver-fit-matrix) for the cited implementation; [HW, NUM, ALG, and R](#evidence-legend) identify hardware results, numerical benchmarks, algorithm constructions, and research extensions.

| If your goal is… | Start at branch | Readiness/ evidence | Hardware or hybrid execution fit |
| --- | --- | --- | --- |
| Find molecular electronic ground-state energies | [I](#branch-i) | HW selected-space examples; ALG/NUM preparation routes [1, 8, 18] | Gate-based QPU + classical optimization or diagonalization |
| Predict periodic spectra, band gaps, or fragment energies | [I](#branch-i), [VIII](#branch-viii) | HW model/preprint examples; R for the selected material [2, 26] | QPU + CPU/GPU embedding and spectral analysis |
| Propagate a reaction wavepacket or estimate an interference amplitude | [II](#branch-ii), [III](#branch-iii) | NUM wavepacket example; R for the selected target [9] | Encoded quantum dynamics + classical channel or spectral analysis |
| Predict magnetic inelastic neutron scattering | [III](#branch-iii) | HW spin-model examples and a 2026 preprint [3, 4] | Quantum spin evolution + classical response reconstruction |
| Calculate an ARPES-related spectral function | [III](#branch-iii), [XI](#branch-xi) | HW 27-site model preprint [26] | Quantinuum H2 example + classical photoemission model |
| Model a dissipative nanodevice or correlated transport | [IV](#branch-iv), [VIII](#branch-viii) | R for the selected device or unresolved target | Gate-based or compatible native quantum evolution + classical transport/ensemble analysis |
| Solve an elliptic FEM problem or a finite-volume formulation | [V](#branch-v) | ALG/HW elliptic prototype; ALG FVM construction [5, 11] | Quantum solution subroutine + classical meshes, constraints, and preconditioning |
| Simulate a Maxwell pulse or transient finite-difference field | [V](#branch-v) | NUM/ALG constructions and HW 2D preprint [19–21] | IonQ QPU example + classical source, boundary, and readout handling |
| Simulate lattice-Boltzmann flow or kinetic populations | [VI](#branch-vi) | NUM nonlinear QLBM; R for candidate confined-flow targets [10] | Encoded streaming/collision + classical boundary and moment processing |
| Simulate a compatible spin, particle, or oscillator model | [VII](#branch-vii) | Experimental programmable simulation of selected interactions | Native analog quantum interactions or digital–analog sequences |
| Couple electronic estimates to nuclear motion or a coarse model | [VIII](#branch-viii) | NUM QCPMD; ALG/NUM homogenization preprint [14, 17] | QPU electronic/fine-scale subproblem + classical dynamics or coarse PDE |
| Estimate thermal observables or a stochastic expectation | [IX](#branch-ix) | HW prototypes/NUM thermal; ALG expectation estimation [15, 18] | Quantum preparation/estimation + classical averaging or stochastic solver |
| Refine a tomographic image or infer atomic structure | [XI](#branch-xi), [XII](#branch-xii) | HW regional CT example; R electron-tomography extension [23, 24] | D-Wave hybrid CT example; explicit quantum inverse/optimization subproblem for electron targets |
| Reconstruct a 4D-STEM object or electron ptychographic volume | [XI](#branch-xi), [XII](#branch-xii) | R computational electron target; C electron-imaging baseline [24, 25] | Candidate quantum propagation/inverse update + classical nonlinear reconstruction |
| Predict EELS or microscopic EUV photoresist spectra | [III](#branch-iii), [XI](#branch-xi) | ALG fault-tolerant resource estimates [28, 35] | Logical-qubit spectral construction + classical beam or process models |
| Predict NMR or coherent spectroscopic signals | [III](#branch-iii), [XI](#branch-xi) | HW small NMR and photonic examples; ALG inference [29–31] | Trapped-ion or silicon-photonic examples + classical fitting/spectral analysis |
| Fit optical/magnetic parameters, identify molecules, or select designs | [XI](#branch-xi), [XII](#branch-xii) | R selected computational targets; construction-specific proposals [32–36] | Explicit quantum response, inverse, or optimization subproblem + classical model/interface |
| Assign coprocessor work or account for complete execution costs | [X](#branch-x), [XII](#branch-xii) | Implementation-specific resource and validation assessment | QPU + CPU/GPU/ASIC/FPGA or a specified classical analog stage |

## Example decision journeys

### Predict a molecular electronic ground-state energy

1. **Goal and route:** Estimate a molecular electronic energy, then follow [Branch I](#branch-i).
2. **Solver choice:** For this journey, use sample-based quantum diagonalization (SQD), which selects a configuration subspace from quantum samples and solves the selected eigenproblem classically.
3. **Execution fit:** Assign sampling to a gate-based QPU and selected-space diagonalization to a CPU/GPU; use the [hardware–solver fit matrix](#hardware-solver-fit-matrix) to check the execution class.
4. **Readiness check:** Consult the SQD row in the [readiness table](#readiness-interpretation). The cited hardware examples include N2 dissociation and iron–sulfur molecular clusters [1].
5. **Cost and validation:** Account for basis and active-space choices, quantum preparation and sampling, classical subspace size, and configuration coverage. Compare the same energy and accuracy with a matched classical calculation using [complete-workflow resource accounting](#resource-accounting).

### Predict a magnetic inelastic neutron-scattering response

1. **Goal and route:** Obtain momentum- and frequency-resolved magnetic response, then follow [Branch III](#branch-iii).
2. **Solver choice:** Prepare and evolve the selected spin model, measure spin correlations, and reconstruct its magnetic dynamical structure factor.
3. **Execution fit:** Assign spin evolution and correlation measurements to a compatible quantum platform; assign transforms, polarization, magnetic form factors, and instrument resolution to classical processing.
4. **Readiness check:** Consult the INS row in the [readiness table](#readiness-interpretation). The document includes hardware studies of finite spin systems and a spin-chain benchmark [3, 4].
5. **Cost and validation:** Assess the time window, evolution depth, fidelity, sampling, and model mismatch against the requested spectral accuracy; include response reconstruction in [complete-workflow resource accounting](#resource-accounting).

### Simulate a transient Maxwell field

1. **Goal and route:** Predict a pulse or selected time-dependent electromagnetic field, then follow [Branch V](#branch-v) and its [temporal-formulation guide](#temporal-formulation-and-explicit-quantum-fdtd-placement).
2. **Solver choice:** Follow a specified Maxwell/Yee-type Schrödingerisation construction with its source, boundary, and time-update treatment [19–21].
3. **Execution fit:** Use the cited IonQ QPU example to assess hardware fit; assign geometry, field preparation, source and boundary handling, and signed-field recovery to the specified hybrid workflow.
4. **Readiness check:** Consult the Maxwell row in the [readiness table](#readiness-interpretation). The cited hardware preprint reports two-dimensional QPU benchmarks and three-dimensional simulator benchmarks [21].
5. **Cost and validation:** Define whether the output is a selected field functional or a reconstructed mesh, then include loading, stability and propagation errors, measurement, and classical processing in [complete-workflow resource accounting](#resource-accounting).

### Plan an electron-tomography reconstruction subproblem

1. **Goal and route:** Refine selected structural parameters or volume information, then follow [Branch XI](#branch-xi) and the shared inverse primitives in [Branch XII](#branch-xii).
2. **Solver choice:** The cited starting route is regional QUBO refinement for CT. An electron-tomography extension requires an electron-specific forward model and an explicit inverse or optimization subproblem [23, 24].
3. **Execution fit:** The CT example uses D-Wave hybrid optimization with classical regional assembly; the electron-target workflow also includes alignment, scattering, priors, and output recovery.
4. **Readiness check:** Consult the tomography row in the [readiness table](#readiness-interpretation): HW identifies the regional CT result, while R identifies the proposed electron-target extension [23].
5. **Cost and validation:** Include discretization, penalties and connectivity, classical reconstruction, full-output recovery, and matched image accuracy in [complete-workflow resource accounting](#resource-accounting).

## How to use this page

1. **Choose a route:** Use the [solver selection router](#quick-start-solver-selection-router) to match your requested output to a branch, or follow an [example decision journey](#example-decision-journeys).
2. **Check execution readiness:** Compare the cited model, evidence, and hardware in [readiness and validation](#readiness-and-validation) with your target and accuracy.
3. **Build the workflow:** Open the relevant [technical deep-dive](#technical-deep-dives), use [implementation workflows](#implementation-workflows) to assign quantum and classical tasks, and consult [Branch XII](#branch-xii) for complete cost and validation requirements.

## Table of contents

- [Quick start: solver selection router](#quick-start-solver-selection-router)
- [Example decision journeys](#example-decision-journeys)
  - [Molecular electronic ground-state energy](#predict-a-molecular-electronic-ground-state-energy)
  - [Magnetic inelastic neutron-scattering response](#predict-a-magnetic-inelastic-neutron-scattering-response)
  - [Transient Maxwell field](#simulate-a-transient-maxwell-field)
  - [Electron-tomography reconstruction subproblem](#plan-an-electron-tomography-reconstruction-subproblem)
- [How to use this page](#how-to-use-this-page)
- [Readiness and validation](#readiness-and-validation)
  - [Readiness interpretation](#readiness-interpretation)
  - [Hardware–solver fit matrix](#hardware-solver-fit-matrix)
- [Solver selection map](#solver-selection-map)
  - [Overview tree](#overview-tree)
  - [Temporal formulation and explicit quantum FDTD placement](#temporal-formulation-and-explicit-quantum-fdtd-placement)
  - [Technical deep-dives: branches I–XII](#technical-deep-dives)
    - [I. Electronic eigenstates and material spectra](#branch-i)
    - [II. Hamiltonian evolution and scattering](#branch-ii)
    - [III. Interferometry, response, and spectroscopy](#branch-iii)
    - [IV. Open systems, baths, and transport](#branch-iv)
    - [V. PDE, finite-element, and finite-volume solvers](#branch-v)
    - [VI. Lattice kinetics, fluid dynamics, and nonlinear equations](#branch-vi)
    - [VII. Analog and digital–analog quantum simulation](#branch-vii)
    - [VIII. Embedded, atomistic, and multiscale simulation](#branch-viii)
    - [IX. Thermal states, sampling, and Monte Carlo](#branch-ix)
    - [X. Supporting hardware and coprocessing](#branch-x)
    - [XI. Characterization, reconstruction, and inverse design](#branch-xi)
      - [Forward prediction, inverse reconstruction, and measurement interpretation](#forward-prediction-inverse-reconstruction-and-measurement-interpretation)
    - [XII. Shared computational overheads and classical interfaces](#branch-xii)
      - [Complete-workflow resource accounting](#resource-accounting)
- [Implementation workflows](#implementation-workflows)
  - [Application areas and concrete examples](#application-areas-and-concrete-examples)
    - [Compact application tree](#application-tree-compact-version)
    - [Extended application tree](#application-tree-extended-version)
  - [Application-to-solver mapping](#application-to-solver-mapping)
  - [Hybrid execution across application branches](#hybrid-execution-across-application-branches)
- [Technical reference](#technical-reference)
  - [Primer and execution legend](#primer-and-execution-legend)
  - [Evidence legend](#evidence-legend)
  - [Acronym glossary](#acronym-glossary)
  - [Physical scale and predicted-output connections](#physical-scale-and-predicted-output-connections)
  - [Direct and indirect connections](#direct-and-indirect-connections)
- [References](#references)
- [Document version](#document-version)

## Readiness and validation

### Readiness interpretation

| Family | Evidence or implementation level | Scaling interpretation |
| --- | --- | --- |
| VQE and related variational methods | Small hardware demonstrations and extensive algorithm studies | Optimization, ansatz error, conditioning, and measurements limit larger targets. |
| SQD and selected-space methods | Hardware electronic-structure examples with substantial classical computation [1, 2] | Classical subspace size and coverage of important configurations govern convergence and resource cost. |
| Dynamical structure factors and INS | Hardware model studies; the 2026 KCuF3 preprint reports up to 50 qubits [3, 4] | Time window, depth, fidelity, and model mismatch govern spectral accuracy. |
| QLSA/QSVT/QPE | Established algorithmic primitives with small demonstrations for selected constructions | Large scientific targets generally motivate error-corrected execution and explicit coherent access. |
| Quantum FEM | Specific preconditioned elliptic construction includes hardware experiments [5] | The demonstrated scope is a specified preconditioned elliptic construction; larger targets require their own operator-access, conditioning, output, and resource analyses. |
| Quantum time-domain Maxwell finite differences | Specific numerical constructions and a 2026 hardware preprint with two-dimensional IonQ benchmarks [19–21] | Arbitrary three-dimensional structures, full-field readout, lossy boundaries, and end-to-end advantage require separate assessment. |
| QLBM and quantum FVM | Formulation-dependent theory, numerical benchmarks, and small prototypes | Record grid size, execution platform, accuracy, and complete runtime for each benchmark; assess practical advantage against a matched classical workflow. |
| Analog quantum models | Experimental programmable simulation of selected interactions | Model matching, control range, observable access, and calibration are decisive. |
| QCPMD, general multiscale coupling, and oscillator–qubit LCHS | Research constructions with different evidence levels [13, 14, 17] | Each proposed target needs its own accuracy and total-resource assessment. |
| QPU plus classical analog numerical coprocessor | Proposed placement in this guide | The proposed coupled workflow requires complete accuracy and runtime benchmarks, including calibration, conversion, and residual correction. |
| Quantum-assisted tomographic reconstruction | Small annealing studies and regional D-Wave hybrid refinement for discrete CT examples [23] | Electron-tomography compatibility is a proposed extension; quantify classical work, discretization, total runtime, and matched image accuracy. |
| Quantum-assisted 4D-STEM ptychographic reconstruction | Target-specific research placement; established classical electron ptychographic tomography supplies a baseline [24, 25] | Data loading, nonlinear updates, coherent operator access, and full complex-image recovery require a complete construction. |
| ARPES spectral-function calculation | A 2026 preprint reports a 54-qubit hardware calculation for a 27-site model [26] | Real-material predictions require suitable model size, state preparation, sampling accuracy, and photoemission/instrument components. |
| Core-level EELS quantum simulation | Algorithm and resource estimates for an 18-active-orbital battery-material cluster: approximately 100 logical qubits, circuit depth reported as 3.25 × 10^8 T gates, and roughly 10^4 shots [28] | Logical qubits and fault-tolerant circuit estimates differ from available physical-qubit counts and hardware demonstrations. |
| NMR spectral simulation and Hamiltonian learning | Four-qubit trapped-ion zero-field NMR demonstration; separate algorithms for learning spin interactions [29, 30] | Larger spin networks require adequate evolution fidelity, ensemble handling, sampling, and parameter identifiability. |
| Generalized computational spectroscopy | Ancilla-assisted autocorrelation spectroscopy demonstrated on a programmable silicon-photonic processor [31] | Target Hamiltonians, state preparation, controlled dynamics, and observable access determine applicability. |
| EUV photoresist microscopic spectroscopy | A 2026 preprint, revised August 2026, estimates about 200 logical qubits, 10^9 non-Clifford gates per absorption circuit, and 10^3 shots; its photoemission model needs several thousand logical qubits, at least 10^14 gates, and 10^4 shots [35] | These fault-tolerant estimates specify resources for microscopic spectra within a multiscale resist workflow; full-chip inverse lithography additionally requires field, transport, chemistry, and process coupling. |
| Computational ellipsometry, polarimetry, and magnetometry extensions | Candidate microscopic-response or reduced inverse subproblems; quantum ellipsometric sensing has a separate theory [32] | QPU acceleration requires an explicit microscopic-response or inverse subproblem and a matched complete performance benchmark. |
| Quantum-assisted mass-spectrum prediction and identification | Established classical fragmentation baselines and a published quantum identification proposal [33, 34] | Electronic inputs, fragmentation dynamics, and database-free identification require separate accuracy and computational comparisons. |
| Quantum-assisted mask or acquisition optimization | Application-specific research extensions | Explicit objectives, mappings, classical baselines, uncertainty, and complete resource costs determine readiness. |

<a id="hardware-solver-fit-matrix"></a>

### Hardware–solver fit matrix

The lookup connects hardware examples and execution classes to their stated task and evidence scope. Application mappings and complete resource requirements are linked in the corresponding branches.

| Platform or execution class | Solver branches | Readiness/ cited scope | Key model or execution requirement |
| --- | --- | --- | --- |
| Quantinuum H2 | [I](#branch-i), [III](#branch-iii), [XI](#branch-xi) | HW preprint: 27-site spectral-function model using 54 qubits [26] | Material-specific Hamiltonian, state preparation, sampling accuracy, and photoemission/instrument model |
| IonQ QPU | [V](#branch-v) | HW 2026 preprint: 2D time-domain Maxwell benchmarks; 3D benchmarks use a simulator [21] | Specified finite-difference mapping, source/boundary treatment, stability, and signed-field recovery |
| D-Wave hybrid solver | [XI](#branch-xi), [XII](#branch-xii) | HW regional QUBO refinement of CT phantoms and a chest slice [23] | Discrete objective, penalties/connectivity, classical regional assembly, and matched image accuracy; electron targets remain R |
| Trapped-ion processor (four-qubit NMR example) | [III](#branch-iii), [XI](#branch-xi) | HW small zero-field NMR demonstration [29] | Spin-network size, evolution fidelity, ensemble handling, and sampling |
| Programmable silicon-photonic processor | [III](#branch-iii), [XI](#branch-xi) | HW ancilla-assisted autocorrelation spectroscopy [31] | Compatible target Hamiltonian, state preparation, controlled dynamics, and observable access |
| Gate-based QPU + CPU/GPU | [I](#branch-i), [VIII](#branch-viii), [IX](#branch-ix), [X](#branch-x) | HW selected-space electronic examples; HW-input/NUM AFQMC interface [1, 2, 16] | Classical subspace size, configuration coverage, embedding error, and total classical work |
| Native analog or mixed digital–analog quantum platforms | [II](#branch-ii), [IV](#branch-iv), [VII](#branch-vii) | Experimental programmable simulation of selected interactions | Match available interactions, geometry, controls, observable access, and calibration to the target model |
| Fault-tolerant logical-qubit execution | [III](#branch-iii), [XI](#branch-xi) | ALG EELS and EUV microscopic-spectrum resource estimates [28, 35] | Logical qubits, non-Clifford gates, and shots are specified in the readiness table |
| QPU + classical analog coprocessor | [X](#branch-x), [XII](#branch-xii) | R proposed combined numerical workflow | Conversion, noise, drift, residual correction, precision, and complete processing-time comparison |

[Back to solver router](#quick-start-solver-selection-router) · [Back to contents](#table-of-contents)

<a id="solver-taxonomy"></a>

## Solver selection map

### Overview tree

<details markdown="1">
<summary><strong>Explore solver families and requested outputs</strong></summary>

The selection map connects application branches to shared execution support. Open the technical deep-dives below for methods and execution roles.

```text
QPU and hybrid solver selection map
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
├── Computational characterization, reconstruction, and inverse design
│   ├── Inverse reconstruction [Q/H/QA; R for target-specific extensions]
│   │   ├── Electron tomography, 4D-STEM ptychography, and field-map inversion
│   │   └── Linearized subproblems, constrained optimization, and selected parameter inference
│   ├── Forward signal prediction [Q/H/AQ]
│   │   ├── ARPES, EELS, NMR, interferometric amplitudes, and microscopic response
│   │   └── Quantum correlations combined with classical sample and instrument models
│   └── Experimental and process design [H; R outside specific constructions]
│       ├── EUV resist spectroscopy, mask design, probes, and acquisition settings
│       └── Selected objectives with explicit output, uncertainty, and resource accounting
└── Shared execution support
    ├── Hardware and coprocessing (X)
    │   └── CPU/GPU stages, ASIC/FPGA support, and classical analog operations
    └── Computational overheads and classical interfaces (XII)
        └── Data access, regularization, reconstruction, readout, and complete-workflow validation
```

</details>

### Temporal formulation and explicit quantum FDTD placement

<details markdown="1">
<summary><strong>Explore temporal formulations and quantum FDTD examples</strong></summary>

The temporal formulation constitutes an additional classification axis, which remains independent of both the spatial discretization and the execution hardware. For instance, a finite-element model may employ either transient or frequency-domain formulations. Similarly, the Finite-Difference Time-Domain (FDTD) method utilizes a combined finite-difference approach for both spatial and temporal discretization. When the quantum evolution of a spatial finite-difference operator is implemented via a specified propagator, the resulting time-update construction determines the relationship between the model and conventional FDTD time-marching schemes.

| Temporal formulation | Mathematical task | Representative quantum routes | Interpretation |
| --- | --- | --- | --- |
| Stationary or steady-state | Solve a time-independent field equation, eigenproblem, or stationary channel | QLSA, VQLS, eigensolvers, dissipative preparation | Static fields, eigenstates, and open-system steady states are different mathematical tasks. |
| Frequency-domain or time-harmonic | Solve a driven system at a specified frequency | Shifted linear systems, resolvents, QLSA, VQLS | Finite-difference frequency-domain and FEM frequency-domain models have frequency-dependent operators and boundary data. |
| Real-time transient | Propagate an initial state or field, with optional time-dependent sources | Hamiltonian simulation, VQS, Schrödingerisation, LCHS, ODE algorithms | Covers pulses, wavepackets, relaxation, and transient transport; a valid FDTD mapping belongs here. |
| Periodically driven or Floquet | Analyze a periodic generator or one-period propagator | Time-dependent simulation, spectral estimation of the period propagator | Period-propagator and Floquet analyses describe evolution across drive cycles. |
| Imaginary-time or thermal preparation | Apply normalized exponential filters or prepare ensembles | QITE, variational imaginary time, filtering, purification | Imaginary time parameterizes exponential filtering for state preparation or inference. |

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

Within this framework, the transient branch provides cross-references to [branch V](#branch-v) (pertaining to numerical PDEs) and [branch II](#branch-ii) (regarding physical Hamiltonian evolution). It is important to note that an electronic wavefunction and a classical electromagnetic field encoded as quantum amplitudes maintain distinct physical interpretations, even when both are processed using unitary evolution algorithms.

| Example | What is implemented or proposed | Evidence and scope |
| --- | --- | --- |
| Jin, Liu, and Ma, Maxwell Schrödingerisation [19] | Yee-based Maxwell discretization and alternative spectral/upwind formulations mapped to unitary evolution; PEC, impedance, and interface treatments | Numerical study of the specified Maxwell formulations, including a continuous-variable quantum representation. |
| Ma et al., Maxwell circuits with time-dependent sources [20] | Explicit circuits for PEC boundaries and driven Maxwell evolution using Schrödingerisation and autonomization | Circuit construction and complexity analysis under the stated source, boundary, and operator-access assumptions. |
| Sharma et al., time-domain Maxwell hardware study [21] | Finite-difference operators, Schrödingerisation, Bell-basis Trotter blocks, and signed-field recovery | The 2026 preprint reports physical IonQ QPU benchmarks in two dimensions and simulator benchmarks in three dimensions. |
| Na and Chew, quantum electromagnetic FDTD [22] | Classical numerical propagation of quantized electromagnetic-field propagators, including a Hong–Ou–Mandel example | Classical numerical execution of a quantized electromagnetic-field model [C]. |
| Finite-difference electronic wavepacket evolution | Discretize the Schrödinger Hamiltonian, prepare a wavepacket, evolve it, and estimate selected observables | Hamiltonian simulation supplies quantum propagation; the FDTD designation requires an explicit finite-difference time-update construction. |
| Hybrid Maxwell–Schrödinger or Maxwell–Bloch dynamics | A classical field solver exchanges specified source or material-response information with a quantum subproblem | Target-specific hybrid construction requires defined coupling, measurement, and stability analyses. Classical execution of the coupled quantum-physics equations is classified C. |

A quantum FDTD construction incorporates coherent operator access, the loading of initial fields and sources, and the treatment of boundaries or absorbing layers. The corresponding error and resource analyses must address several critical factors (including constraint preservation, stability, discretization, and propagation), as well as the recovery of signed fields or observables. While conventional explicit FDTD is governed by the Courant–Friedrichs–Lewy stability bound, quantum propagators necessitate a dedicated stability analysis tailored to their specific evolution scheme. Furthermore, the output costs for full electromagnetic mesh reconstruction differ significantly from those of selected field measurements, whereas predictions for nanoscale Maxwell equations require the integration of an appropriate material-response model.

</details>

<a id="detailed-technique-tree"></a>

### Technical deep-dives

Choose a branch below and expand its methods, outputs, and execution roles. Branches V and XII also contain grouped technical subsections.

| Branch | Start here for |
| --- | --- |
| [Branch I](#branch-i) | Electronic ground states, excited states, and material spectra |
| [Branch II](#branch-ii) | Real-time Hamiltonian evolution and computational scattering |
| [Branch III](#branch-iii) | Computational interferometry, response functions, and spectroscopy |
| [Branch IV](#branch-iv) | Open systems, quantum baths, and transport |
| [Branch V](#branch-v) | Quantum numerical PDE solvers, finite elements, and finite volumes |
| [Branch VI](#branch-vi) | Quantum lattice kinetics, fluid dynamics, and nonlinear equations |
| [Branch VII](#branch-vii) | Programmable analog and mixed digital–analog quantum simulation |
| [Branch VIII](#branch-viii) | Embedded, atomistic, and multiscale hybrid simulation |
| [Branch IX](#branch-ix) | Thermal states, quantum sampling, and Monte Carlo interfaces |
| [Branch X](#branch-x) | Supporting hardware and coprocessing |
| [Branch XI](#branch-xi) | Computational characterization, reconstruction, and inverse design |
| [Branch XII](#branch-xii) | Shared computational overheads and classical interfaces |

<a id="branch-i"></a>

<details markdown="1">
<summary><strong>Branch I: Electronic ground states, excited states, and material spectra</strong></summary>

```text
I. Electronic ground states, excited states, and material spectra
├── Variational eigenstate solvers [H]
│   ├── VQE
│   │   └── the QPU estimates a parameterized state's energy; a CPU or GPU minimizes that estimate.
│   ├── ADAPT-VQE
│   │   └── measured operator gradients guide classical selection of ansatz operators.
│   └── VQD and state-specific variational methods
│       └── overlap penalties or state constraints target excited states, with sampling and optimization
│           overhead.
├── Quantum subspace solvers [Q/H]
│   ├── QSE, quantum Krylov, and quantum Lanczos
│   │   └── quantum preparation and matrix-element estimation build a projected problem; classical
│   │       generalized diagonalization returns approximate eigenvalues.
│   ├── QSCI and SQD
│   │   └── quantum configuration samples select classical configuration-interaction subspaces; the
│   │       eigenproblem is solved classically.
│   ├── SKQD and randomized propagation variants
│   │   └── evolved-state samples enrich the selected classical subspace; convergence depends on the state
│   │       distribution and subspace size.
│   └── Extended SQD
│       └── extensions enlarge or refine the sample-selected electronic problem, including embedded
│           fragments in the FLiBe study [1, 2].
├── Spectral and state-preparation routes [Q/H/AQ]
│   ├── QPE and iterative spectral estimation
│   │   └── coherent evolution resolves eigenenergies; initial-state overlap determines which eigenstates
│   │       are sampled.
│   ├── QSP/QSVT spectral filters
│   │   └── block-encoded functions suppress unwanted spectral components, with normalization and
│   │       state-preparation costs.
│   ├── QITE and variational imaginary time
│   │   └── quantum measurements determine state-dependent updates, usually accompanied by classical
│   │       coefficient solves.
│   └── Adiabatic preparation and engineered cooling
│       └── a controlled Hamiltonian path or dissipative process prepares an accessible low-energy state;
│           gaps and mixing govern cost.
└── Periodic-material extensions [H; model-specific]
    ├── Momentum-resolved models and periodic active spaces
    │   └── solve selected sectors or embedded periodic problems.
    ├── Charged excitation energies and Green-function spectra
    │   └── extract addition/removal energies or spectral poles for band gaps and quasiparticle dispersion.
    └── Scope
        └── band reconstruction requires momentum-resolved spectral information. Kohn–Sham eigenvalues
            describe the auxiliary one-electron problem; charged excitation energies and Green functions
            describe many-body addition/removal spectra.
```

</details>

<a id="branch-ii"></a>

<details markdown="1">
<summary><strong>Branch II: Real-time Hamiltonian evolution and computational scattering</strong></summary>

```text
II. Real-time Hamiltonian evolution and computational scattering
├── Digital Hamiltonian simulation [Q]
│   ├── Lie–Trotter, Strang, Suzuki, and qDRIFT formulas
│   │   └── approximate evolution using simpler accessible terms.
│   ├── LCU, truncated Taylor, and Dyson methods
│   │   └── combine controlled operations to simulate static or time-dependent dynamics.
│   ├── Qubitization and QSP
│   │   └── implement block-encoded Hamiltonian evolution; QSVT extends the matrix-function framework [8].
│   ├── Interaction-picture algorithms
│   │   └── treat an efficiently simulated dominant term separately from the remaining interactions.
│   └── Sparse-Hamiltonian and quantum-walk constructions
│       └── use explicit coherent access to nonzero entries or a suitable block encoding.
├── Variational quantum dynamics [H]
│   ├── Real-time VQS and adaptive variants
│   │   └── the QPU estimates projected equations of motion; classical linear algebra updates circuit
│   │       parameters.
│   └── Nonadiabatic multi-state dynamics
│       └── couple electronic-state amplitudes and nuclear degrees of freedom using a specified ansatz and
│           propagation rule.
├── Spatial and occupation encodings [Q/AQ]
│   ├── First quantization
│   │   └── spatial grids, plane waves, or orbital bases represent particle coordinates.
│   ├── Second quantization
│   │   └── orbital occupation registers use Jordan–Wigner, Bravyi–Kitaev, or another justified fermionic
│   │       encoding.
│   ├── Split-operator evolution
│   │   └── quantum Fourier transformations exchange position and momentum representations when the
│   │       discretization permits efficient kinetic and potential steps.
│   └── Qudit and bosonic encodings
│       └── truncated occupations represent phonons, photons, magnons, or vibrational modes.
└── Scattering outputs [Q/H]
    ├── Channel-resolved wavepacket propagation
    │   └── evolving prepared incoming states and measuring outgoing channels yields probabilities or
    │       transition amplitudes.
    ├── Interferometric amplitude estimation
    │   └── coherent reference branches resolve complex overlaps and relative phases [9].
    └── Phase shifts, cross sections, and rates
        └── classical analysis supplies channel normalization, flux factors, and spectral reconstruction.
```

</details>

<a id="branch-iii"></a>

<details markdown="1">
<summary><strong>Branch III: Computational interferometry, response functions, and spectroscopy</strong></summary>

```text
III. Computational interferometry, response functions, and spectroscopy
├── Complex overlaps and phase-sensitive observables [Q/H]
│   ├── Hadamard tests
│   │   └── ancilla measurements estimate the real or imaginary component of a controlled-unitary
│   │       expectation.
│   ├── Reference-state interference and related overlap protocols
│   │   └── reconstruct selected transition amplitudes when the required state preparation is available.
│   ├── SWAP and fidelity tests
│   │   └── estimate squared overlap magnitudes. Complex phases require a phase-sensitive reference
│   │       protocol.
│   └── Loschmidt amplitudes and echo probabilities
│       └── distinguish phase-sensitive return amplitudes from their squared magnitudes.
├── Correlation functions and spectra [Q/H/AQ]
│   ├── Two-time spin, density, and current correlators
│   │   └── quantum evolution and measurement supply time-dependent response data.
│   ├── Dynamical structure factors
│   │   └── classical spatial and temporal transforms of measured spin correlations produce momentum- and
│   │       frequency-resolved spectra.
│   ├── Kubo response
│   │   └── equilibrium correlators determine specified linear-response coefficients with thermal-state
│   │       preparation and integration errors.
│   └── Green functions and shifted resolvents
│       └── evaluate selected matrix elements, including scale recovery for normalized quantum solution
│           states.
├── Inelastic neutron scattering prediction [Q/H/AQ]
│   ├── Spin Hamiltonian preparation and evolution
│   │   └── represent the relevant magnet or molecular nanomagnet.
│   ├── Spectral reconstruction
│   │   └── obtain the magnetic dynamical structure factor and apply polarization, magnetic form factors,
│   │       geometry, and instrument resolution.
│   └── Interpretation
│       └── the QPU simulates material response; instrument-specific modeling converts the response into
│           predicted neutron-scattering signals. Hardware examples include molecular spin models and the
│           KCuF3 comparison [3, 4].
└── Molecular and solid-state spectroscopies [Q/H/AQ]
    ├── Electronic transitions
    │   └── excitation energies plus dipole or other transition matrix elements yield selected spectral
    │       intensities.
    ├── Vibrational and vibronic spectra
    │   └── bosonic encodings or native oscillator systems represent nuclear modes and electron–vibration
    │       coupling.
    └── OTOCs and contour-ordered observables
        └── specified operator orderings and circuit constructions define the requested out-of-time-order or
            contour-ordered correlation function.
```

</details>

<a id="branch-iv"></a>

<details markdown="1">
<summary><strong>Branch IV: Open systems, quantum baths, and transport</strong></summary>

```text
IV. Open systems, quantum baths, and transport
├── Channel and Lindblad evolution [Q/H/AQ]
│   ├── Stinespring dilation and Kraus circuits
│   │   └── auxiliary registers represent a specified completely positive channel.
│   ├── Collision models, resets, and engineered dissipation
│   │   └── repeated system–environment interactions implement a target open-system process.
│   └── Lindblad product formulas, LCU, or block encodings
│       └── simulate Markovian generators under their construction-specific assumptions.
├── Trajectory and memory methods [Q/H/AQ]
│   ├── Quantum jumps and diffusive trajectories
│   │   └── quantum evolution generates stochastic records; classical averaging estimates ensemble
│   │       observables.
│   ├── Explicit baths, pseudomodes, and reaction coordinates
│   │   └── additional quantum degrees of freedom represent selected non-Markovian memory.
│   └── Process-tensor workflows
│       └── specified memory representations require storage and circuit-cost accounting.
└── Stationary states and electronic transport [Q/H; R for unresolved targets]
    ├── Dissipative or variational steady-state preparation
    │   └── assess uniqueness, relaxation, and physical-state validity.
    ├── Trace-constrained Liouvillian solves
    │   └── handle singularity and non-Hermitian structure explicitly. Physical density-matrix preparation
    │       requires trace normalization, physical-state validation, and a compatible preparation protocol.
    ├── QPU-assisted Green functions or impurity self-energies
    │   └── feed measured quantum quantities into classical NEGF or transport calculations.
    └── Scope
        └── CPU/GPU execution of NEGF, Landauer transport, or Lindblad evolution is classified C;
            QPU-derived inputs define the quantum stage of a hybrid workflow.
```

</details>

<a id="branch-v"></a>

<details markdown="1">
<summary><strong>Branch V: Quantum numerical PDE solvers, finite elements, and finite volumes</strong></summary>

Select a mathematical primitive, then its spatial discretization and temporal formulation. Shared loading, measurement, and validation requirements are in [branch XII](#branch-xii).

<details markdown="1">
<summary>Linear-system primitives [Q/H]</summary>

```text
Linear-system primitives [Q/H]
├── HHL and subsequent QLSAs
│   └── prepare a state proportional to an inverse action under spectral and access assumptions [7].
├── QSVT inverse or pseudoinverse filters
│   └── transform a block-encoded matrix, with conditioning and support requirements [8].
├── VQLS
│   └── quantum residual or cost estimates drive classical optimization of a normalized solution state
│       [6].
└── Quantum projected/Krylov approaches
    └── estimate subspace operators, then solve the reduced algebraic problem classically.
```

</details>

<details markdown="1">
<summary>Technical deep dive: operator access, discretization, and preconditioning</summary>

| Existing construction | Operator or data mapping | Implementation requirement | Branch |
| --- | --- | --- | --- |
| Sparse-Hamiltonian and quantum-walk constructions | Nonzero matrix entries or a suitable block encoding | Explicit coherent access | [II](#branch-ii) |
| Finite-difference and spectral methods | Stencil or operator access | Include coherent-access implementation cost | [V](#branch-v) |
| Quantum FEM | Stiffness or generalized eigenvalue matrices, load vectors, and boundaries | Map to the chosen quantum primitive | [V](#branch-v) |
| Quantum FVM | Conservative cell balances and flux equations | Specify the quantum linear or nonlinear workflow [11] | [V](#branch-v) |
| Preconditioning | BPX, domain decomposition, or another justified preconditioner | Include construction and application costs [5] | [V](#branch-v) |

```text
Discretization-specific constructions [Q/H]
├── Quantum FEM
│   └── map stiffness or generalized eigenvalue matrices, load vectors, and boundary conditions to the
│       chosen primitive.
├── Quantum FVM
│   └── map conservative cell balances and flux equations to a specified quantum linear or nonlinear
│       workflow [11].
├── Finite-difference and spectral methods
│   └── construct coherent stencil or operator access and include its implementation cost.
└── Preconditioning
    └── encode BPX, domain-decomposition, or another justified preconditioner and include its
        construction and application costs [5].
```

</details>

<details markdown="1">
<summary>Time-dependent and dissipative numerical evolution [Q/H/AQ]</summary>

```text
Time-dependent and dissipative numerical evolution [Q/H/AQ]
├── Quantum finite-difference time-domain and related evolution formulations
│   └── implement specified Maxwell/Yee-type mappings, driven-source circuits, or spatial
│       finite-difference Hamiltonian evolution; identify the time-update rule for each construction
│       [19–21].
├── FDTD-related applications
│   └── electromagnetic pulse propagation, specified dielectric-interface scattering, quantum
│       wavepackets, and proposed coupled light-matter workflows. Physical sources, losses, and
│       absorbing boundaries require construction-specific handling.
├── Linear ODE embeddings
│   └── reformulate time discretization as a linear system or use a dedicated evolution algorithm.
├── Schrödingerisation
│   └── enlarge the representation so suitable linear dynamics is recovered from unitary evolution [12].
├── LCHS
│   └── represent suitable nonunitary evolution through weighted Hamiltonian simulations.
└── Hybrid oscillator–qubit LCHS
    └── an ancillary quantum oscillator represents the kernel in a coupled oscillator–qubit quantum
        architecture [13].
```

</details>

<details markdown="1">
<summary>Application scope [model-specific]</summary>

```text
Application scope [model-specific]
├── Poisson, Helmholtz, Maxwell, elasticity, diffusion, and reaction–transport problems
│   └── boundary treatment, stability, physical constraints, and requested outputs determine
│       eligibility.
├── Nano applicability
│   └── continuum constitutive assumptions and included physical mechanisms determine the model's
│       nanoscale validity.
└── Output scope
    └── selected-function estimation and full-mesh reconstruction require separate measurement and
        readout budgets.
```

</details>

</details>

<a id="branch-vi"></a>

<details markdown="1">
<summary><strong>Branch VI: Quantum lattice kinetics, fluid dynamics, and nonlinear equations</strong></summary>

```text
VI. Quantum lattice kinetics, fluid dynamics, and nonlinear equations
├── QLBM [Q/H; AQ only for an explicit native mapping]
│   ├── Reversible streaming
│   │   └── encoded populations are moved between lattice sites.
│   ├── Collision implementations
│   │   └── dilation, measurement, feedback, or a specified ensemble construction represents relaxation or
│   │       collision.
│   ├── Carleman formulations
│   │   └── lifted polynomial dynamics requires a finite truncation and convergence bounds.
│   └── Node-level ensemble formulations
│       └── enlarge the local description to implement a specified nonlinear fluid model [10].
├── QLGA, quantum walks, and QCA [Q/AQ]
│   ├── Quantum lattice gas automata
│   │   └── local occupation registers undergo collisions and streaming.
│   ├── Coined, split-step, and continuous-time walks
│   │   └── simulate controlled transport or wave propagation with model-specific continuum limits.
│   └── Quantum cellular automata
│       └── local updates represent specified reversible lattice dynamics.
├── Extensions to kinetic or nonlinear models [Q/H; R outside established constructions]
│   ├── Carleman, Koopman, and Liouville representations
│   │   └── linearize a lifted description while retaining truncation and conditioning costs.
│   ├── Wigner or Boltzmann kinetics
│   │   └── specify phase-space encoding, signed quasiprobabilities, scattering kernels, and observables.
│   ├── Nonlinear Schrödinger and Gross–Pitaevskii equations
│   │   └── require feedback, a justified lifting, or an explicit many-body mean-field limit.
│   └── Stochastic and uncertainty workflows
│       └── quantum expectation estimation requires a coherently implementable model or sampling oracle.
└── Physical scale distinction
    ├── Fluid QLBM
    │   └── describes mesoscopic kinetic or hydrodynamic populations under the selected lattice closure.
    ├── Confined nanofluids
    │   └── boundary scattering, Knudsen effects, molecular structure, and quantum statistics determine
    │       whether the selected fluid closure remains valid.
    └── Interpretation
        └── classical circuit simulation provides numerical validation; processor execution provides
            physical-QPU evidence.
```

</details>

<a id="branch-vii"></a>

<details markdown="1">
<summary><strong>Branch VII: Programmable analog and mixed digital–analog quantum simulation</strong></summary>

```text
VII. Programmable analog and mixed digital–analog quantum simulation
├── Spin and fermionic lattice models [AQ/Q]
│   ├── Native Ising, XY, and accessible Heisenberg interactions
│   │   └── simulate selected quantum magnetic Hamiltonians.
│   ├── Optical-lattice Bose–Hubbard and Fermi–Hubbard models
│   │   └── represent interacting lattice particles subject to experimental controls.
│   ├── Rydberg, ion, and superconducting analog platforms
│   │   └── match available interactions, geometry, and observables to the target model.
│   └── Digital–analog sequences
│       └── interleave native interaction blocks with gates, including approximation and calibration errors.
├── Bosonic and oscillator models [AQ/Q]
│   ├── Gaussian mode transformations
│   │   └── simulate quadratic oscillator dynamics.
│   ├── Anharmonic or interacting mode dynamics
│   │   └── require suitable non-Gaussian operations or encoded digital interactions.
│   └── Spin–boson and vibronic models
│       └── couple quantum spins or electronic states to phonon, photon, or molecular vibrational modes.
└── Annealing and gauge-model extensions [AQ/Q; model-specific]
    ├── Annealing
    │   └── searches low-energy states of accessible Hamiltonians; chemistry requires an explicit useful
    │       mapping.
    ├── Truncated gauge fields and quantum links
    │   └── require constraint preservation and truncation-error assessment.
    └── Scope
        └── native quantum interactions implement the target spin, particle, or oscillator Hamiltonian;
            classical analog numerical operations are classified CA.
```

</details>

<a id="branch-viii"></a>

<details markdown="1">
<summary><strong>Branch VIII: Embedded, atomistic, and multiscale hybrid simulation</strong></summary>

```text
VIII. Embedded, atomistic, and multiscale hybrid simulation
├── Quantum impurity and fragment solvers [H]
│   ├── DMFT
│   │   └── QPU impurity observables or Green functions enter a classical dynamical mean-field
│   │       self-consistency loop.
│   ├── DMET
│   │   └── a quantum fragment solver supplies correlated information to a classical density-matrix
│   │       embedding loop.
│   ├── Active-space and EWF methods
│   │   └── classically construct orbitals and fragments; solve selected correlated electronic problems
│   │       using VQE, SQD, QPE, or another compatible method.
│   ├── QM/MM
│   │   └── the quantum-mechanical region has an explicit QPU subproblem, while classical molecular
│   │       mechanics represents the environment.
│   └── DFT interfaces
│       └── a QPU supplies a specified correlated correction or impurity solution within the classical DFT
│           workflow.
├── Nuclear trajectories and reactions [H]
│   ├── QPU-assisted Born–Oppenheimer MD
│   │   └── quantum electronic estimates and force information drive classical nuclear integration.
│   ├── QCPMD
│   │   └── coupled equations update electronic circuit parameters and classical nuclear coordinates within
│   │       the same integration loop [14].
│   ├── Nonadiabatic dynamics
│   │   └── represent electronic transitions and nuclear coupling explicitly. Nuclear quantum effects
│   │       require a quantum nuclear representation or a justified correction to classical trajectories.
│   └── Force recovery
│       └── include derivative operators, basis-response terms when required, and sampling error in
│           trajectory stability.
└── Fine-to-coarse coupling [H; R for unconstructed extensions]
    ├── Quantum numerical homogenization
    │   └── selected fine-scale quantum solution functionals modify a classical coarse PDE construction
    │       [17].
    ├── Domain decomposition
    │   └── communicate explicitly defined interface data with convergence and reconstruction costs.
    ├── Quantum-derived material parameters
    │   └── electronic or spin observables supply exchange, response, or constitutive inputs to classical
    │       continuum models.
    └── Micromagnetic interfaces
        └── QPU-derived spin information parameterizes classical LLG dynamics through a justified
            microscopic-to-continuum reduction.
```

</details>

<a id="branch-ix"></a>

<details markdown="1">
<summary><strong>Branch IX: Thermal states, quantum sampling, and Monte Carlo interfaces</strong></summary>

```text
IX. Thermal states, quantum sampling, and Monte Carlo interfaces
├── Quantum thermal preparation [Q/H/AQ]
│   ├── TFD and purification
│   │   └── enlarge the quantum system so tracing out an auxiliary register yields the intended thermal
│   │       state.
│   ├── VQT and product-spectrum ansätze
│   │   └── minimize free-energy objectives with an explicit entropy evaluation or tractable entropy model.
│   ├── QITE, QMETTS, and TPQ routes
│   │   └── prepare approximate thermal states or quantum thermal ensembles with preparation and averaging
│   │       costs.
│   └── Quantum Metropolis and dissipative Gibbs sampling
│       └── energy resolution, detailed balance, and mixing determine validity and cost.
├── Quantum expectation estimation [Q]
│   ├── QAE and iterative variants
│   │   └── estimate expectations through coherent preparation and amplification; include oracle
│   │       implementation and total execution time in performance comparisons [15].
│   └── Quantum walks for sampling
│       └── improvements depend on accessible transitions and spectral gaps.
└── QPU-assisted classical Monte Carlo [H]
    ├── SQD/QSCI trial states for phaseless AFQMC
    │   └── quantum-generated information enters a classical stochastic solver [16].
    ├── Quantum-generated proposals
    │   └── acceptance and stationary-distribution checks are essential.
    └── Scope
        └── VMC, DMC, PIMC, AFQMC, and tensor-network calculations use classical representations [C]. A
            specified QPU stage defines a hybrid workflow; fixed-node, constrained-path, and phaseless
            approximations require their own bias assessments.
```

</details>

<a id="branch-x"></a>

<details markdown="1">
<summary><strong>Branch X: Supporting hardware and coprocessing</strong></summary>

```text
X. Supporting hardware and coprocessing
├── CPU/GPU mathematical coprocessing [C within H]
│   ├── Before QPU execution
│   │   └── basis construction, molecular integrals, active spaces, meshes, preprocessing, and circuit
│   │       compilation.
│   ├── Inside hybrid loops
│   │   └── optimization, reduced diagonalization, configuration recovery, self-consistency, trajectories,
│   │       and classical sparse solves.
│   └── After QPU execution
│       └── spectra, Fourier transforms, uncertainty estimates, instrument models, and parameter inference.
├── ASIC/FPGA control and acceleration [C]
│   ├── Control/readout
│   │   └── waveform generation, sequencing, digitization, discrimination, reset, and feedforward support
│   │       QPU execution directly.
│   ├── Error processing
│   │   └── a specified ASIC or FPGA decoder processes error syndromes for the error-corrected solver
│   │       implementation.
│   ├── Numerical acceleration
│   │   └── a dedicated classical accelerator performs an explicit matrix, sampling, or reduction task when
│   │       the design supports it.
│   └── Scope
│       └── ASICs implement assigned control, decoding, or numerical kernels within the chosen eigensolver
│           or PDE workflow.
└── Classical analog coprocessing [CA; R for the proposed combined target]
    ├── Analog matrix-vector operations
    │   └── proposed electronic or photonic accelerators support an explicit classical linear-algebra stage.
    └── Classical oscillator or field networks
        └── a calibrated physical model supplies a classical reduced response.
```

Shared data-access, error-budget, and complete-workflow costs are in [branch XII](#branch-xii).

</details>

<a id="branch-xi"></a>

<details markdown="1">
<summary><strong>Branch XI: Computational characterization, reconstruction, and inverse design</strong></summary>

For the selected application, use [branch XII](#branch-xii) for inverse-problem primitives, regularization, data loading, classical reconstruction loops, and output costs.

```text
XI. Computational characterization, reconstruction, and inverse design
├── Electron tomography and atomic electron tomography [H; R for unvalidated electron targets]
│   ├── Projection-based reconstruction
│   │   └── Under a justified projection approximation, discretized line-integral data define an inverse
│   │       problem. Multiple scattering or nonlinear image formation requires a different forward model.
│   ├── Regional annealing refinement
│   │   └── A 2026 study validates compact regional QUBO updates with a D-Wave hybrid solver on CT phantoms
│   │       and a chest slice. Electron-tomography compatibility is a proposed extension requiring an
│   │       electron-specific forward model and benchmark [23].
│   └── Atomic-coordinate and potential refinement
│       └── Proposed quantum subproblems refine a reduced structural description or selected potential
│           coefficients.
├── 4D-STEM ptychography and ptychographic electron tomography [H/R]
│   ├── Complex object and probe reconstruction
│   │   └── Overlapping diffraction intensities constrain complex transmission, probe parameters, positions,
│   │       and coherence. Multislice formulations describe thicker specimens; the complete reconstruction
│   │       is generally nonlinear [24].
│   ├── Candidate QPU subproblems
│   │   └── Quantum propagation, inverse action on a linearized update, or reduced parameter estimation
│   │       requires a target-specific construction, benchmarked against classical CPU/GPU multislice
│   │       propagation.
│   └── Encoding and naming
│       └── 4D-STEM comprises two scan-position and two diffraction coordinates. Quantum-state ptychography
│           reconstructs an unknown quantum state; electron-image reconstruction requires a sample–probe
│           forward model and an explicit computational construction [25].
├── ARPES, EELS, and other electronic spectroscopies [Q/H]
│   ├── ARPES spectral prediction
│   │   └── Quantum electronic dynamics supplies selected removal spectra or spectral functions. Classical
│   │       photoemission matrix elements, occupations, surface/final-state effects, and resolution map
│   │       these quantities to measured intensities [26, 27].
│   ├── ARPES hardware scope
│   │   └── A 2026 preprint reports a 27-site spectral-function calculation using 54 Quantinuum H2 qubits.
│   │       Realistic-material ARPES prediction additionally requires a material-specific Hamiltonian and
│   │       photoemission/instrument modeling [26].
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
│   │   └── A quantum simulator supplies predictions and specified derivatives for learning nuclear-spin
│   │       interactions [30].
│   └── Ensemble and pulse models
│       └── Classical pulse descriptions, orientation averaging, calibration, and fitting connect the
│           simulated spin model to a measurement; electronic-structure-derived chemical shifts supply
│           additional model inputs when required.
├── Computational interferometry and coherent spectroscopy [Q/H]
│   ├── Phase-sensitive amplitudes
│   │   └── Hadamard tests or other explicit reference protocols estimate complex overlaps and
│   │       autocorrelations, while SWAP fidelity protocols estimate squared overlap magnitudes. A
│   │       photonic-chip study demonstrates generalized computational spectroscopy [31].
│   └── Instrument connection
│       └── Classical geometry, reference calibration, phase unwrapping, and detector response convert
│           selected simulated amplitudes into predicted interference signals for comparison with acquired
│           sample-phase data.
├── Ellipsometry and polarimetry [H/R for proposed computational targets]
│   ├── Microscopic optical response
│   │   └── Selected current or polarization correlators supply compatible optical-response quantities. A
│   │       classical constitutive model maps these to dielectric/conductivity tensors and sample-specific
│   │       propagation.
│   ├── Optical inversion
│   │   └── Classical Fresnel or transfer-matrix calculations and Jones/Mueller models connect sample
│   │       parameters to observables. Candidate quantum-assisted fits require an explicit subproblem that
│   │       accounts for depolarization and parameter identifiability.
│   └── Quantum sensing role
│       └── Squeezed or other quantum probe states modify measurement precision in quantum ellipsometry. A
│           QPU-assisted reconstruction requires its own computational subroutine and interface to the
│           acquired data [32].
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
│       └── NV centers, atomic magnetometers, and SQUIDs acquire magnetic signals. A QPU processes these
│           signals through a specified computational subroutine or an explicit coherent sensor–processor
│           algorithm.
├── Mass spectrometry and molecular identification [H/R; construction-specific Q]
│   ├── Quantum chemistry and fragmentation
│   │   └── Candidate QPU electronic calculations supply ionic energies, ionization channels, or dynamical
│   │       inputs. Classical nuclear trajectories, collision ensembles, reaction networks, and detector
│   │       models determine fragment yields; classical QCxMS is a baseline [33].
│   ├── Identification and candidate search
│   │   └── A published algorithmic proposal applies quantum graph methods to a metabolite-identification
│   │       stage. Its performance assessment requires matched identification benchmarks; fragmentation
│   │       prediction requires an additional dynamical construction [34].
│   └── Scope
│       └── Mass-to-charge separation, isotope patterns, and spectral assignment each require their own
│           physical or inference model. CPU/GPU quantum-chemical calculations are classified C; a specified
│           QPU stage defines a hybrid workflow.
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
└── Acquisition design [H/R]
    └── Selected design objectives
        └── Proposed QPU estimates or quantum optimization guide chosen tilts, probe settings, spectral
            points, or process parameters only when the forward model, utility, and uncertainty construction
            are specified.
```

</details>

#### Forward prediction, inverse reconstruction, and measurement interpretation

<details markdown="1">
<summary><strong>Explore forward models, inverse reconstruction, and probe-specific observables</strong></summary>

Forward prediction involves the evaluation of a sample and instrument model, whereas inverse reconstruction is the process of inferring sample parameters from measured data. These roles remain independent of the previously discussed temporal formulations (as inverse fits may utilize static projections, frequency-resolved spectra, or time-dependent signals depending on the specific measurement model). In this framework, a quantum processing unit (QPU) is used to directly evaluate assigned operator actions, quantum correlations, or optimization subproblems, while the surrounding physical and instrument models mediate the connection to the final observable. Finally, the modality placements designated as "R" in [branch XI](#branch-xi) represent proposed applications of existing primitives that still necessitate target-specific construction and validation.

> $$
> \boldsymbol{\theta}\xrightarrow{\;\mathcal{F}\;}\boldsymbol{y}_{\mathrm{pred}},
> \qquad
> \boldsymbol{y}_{\mathrm{meas}}\longrightarrow\widehat{\boldsymbol{\theta}}.
> $$

The parameter vector $\boldsymbol{\theta}$ describes the selected atomic coordinates, electrostatic potentials, spin-Hamiltonian parameters, optical constants, or lithographic-mask variables. The forward map $\mathcal{F}$ includes the sample–probe interaction and instrument response. Shared inverse formulations, regularization, and classical reconstruction loops are grouped in [branch XII](#branch-xii).

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

> $$
> I(\boldsymbol{R},\boldsymbol{q})
> =\left|
> \mathcal{F}_{\boldsymbol{r}\rightarrow\boldsymbol{q}}
> \left[P(\boldsymbol{r}-\boldsymbol{R})O(\boldsymbol{r})\right]
> \right|^2.
> $$

In this framework, the interaction between the probe $P$ and the object transmission $O$ generates an exit wave, the diffraction intensities of which are captured by the detector. The recovery of the phase is constrained by overlapping measurements, while the model is further extended to account for multislice propagation, partial coherence, position errors, and detector response [24]. The corresponding encoding and image-recovery costs are grouped in [branch XII](#branch-xii). While published research on quantum-state ptychography focuses on reconstructing unknown quantum states through overlapping projections, electron-microscope reconstruction requires a distinct sample–probe encoding and a corresponding inverse workflow [25].

For Angle-Resolved Photoemission Spectroscopy (ARPES), the commonly employed equilibrium sudden-approximation relation is expressed as $I(\boldsymbol{k},\omega)\propto |M(\boldsymbol{k},\omega)|^2 f(\omega)A(\boldsymbol{k},\omega)$ prior to background subtraction and instrument convolution. In this context, a quantum processing unit (QPU) may be used to calculate the spectral function $A$, although additional modeling is required to account for matrix elements $M$, occupations $f$, surface/final-state effects, and the detector response [26, 27]. While electronic spectral functions are defined by electron addition/removal operators, density and spin operators define their respective response structure factors. Related solver primitives utilizing these probe-specific operators are employed across ARPES, EELS, and magnetic INS [3, 4, 28]. Furthermore, quantum-assisted Hamiltonian inference leverages repeated model predictions to fit experimental data, thereby bridging the gap between forward spectroscopy and inverse characterization [30].

The role of a quantum sensor is to enhance or modify data acquisition through its specific physical probe and measurement protocol. In contrast, a quantum computer performs a designated computational operation; any workflow integrating both requires an explicit interface and a comprehensive resource analysis. For example, squeezed-light ellipsometry is categorized as quantum sensing even when its parameter fitting is performed classically [32]. Because full-image reconstruction, selected response coefficients, and microscopic model predictions each impose different output requirements, readiness is assigned based on the specific output and implementation.

</details>

<a id="resource-accounting"></a>
<a id="branch-xii"></a>

<details markdown="1">
<summary><strong>Branch XII: Shared computational overheads and classical interfaces</strong></summary>

Shared computational requirements accompany the selected application and solver. Hardware assignments are grouped in [branch X](#branch-x); application-specific physical models are grouped in [branch XI](#branch-xi).

<details markdown="1">
<summary>Shared inverse-problem primitives</summary>

```text
Inverse-problem primitives [Q/H/QA; model-specific]
├── Regularized linear reconstruction [Q/H]
│   └── QLSA, QSVT, VQLS, or projected methods target a coherently accessible linear subproblem. Include
│       input loading, conditioning, preconditioning, normalization, and selected-observable or
│       full-image recovery [6–8].
├── Discrete reconstruction and model selection [QA/H; Q/H for a specified QAOA mapping]
│   └── Encode a bounded integer or binary objective as QUBO/Ising variables. Quantum annealing or
│       gate-based optimization processes the explicit objective; discretization, penalties,
│       connectivity, and classical work determine total cost [23].
└── Nonlinear reconstruction and parameter inference [H; R for unresolved targets]
    └── A classical outer loop updates sample and nuisance parameters while a specified quantum
        subroutine evaluates selected forward predictions, derivatives, or linearized updates. The
        nonlinear objective requires an explicit quantum subproblem and resource construction.
```

</details>

<details markdown="1">
<summary>Regularization and classical reconstruction loops</summary>

Parameter definitions and the forward map are in the [measurement-interpretation framework](#forward-prediction-inverse-reconstruction-and-measurement-interpretation).

A general regularized inverse formulation is

> $$
> \widehat{\boldsymbol{\theta}}
> =\underset{\boldsymbol{\theta}\in\mathcal{C}}{\operatorname{argmin}}
> \left[
> D\!\left(\mathcal{F}(\boldsymbol{\theta}),\boldsymbol{y}_{\mathrm{meas}}\right)
> +\lambda R(\boldsymbol{\theta})
> \right].
> $$

Here, $D$ is a discrepancy appropriate to the measurement-noise model, $R$ represents prior information, $\lambda$ controls regularization, and $\mathcal{C}$ specifies physical constraints. Weighted least squares is appropriate for a justified Gaussian approximation, whereas low-count measurements often require a Poisson likelihood. A linear quantum solver addresses a compatible linear problem or a linearized update within the nonlinear reconstruction loop [6–8]. For a least-squares normal-equation construction, $A^\dagger A$ squares the spectral condition number when $A$ has full column rank; alternative formulations and preconditioning therefore belong in the resource assessment.

</details>

<details markdown="1">
<summary>Classical interfaces and inference constraints</summary>

| Application | Classical interface or inference constraint |
| --- | --- |
| Projection-based electron tomography | Under a justified projection approximation, discretized line-integral data define a regularized inverse problem. Multiple scattering or nonlinear image formation requires a different forward model. |
| Atomic-coordinate and potential refinement | Classical alignment, segmentation, and instrument modeling accompany structural refinement. Measurement coverage and justified priors constrain inference in the missing wedge. |
| NMR Hamiltonian parameter learning | A quantum simulator supplies predictions and specified derivatives inside a classical fit to time-resolved measurements; published NMR constructions assess learning of nuclear-spin interactions [30]. |

</details>

<details markdown="1">
<summary>Measurement, data loading, and image export</summary>

```text
Measurement, reconstruction, and requested-output costs
├── Output constraints
│   └── A QFT-based reconstruction requires explicit costs for input encoding, intensity constraints,
│       signed or complex-amplitude recovery, probe/object ambiguities, detector response, and
│       full-image export, compared at matched precision with a classical FFT workflow [7, 24].
└── Matched experimental goals
    └── Assess computational advantage and dose reduction against appropriate classical methods at
        matched reconstruction precision, physical model, acquisition conditions, and total processing
        cost.
```

For the [ptychographic reconstruction workflow](#forward-prediction-inverse-reconstruction-and-measurement-interpretation):

Within this process, a quantum Fourier transform is applied to the encoded amplitudes; however, recovery via classical arrays necessitates a specific measurement procedure. The total resource cost is determined by several factors, including initial-data loading, nonlinear intensity constraints, complex-amplitude recovery, repeated measurements, and full-image readout.

</details>

<details markdown="1">
<summary>Complete-workflow validation and resource accounting</summary>

```text
Complete-workflow validation and resource accounting
├── Matched baselines
│   └── compare the same model, observable, precision, and total processing cost against appropriate
│       CPU/GPU methods.
├── Error budget
│   └── separate physical-model, discretization, truncation, quantum-algorithm, sampling, hardware, and
│       hybrid-coupling errors.
├── Data access
│   └── coherent matrix or QRAM access requires a specified quantum interface to the stored data, with
│       loading and access costs included.
├── Output and advantage
│   └── state preparation, normalization, postselection, repetitions, classical processing, and
│       full-field recovery count toward total cost.
└── Hybrid validity
    └── a proposed QPU+CA workflow requires an error budget for conversion, noise, drift, residual
        correction, and precision, together with a matched comparison of complete processing time.
```

</details>

</details>

[Back to solver router](#quick-start-solver-selection-router) · [Back to contents](#table-of-contents)

<a id="application-framework"></a>

## Implementation workflows

### Application areas and concrete examples

The following application tree originates from the primary scientific task, linking each example to a predicted quantity, a solver family, and its associated execution roles. Branch references I–XII correspond to the technical deep-dives in the selection map, while numbered citations refer to the primary research anchors. Microscopic electronic, spin, and vibrational models are used to describe quantum degrees of freedom directly, whereas kinetic and continuum models describe reduced populations or fields, the physical validity of which is governed by their closure, boundary conditions, and constitutive assumptions.

#### Application tree (compact version)

```
QPU and hybrid quantum-solvers: application areas and examples
├── A. Electronic structure and quantum chemistry
│   ├── Electronic ground-state energies
│   ├── Ground-state preparation and energy estimation
│   ├── Electronic band structure and dispersion relations
│   ├── Periodic electronic spectra
│   └── Candidate defect, catalyst, and adsorbate energetics
├── B. Atomistic, molecular, and nuclear dynamics
│   ├── Finite-temperature molecular trajectories
│   ├── Chemical reaction wavepacket propagation
│   ├── Candidate nonadiabatic reaction dynamics
│   └── Candidate vibronic and phonon-coupled molecular models
├── C. Magnetism, spin dynamics, and correlated materials
│   ├── Molecular-magnet inelastic neutron scattering
│   ├── Quantum-magnet scattering benchmarks
│   ├── Candidate correlated lattice and impurity calculations
│   └── Candidate spintronic material-parameter extraction
├── D. Computational spectroscopy and material-response prediction
│   ├── ARPES-related electronic spectral functions
│   ├── Core-level electron energy-loss spectroscopy
│   ├── Nuclear magnetic resonance and molecular spin inference
│   ├── Generalized computational spectroscopy
│   └── Candidate optical, X-ray, Raman, and spin-resonance response
├── E. Computational interferometry and coherent scattering
│   ├── Phase-sensitive molecular wavepacket correlations
│   ├── Candidate Loschmidt amplitudes and quantum echoes
│   └── Candidate interferometer and scattering-signal prediction
├── F. Quantum nanodevices, electronic transport, and dissipation
│   ├── Candidate correlated nanodevice transport
│   ├── Candidate decoherence and relaxation models
│   └── Candidate photon, phonon, and magnon subsystem dynamics
├── G. Quantum lattice kinetics, fluid dynamics, and transport equations
│   ├── Nonlinear quantum-lattice-Boltzmann fluid models
│   ├── Candidate confined-fluid and nanofluid transport
│   ├── Candidate electronic Wigner or Boltzmann kinetics
│   └── Candidate phonon and reaction–transport kinetics
├── H. Quantum finite elements, finite volumes, and field-equation simulation
│   ├── Preconditioned elliptic finite elements
│   ├── Computational-fluid finite volumes
│   ├── Maxwell transients and time-domain finite differences
│   ├── Candidate nanodevice electrostatics and resonator fields
│   ├── Candidate nanoscale elasticity and diffusion fields
│   └── Candidate finite-difference electronic wavepacket dynamics
├── I. Finite-temperature properties, ensembles, and stochastic estimates
│   ├── Eigenstate and thermal-state calculations
│   ├── Candidate temperature-dependent material response
│   ├── Quantum-assisted electronic Monte Carlo
│   └── Coherent expectation estimation and uncertainty propagation
├── J. Materials characterization, imaging, and inverse reconstruction
│   ├── Regional tomographic refinement
│   ├── Candidate electron tomography and atomic-coordinate refinement
│   ├── Candidate 4D-STEM ptychography and electron ptychographic tomography
│   ├── Candidate ellipsometric, polarimetric, and magnetic characterization
│   └── Candidate mass-spectrum prediction and molecular identification
├── K. Multiscale materials and manufacturing-process prediction
│   ├── Fusion-blanket molten-salt electronic fragments
│   ├── EUV photoresist microscopic spectroscopy
│   ├── Quantum-enhanced numerical homogenization
│   └── Candidate microscopic-to-device materials prediction
└── L. Inverse design, acquisition planning, and model selection
      ├── Candidate EUV mask and source optimization
      ├── Candidate experimental acquisition design
      └── Candidate material and molecular model selection
```

#### Application tree (extended version)

<details markdown="1">
<summary>Application examples, outputs, solver routes, and evidence</summary>

```text
QPU and hybrid quantum-solvers: application areas and examples
├── A. Electronic structure and quantum chemistry
│   ├── Electronic ground-state energies [H; 💻 HW; 1]
│   │   ├── Examples: N2 dissociation; [2Fe-2S] and [4Fe-4S] molecular clusters
│   │   ├── Outputs: electronic energies and sample-selected wavefunction approximations
│   │   └── Route: SQD samples on the QPU + classical selected-space diagonalization (I)
│   ├── Ground-state preparation and energy estimation [Q/H; 📝 ALG/🔢 NUM; 8, 18]
│   │   ├── Examples: a directly encoded molecular or interacting-electron Hamiltonian
│   │   ├── Outputs: prepared low-energy states and selected energy estimates
│   │   └── Routes: VQE, ADAPT-VQE, QITE, Krylov/subspace methods, filters, or QPE (I)
│   ├── Electronic band structure and dispersion relations [Q/H; 🔍 R]
│   │   ├── Examples: periodic solid k-space evaluations; strongly correlated tight-binding lattices
│   │   ├── Outputs: momentum-resolved energy eigenvalues and correlated bandgap estimates
│   │   └── Route: Bloch-encoded quantum eigensolvers + classical Brillouin zone sampling (I, VIII)
│   ├── Periodic electronic spectra [Q/H; 💻 HW model example; 26]
│   │   ├── Example: a 27-site electronic spectral-function model
│   │   ├── Outputs: momentum- and frequency-resolved spectral functions
│   │   └── Route: quantum electronic dynamics + classical spectral interpretation (I, III)
│   └── Candidate defect, catalyst, and adsorbate energetics [H; 🔍 R]
│       ├── Examples: charge-state energy differences; adsorbate binding; reaction intermediates
│       ├── Outputs: selected energy differences within a defined active-space model
│       └── Route: quantum fragment/eigenstate solver + geometry and environment modeling (I, VIII)
├── B. Atomistic, molecular, and nuclear dynamics
│   ├── Finite-temperature molecular trajectories [H; 🔢 NUM; 14]
│   │   ├── Example: QCPMD molecular motion and vibrational-frequency analysis
│   │   ├── Outputs: nuclear trajectories, equilibrium statistics, and vibrational frequencies
│   │   └── Route: QPU electronic measurements + coupled classical parameter/nuclear dynamics (VIII)
│   ├── Chemical reaction wavepacket propagation [Q/H; 🔢 NUM; 9]
│   │   ├── Example: prepared molecular reaction wavepackets in the cited circuit study
│   │   ├── Outputs: wavepacket correlations and selected transition amplitudes
│   │   └── Route: encoded Hamiltonian evolution + channel analysis and spectral transforms (II, III)
│   ├── Candidate nonadiabatic reaction dynamics [Q/H; 🔍 R]
│   │   ├── Examples: coupled electronic surfaces; photoinduced charge-transfer models
│   │   ├── Outputs: electronic populations, coherences, and specified reaction-channel probabilities
│   │   └── Route: multi-state quantum dynamics + a defined nuclear/coupling model (II, VIII)
│   └── Candidate vibronic and phonon-coupled molecular models [Q/H/AQ; 🔍 R]
│       ├── Examples: an electronic transition coupled to selected molecular vibrations
│       ├── Outputs: mode occupations, vibronic correlations, and selected spectral features
│       └── Route: bosonic/qudit encoding or native spin–oscillator dynamics + mode fitting (II, VII)
├── C. Magnetism, spin dynamics, and correlated materials
│   ├── Molecular-magnet inelastic neutron scattering [Q/H; 💻 HW; 4]
│   │   ├── Example: prototypical finite spin systems representing magnetic molecules
│   │   ├── Outputs: spin correlations, magnetic structure factors, and neutron cross sections
│   │   └── Route: quantum spin evolution + form factors, polarization, and spectral reconstruction (III)
│   ├── Quantum-magnet scattering benchmarks [Q/H; 💻 HW preprint; 3]
│   │   ├── Example: a spin-chain model compared with KCuF3 neutron-scattering measurements
│   │   ├── Outputs: momentum- and energy-resolved magnetic response
│   │   └── Route: quantum correlation measurements + matched experimental-response analysis (III)
│   ├── Candidate correlated lattice and impurity calculations [H/AQ; 🔍 R for the selected target]
│   │   ├── Examples: Hubbard-type spin/charge correlations; correlated impurity models
│   │   ├── Outputs: local observables, Green functions, and phase-dependent correlations
│   │   └── Route: native quantum lattice simulation or QPU impurity solver + embedding (IV, VII, VIII)
│   └── Candidate spintronic material-parameter extraction [H; 🔍 R]
│       ├── Examples: selected exchange couplings, anisotropy energies, and magnetic susceptibility
│       ├── Outputs: microscopic parameters or response quantities for a justified coarse model
│       └── Route: quantum spin/electronic solver + parameter fitting and classical micromagnetics (VIII)
├── D. Computational spectroscopy and material-response prediction
│   ├── ARPES-related electronic spectral functions [Q/H; 💻 HW preprint; 26]
│   │   ├── Example: a 27-site model evaluated with 54 Quantinuum H2 qubits
│   │   ├── Outputs: selected electronic spectral functions relevant to photoemission
│   │   └── Route: QPU spectral measurement + photoemission matrix elements and instrument response (XI)
│   ├── Core-level electron energy-loss spectroscopy [Q/H; 📝 ALG; 28]
│   │   ├── Example: oxygen K-edge response of an oxygen-centered Li2MnO3 battery-material cluster
│   │   ├── Outputs: electronic dynamical structure factors and selected EELS spectra
│   │   └── Route: quantum electronic correlations + scattering and beam-response models (III, XI)
│   ├── Nuclear magnetic resonance and molecular spin inference [Q/H; 💻 HW/📝 ALG; 29, 30]
│   │   ├── Examples: four-qubit zero-field NMR; inference of molecular spin-Hamiltonian parameters
│   │   ├── Outputs: NMR spectra or fitted interaction parameters
│   │   └── Route: quantum spin dynamics + pulse/ensemble modeling and classical fitting (III, XI)
│   ├── Generalized computational spectroscopy [Q/H; 💻 HW; 31]
│   │   ├── Example: ancilla-assisted spectroscopy on a silicon-photonic quantum processor
│   │   ├── Outputs: autocorrelation-derived spectral information
│   │   └── Route: quantum interferometric measurements + classical spectral reconstruction (III, XI)
│   └── Candidate optical, X-ray, Raman, and spin-resonance response [Q/H/AQ; 🔍 R]
│       ├── Examples: selected absorption transitions; polarization response; vibrational sidebands
│       ├── Outputs: compatible transition strengths and operator-specific correlation spectra
│       └── Route: eigenstates/dynamics + the probe's selection rules and linewidth model (I–III, VII, XI)
├── E. Computational interferometry and coherent scattering
│   ├── Phase-sensitive molecular wavepacket correlations [Q/H; 🔢 NUM; 9]
│   │   ├── Example: complex overlap between a propagated state and a reference state
│   │   ├── Outputs: relative phase and complex transition or autocorrelation amplitudes
│   │   └── Route: reference-branch interference or a Hadamard test + classical analysis (II, III)
│   ├── Candidate Loschmidt amplitudes and quantum echoes [Q/H; 🔍 R]
│   │   ├── Example: forward/reversed evolution of a directly specified spin model
│   │   ├── Outputs: complex return amplitudes or echo probabilities, according to the protocol
│   │   └── Route: controlled Hamiltonian evolution + overlap estimation (II, III)
│   └── Candidate interferometer and scattering-signal prediction [Q/H; 🔍 R]
│       ├── Examples: matter-wave path amplitudes; selected scattering-channel interference
│       ├── Outputs: predicted phases and intensities under a specified propagation model
│       └── Route: quantum wave dynamics + geometry, reference calibration, and detector response (II, XI)
├── F. Quantum nanodevices, electronic transport, and dissipation
│   ├── Candidate correlated nanodevice transport [H; 🔍 R]
│   │   ├── Examples: interacting quantum dots; molecular-junction active regions; impurity-assisted transport
│   │   ├── Outputs: selected Green functions or self-energies entering current/response calculations
│   │   └── Route: QPU correlated subproblem + classical NEGF, contacts, and self-consistency (IV, VIII)
│   ├── Candidate decoherence and relaxation models [Q/H/AQ; 🔍 R for the selected device]
│   │   ├── Examples: spin–boson baths; qubit dephasing; dissipative electronic or vibrational dynamics
│   │   ├── Outputs: populations, coherences, bath correlations, and fitted relaxation parameters
│   │   └── Route: channels, defined baths, or quantum trajectories + ensemble analysis (IV)
│   └── Candidate photon, phonon, and magnon subsystem dynamics [Q/H/AQ; 🔍 R]
│       ├── Examples: coupled spin–mode models; truncated cavity or vibrational occupations
│       ├── Outputs: mode populations, exchange dynamics, and selected response functions
│       └── Route: encoded or native quantum mode evolution + physical-parameter calibration (II, VII)
├── G. Quantum lattice kinetics, fluid dynamics, and transport equations
│   ├── Nonlinear quantum-lattice-Boltzmann fluid models [Q/H; 🔢 NUM; 10]
│   │   ├── Examples: vortex-pair merging and decaying-turbulence benchmarks
│   │   ├── Outputs: selected density/velocity moments and flow statistics
│   │   └── Route: node-level ensemble collision/streaming + boundary and observable processing (VI)
│   ├── Candidate confined-fluid and nanofluid transport [Q/H; 🔍 R]
│   │   ├── Examples: channel flow with a justified slip or boundary-scattering model
│   │   ├── Outputs: selected fluxes, velocity moments, and pressure-response quantities
│   │   └── Route: a regime-appropriate lattice kinetic closure + geometry/boundary updates (VI)
│   ├── Candidate electronic Wigner or Boltzmann kinetics [Q/H; 🔍 R]
│   │   ├── Examples: phase-space electronic transport; selected collision-resolved distribution moments
│   │   ├── Outputs: charge/energy currents or selected distribution functionals
│   │   └── Route: direct kinetic encoding, scattering kernels, and evolution + observable recovery (VI)
│   └── Candidate phonon and reaction–transport kinetics [Q/H; 🔍 R]
│       ├── Examples: reduced phonon populations; coupled diffusion and reaction variables
│       ├── Outputs: selected energy fluxes, species populations, or effective rates
│       └── Route: specified linear evolution or justified nonlinear lifting + classical closure (V, VI)
├── H. Quantum finite elements, finite volumes, and field-equation simulation
│   ├── Preconditioned elliptic finite elements [Q/H; 📝 ALG/💻 HW prototype; 5]
│   │   ├── Example: the cited second-order elliptic problem on a Cartesian finite-element grid
│   │   ├── Outputs: suitable functionals of the discretized solution
│   │   └── Route: BPX-preconditioned quantum FEM + load/constraint construction (V)
│   ├── Computational-fluid finite volumes [Q/H; 📝 ALG; 11]
│   │   ├── Example: the cited quantum CFD/FVM formulation with classical input and output
│   │   ├── Outputs: the formulation's encoded or reconstructed flow quantities
│   │   └── Route: quantum finite-volume updates + specified QRAM and classical data exchange (V, VI)
│   ├── Maxwell transients and time-domain finite differences [Q/H; 📝 ALG/💻 HW preprint; 19–21]
│   │   ├── Examples: driven fields; dielectric interfaces; specified conducting boundaries
│   │   ├── Outputs: selected electric/magnetic fields or field functionals
│   │   └── Route: Maxwell/Yee-type Schrödingerisation + source, boundary, and readout handling (V)
│   ├── Candidate nanodevice electrostatics and resonator fields [Q/H; 🔍 R]
│   │   ├── Examples: Poisson gate potentials; Helmholtz field response in a specified structure
│   │   ├── Outputs: selected potentials, field overlaps, or driven-response quantities
│   │   └── Route: QLSA/QSVT/VQLS on a suitable discretized operator + geometry/material modeling (V)
│   ├── Candidate nanoscale elasticity and diffusion fields [Q/H; 🔍 R]
│   │   ├── Examples: membrane displacement; heterogeneous diffusion under a valid continuum model
│   │   ├── Outputs: selected displacement/flux functionals and compatible spectral quantities
│   │   └── Route: a constructed FEM/FVM/eigenproblem + constitutive and boundary data (V)
│   └── Candidate finite-difference electronic wavepacket dynamics [Q/H; 🔍 R for the selected target]
│       ├── Example: propagation of a prepared wavepacket across a specified nanoscale potential
│       ├── Outputs: selected position probabilities, complex overlaps, or outgoing-channel weights
│       └── Route: discretized Schrödinger Hamiltonian evolution + physical normalization (II, V)
├── I. Finite-temperature properties, ensembles, and stochastic estimates
│   ├── Eigenstate and thermal-state calculations [Q/H; 💻 HW prototypes/ 🔢 NUM thermal; 18]
│   │   ├── Examples: prototype QITE eigenstate circuits and numerically evaluated Gibbs averages
│   │   ├── Outputs: low-energy estimates and selected thermal expectation values
│   │   └── Route: QITE/thermal sampling + classical coefficient solves and averaging (I, IX)
│   ├── Candidate temperature-dependent material response [Q/H/AQ; 🔍 R]
│   │   ├── Examples: spin susceptibility; thermal correlations; energy fluctuations and heat capacity
│   │   ├── Outputs: temperature-dependent response and thermodynamic observables
│   │   └── Route: thermal-state preparation + operator measurements and physical analysis (III, IX)
│   ├── Quantum-assisted electronic Monte Carlo [H; 💻 HW inputs + 🔢 NUM; 16]
│   │   ├── Example: sample-based quantum diagonalization coupled to phaseless AFQMC
│   │   ├── Outputs: improved electronic energy estimates within the stated stochastic approximation
│   │   └── Route: quantum-selected state information + classical AFQMC (I, IX)
│   └── Coherent expectation estimation and uncertainty propagation [Q/H; 📝 ALG; 15]
│       ├── Example: an expectation with a coherently implementable sampling/evaluation procedure
│       ├── Outputs: selected means or probabilities at a specified estimation tolerance
│       └── Route: quantum amplitude estimation + construction of the complete sampling oracle (IX)
├── J. Materials characterization, imaging, and inverse reconstruction
│   ├── Regional tomographic refinement [QA/H; 💻 HW; 23]
│   │   ├── Examples: CT phantoms and a chest-slice reconstruction in the cited hybrid study
│   │   ├── Outputs: regional image updates from a defined QUBO objective
│   │   └── Route: D-Wave hybrid optimization + classical regional assembly and refinement (XI)
│   ├── Candidate electron tomography and atomic-coordinate refinement [Q/H/QA; 🔍 R; 23, 24]
│   │   ├── Examples: reduced electrostatic-potential coefficients; selected structural parameters
│   │   ├── Outputs: fitted structure or volume information consistent with the measurement model
│   │   └── Route: a constructed quantum inverse/optimization subproblem + alignment and scattering (XI)
│   ├── Candidate 4D-STEM ptychography and electron ptychographic tomography [Q/H; 🔍 R; 24]
│   │   ├── Examples: reduced object/probe updates; selected multislice model parameters
│   │   ├── Outputs: complex transmission or structural estimates under intensity constraints
│   │   └── Route: direct quantum propagation/inverse action + nonlinear classical reconstruction (XI)
│   ├── Candidate ellipsometric, polarimetric, and magnetic characterization [H; 🔍 R]
│   │   ├── Examples: dielectric-tensor response; magnetic susceptibility; reduced source inference
│   │   ├── Outputs: material-response quantities or fitted sample parameters
│   │   └── Route: QPU microscopic correlations + optical/magnetostatic forward models and fitting (III, XI)
│   └── Candidate mass-spectrum prediction and molecular identification [Q/H; 🔍 R; 33, 34]
│       ├── Examples: ionic-state energy inputs; fragmentation-channel models; metabolite candidate search
│       ├── Outputs: selected molecular inputs, predicted fragment yields, or candidate rankings
│       └── Route: a defined quantum chemistry/search subproblem + fragmentation and instrument models (XI)
├── K. Multiscale materials and manufacturing-process prediction
│   ├── Fusion-blanket molten-salt electronic fragments [H; 💻 HW preprint; 2]
│   │   ├── Example: representative FLiBe clusters containing tritium-related configurations
│   │   ├── Outputs: correlated fragment ground-state energies, with separately assessed embedding error
│   │   └── Route: AIMD snapshots + EWF fragmentation + ext-SQD and classical diagonalization (VIII)
│   ├── EUV photoresist microscopic spectroscopy [Q/H; 📝 ALG preprint; 35]
│   │   ├── Example: absorption and photoemission spectra of the model IMePh resist monomer
│   │   ├── Outputs: microscopic spectral inputs for absorption and emitted-electron modeling
│   │   └── Route: quantum spectral/evolution algorithms + electron cascades, chemistry, and development (XI)
│   ├── Quantum-enhanced numerical homogenization [H; 📝 ALG/🔢 NUM preprint; 17]
│   │   ├── Example: a scalar elliptic PDE with heterogeneous diffusion coefficients
│   │   ├── Outputs: selected fine-scale corrections entering a classical coarse model
│   │   └── Route: quantum local subproblems + classical localized coarse-scale construction (V, VIII)
│   └── Candidate microscopic-to-device materials prediction [H; 🔍 R]
│       ├── Examples: correlated optical response supplied to a field model; spin parameters supplied to LLG
│       ├── Outputs: device-level fields or dynamics mediated by the specified coarse-graining model
│       └── Route: quantum electronic/spin solver + CPU/GPU continuum or micromagnetic simulation (VIII)
└── L. Inverse design, acquisition planning, and model selection
    ├── Candidate EUV mask and source optimization [Q/H/QA; 🔍 R; 19–21, 36]
    │   ├── Examples: bounded mask variables or source settings with a defined imaging objective
    │   ├── Outputs: candidate designs evaluated against process and manufacturing constraints
    │   └── Route: a constructed quantum field/optimization subproblem + classical lithography models (XI)
    ├── Candidate experimental acquisition design [Q/H/QA; 🔍 R]
    │   ├── Examples: tomography tilts; diffraction-probe settings; selected spectral sampling points
    │   ├── Outputs: settings ranked by a defined precision, dose, or information objective
    │   └── Route: a quantum prediction/optimization subproblem + statistical design and calibration (XI)
    └── Candidate material and molecular model selection [Q/H/QA; 🔍 R]
        ├── Examples: bounded structural candidates or Hamiltonian hypotheses
        ├── Outputs: objective values or ranked candidates under a stated uncertainty model
        └── Route: quantum subproblem + classical parameter fitting and independent verification (I, XI)
```

</details>

### Application-to-solver mapping

| Application | Solver branches | Direct quantum task | Essential or useful classical task | Model, accuracy, and output requirements |
| --- | --- | --- | --- | --- |
| Molecular electronic ground-state energies | [I](#branch-i) | VQE, ADAPT-VQE, SQD/QSCI, Krylov, QITE, or QPE | Integrals, orbital selection, optimization or projected diagonalization | Accuracy is specific to basis, active space, preparation, and sampling. |
| Periodic electronic structure and band gaps | [I](#branch-i), [VIII](#branch-viii) | Selected eigenstates, charged excitations, or Green functions | Periodic basis, momentum sectors, embedding, spectral fitting | Band dispersion requires momentum-resolved excitation or spectral information. |
| Molten salts and complex molecular environments | [VIII](#branch-viii) | Extended SQD for correlated embedded fragments | AIMD snapshots, EWF fragmentation, CPU/GPU diagonalization | Total binding-energy accuracy requires assessment of fragmentation and embedding errors [2]. |
| Magnetic inelastic neutron scattering | [III](#branch-iii) | Spin-state preparation, real-time evolution, correlation measurements | Momentum/time transforms, form factors, instrument convolution | Simulates material response; limited time windows affect energy resolution [3, 4]. |
| Computational interferometry | [II](#branch-ii), [III](#branch-iii) | Complex overlaps, controlled phases, propagator matrix elements | Geometry, model assembly, reconstruction, fitting | Complex overlap phases require a phase-sensitive reference protocol. |
| Chemical reaction scattering | [II](#branch-ii), [III](#branch-iii) | Wavepacket evolution and transition amplitudes | Potential surfaces, incoming/outgoing channel normalization, rate integration | The cited wavepacket study provides classical circuit-simulation evidence [9]. |
| Quantum-lattice-Boltzmann fluid dynamics | [VI](#branch-vi) | Encoded streaming and a specified collision construction | Boundary updates, nonlinear feedback where needed, moment recovery | Mesoscopic kinetic populations and their closure require validation for the selected flow regime [10]. |
| Quantum FEM/FVM | [V](#branch-v), [VI](#branch-vi) | Quantum inverse action, residual estimation, or generalized eigenproblem | Mesh assembly, constraints, preconditioners, interface data | Selected functionals and full mesh reconstruction have different costs [5, 6, 11]. |
| Quantum FDTD and related transient finite-difference evolution | [V](#branch-v) | Maxwell/Yee-type Schrödingerisation, driven-source circuits, or discretized wavefunction propagation | Geometry, field preparation, sources, boundary treatment, signed-field recovery | Specify the time-update rule and its stability analysis; cited results include two-dimensional QPU benchmarks and three-dimensional simulator benchmarks [19–21]. |
| Nanodevice electromagnetic or elastic fields | [V](#branch-v) | A specified PDE solution-state subroutine | Geometry, constitutive parameters, constraints, verification | Nanoscale constitutive and nonlocal effects require a valid physical model. |
| Molecular dynamics | [VIII](#branch-viii) | Electronic energies, forces, or electronic parameter evolution | Nuclear trajectories, thermostats, cell updates | Classical nuclear propagation is an approximation; force noise affects dynamics [14]. |
| Vibronic, phononic, photonic, and magnonic dynamics | [II](#branch-ii), [VII](#branch-vii) | Bosonic/qudit evolution or native coupled-mode simulation | Mode fitting, cutoffs, analysis | Anharmonic response requires interacting-mode terms and a compatible evolution construction. |
| Decoherence and quantum-device dissipation | [IV](#branch-iv) | Channel, bath, or trajectory simulation | Ensemble averaging, fitting, validation | Markovian and non-Markovian constructions have different assumptions. |
| Strongly correlated materials | [IV](#branch-iv), [VII](#branch-vii), [VIII](#branch-viii) | Quantum impurity or fragment solver | DMFT/DMET self-consistency and embedding | Solver accuracy and embedding accuracy are separate. |
| Finite-temperature response | [III](#branch-iii), [IX](#branch-ix) | Thermal state preparation and correlations | Ensemble statistics, transforms, thermodynamic analysis | Entropy, thermalization, and mixing costs are essential. |
| Quantum-assisted Monte Carlo | [I](#branch-i), [IX](#branch-ix) | Trial-state information or coherent expectation subroutine | AFQMC or another explicitly coupled stochastic algorithm | Classical bias and oracle costs remain part of the result [15, 16]. |
| Multiscale homogenization and spintronics | [V](#branch-v), [VIII](#branch-viii) | Fine-scale solution functionals or microscopic correlations | Coarse PDE or micromagnetic evolution | Coupling and reduction errors require independent validation [17]. |
| Electron tomography and atomic electron tomography | [XI](#branch-xi) | A suitable quantum linear subproblem or discrete reconstruction objective [Q/H/QA; R for unvalidated electron targets] | Alignment, projection or multiple-scattering models, priors, coordinate refinement, and volume recovery | The cited regional CT refinement provides a starting construction; electron-specific validation requires a scattering model, missing-angle treatment, and full-output resource assessment [23]. |
| 4D-STEM ptychography and ptychographic electron tomography | [XI](#branch-xi) | Candidate quantum propagation, linearized inverse action, or reduced parameter inference [H/R] | Probe/object optimization, multislice propagation, position/coherence correction, and detector models | Electron-image reconstruction requires a target-specific nonlinear intensity model and complex-image recovery; quantum-state ptychography supplies an overlapping-projection method for state tomography [24, 25]. |
| ARPES | [XI](#branch-xi) | Selected removal spectra, Green functions, or momentum-resolved spectral functions [Q/H] | Matrix elements, occupations, final-state/surface effects, background, and resolution | The 54-qubit, 27-site spectral-function demonstration requires material-specific Hamiltonians and photoemission/instrument components for realistic ARPES prediction [26, 27]. |
| EELS and electronic inelastic scattering | [III](#branch-iii), [XI](#branch-xi) | Density-response dynamical structure factors and core-excitation dynamics [Q/H] | Beam propagation, scattering kinematics, geometry, and detector acceptance | The battery-cluster framework specifies fault-tolerant logical resources for electronic response; beam and instrument modeling complete the EELS prediction [28]. |
| NMR and spin spectroscopy | [III](#branch-iii), [XI](#branch-xi) | Spin-Hamiltonian evolution, correlators, and specified Hamiltonian-learning derivatives [Q/H/AQ] | Pulses, orientation averages, spectral reconstruction, calibration, and fitting | Small hardware spectra exist; many-spin dynamics and inverse identifiability require separate assessment [29, 30]. |
| Ellipsometry | [III](#branch-iii), [XI](#branch-xi) | Candidate microscopic optical-response quantities or an explicit fitting subproblem [H/R] | Fresnel/transfer-matrix calculations, dielectric models, calibration, and thickness/optical-constant inference | Parameter nonuniqueness and constitutive assumptions remain; quantum-enhanced probe precision is a sensing result [32]. |
| Polarimetry | [III](#branch-iii), [XI](#branch-xi) | Candidate polarization/current response predictions or specified parameter estimation [H/R] | Jones/Mueller propagation, depolarization models, calibration, and Stokes-data processing | Quantum acceleration requires an identified microscopic-response or inverse subproblem with a favorable complete resource comparison. |
| Magnetometry and magnetic imaging | [III](#branch-iii), [XI](#branch-xi) | Candidate spin correlations, susceptibility, magnetization, or a reduced inverse subproblem [H/R] | Magnetostatics, sensor transfer functions, spatial reconstruction, and uncertainty analysis | Source identifiability and sensor sampling constrain field inference; acquisition and QPU processing require a specified interface. |
| Mass spectrometry and MS/MS identification | [XI](#branch-xi) | Candidate ionic electronic/dynamical inputs or a specified graph/search algorithm [H/R; construction-specific Q] | Fragmentation trajectories, collision ensembles, kinetics, isotope patterns, and instrument response | CPU/GPU quantum chemistry provides classical microscopic inputs [C]; the quantum identification proposal addresses a selected search stage, while full-spectrum prediction requires a coupled fragmentation and instrument model [33, 34]. |
| EUV photoresist spectroscopy and microscopic process inputs | [XI](#branch-xi) | Selected microscopic spectra [Q/H] | Electron cascades, chemistry, development, and pattern statistics | Model-specific resources are listed below; process prediction requires multiscale coupling [35]. |
| EUV mask, source, and inverse process design | [XI](#branch-xi) | Candidate quantum electromagnetic or optimization subproblems [H/R] | Mask geometry, imaging, resist effects, process windows, and manufacturing constraints | A full-chip workflow requires an explicit mask/source construction and complete benchmarking; microscopic resist spectra supply material inputs to the mask-optimization objective [19–21, 36]. |
| Acquisition and model design for characterization | [XI](#branch-xi) | Proposed selected-response estimates or explicit quantum optimization of settings [H/R] | Utility definition, calibration, statistical design, and experimental execution | Assess dose reduction and computational advantage at matched reconstruction precision, acquisition conditions, and total processing cost. |

### Hybrid execution across application branches

Hardware assignment is a second axis shared by applications A–L. CPU and GPU tasks directly complete the classical stages of a hybrid mathematical algorithm. ASIC/FPGA control directly supports state preparation, gates, readout, and feed-forward; a specified numerical accelerator directly processes its assigned classical solver stage. Classical analog numerical operations and native analog quantum evolution have separate execution roles.

| Execution arrangement | Assigned work | Concrete application connection | Existing solver branch |
| --- | --- | --- | --- |
| QPU + CPU | Optimization, embedding, projected solves, nuclear integration, and parameter inference | SQD electronic energies, QCPMD trajectories, impurity self-consistency, or Hamiltonian fitting | [I](#branch-i), [VIII](#branch-viii), [X](#branch-x), [XI](#branch-xi) |
| QPU + GPU | Compatible classical diagonalization, sparse algebra, batches, ensemble analysis, and spectral processing | Selected-space chemistry, quantum-assisted transport, correlations-to-spectra processing, or multiscale field updates | [I](#branch-i), [III](#branch-iii), [IV](#branch-iv), [VIII](#branch-viii), [X](#branch-x) |
| QPU + ASIC/FPGA | Control/readout, feed-forward, error decoding, or explicitly implemented numerical kernels | Supports the chosen chemistry, dynamics, PDE, or characterization workload through the assigned hardware function | [X](#branch-x) |
| QPU + classical analog coprocessor [CA; R for the combined target] | An assigned classical matrix-vector or reduced-response operation | Candidate numerical stages within an inverse, embedding, or field-equation loop, with calibration and residual correction | [X](#branch-x) |
| Digital + native analog quantum simulation [Q/AQ] | Gate sequences and native quantum interaction blocks | Compatible spin, interacting-mode, or lattice-model dynamics | [II](#branch-ii), [IV](#branch-iv), [VII](#branch-vii) |
| Hybrid quantum annealing + classical processing [QA/H] | A bounded encoded optimization objective and classical assembly/refinement | Regional tomographic updates or a separately constructed discrete design objective | [XI](#branch-xi) |

Use [branch XII](#branch-xii) to assess shared loading, reconstruction, measurement, and validation costs for these arrangements.

[Back to solver router](#quick-start-solver-selection-router) · [Back to contents](#table-of-contents)

<a id="fundamentals"></a>

## Technical reference

Open a reference section when you need definitions, evidence labels, or connections between modeling levels.

### Primer and execution legend

<details markdown="1">
<summary><strong>Expand the primer and execution roles</strong></summary>

The physical model and requested observable must be defined prior to the selection of a solver. While quantum many-body simulation represents a physical quantum system, quantum numerical simulation encodes a mathematical problem (including classical PDEs) within quantum states. The computational difficulty of each approach is a direct consequence of the represented physical system or the encoded mathematical problem. A typical coupled nanoscale-device workflow exemplifies this integration by combining quantum electronic structure with classical nuclear dynamics and continuum electromagnetic fields.

| Label | Execution role | Interpretation |
| --- | --- | --- |
| Q | Gate-based QPU | Quantum state preparation, evolution, interference, sampling, or operator estimation performs the central quantum subroutine. Classical control electronics implement the required gates and measurements. |
| H | Essential quantum–classical hybrid | Classical optimization, diagonalization, self-consistency, trajectories, or reconstruction is part of the mathematical algorithm. |
| AQ | Programmable analog quantum simulation | Native quantum interactions implement a specified Hamiltonian or channel, with finite control range and calibration error. |
| C | Classical CPU, GPU, or accelerator | Performs numerical computation on classical representations, even when the modeled physics is quantum. |
| CA | Classical analog computation | A classical electronic, photonic, or other analog device performs a numerical operation, subject to calibration, precision, and conversion costs. |
| R | Target-specific research extension | The stated application requires a target-specific construction, cost analysis, or hardware validation. |
| QA | Quantum annealing | An accessible annealing Hamiltonian encodes an optimization objective, with classical embedding, discretization, and result processing. QA denotes optimization; AQ denotes simulation of a target physical system. |

The representation of information is characterized by discrete-variable and continuous-variable encodings, whereas the implementation of system evolution is defined by digital and analog simulation. These two frameworks operate along independent axes (representing distinct design choices); for instance, an analog spin simulator may utilize discrete spin degrees of freedom, while a continuous-variable system can also support gate-based operations.

</details>

### Evidence legend

<details markdown="1">
<summary><strong>Expand the evidence labels</strong></summary>

Execution labels Q, H, AQ, QA, C, and CA retain their definitions above. The following evidence labels identify the named example's cited implementation, model size, and validation scope.

| Evidence label | Meaning | How to interpret an example |
| --- | --- | --- |
| **HW** | Physical quantum-hardware result | A processor or quantum simulator executed the cited model task; the demonstrated size and accuracy belong to that example. |
| **NUM** | Numerical benchmark | Classical numerical experiments or quantum-circuit simulations evaluated the cited construction. |
| **ALG** | Explicit algorithm or resource construction | The source supplies a mathematical quantum workflow, complexity analysis, or logical resource estimate. |
| **R** | Target-specific research extension | The example is an inferred application of established primitives, pending a complete target construction and benchmark. |

</details>

### Acronym glossary

<details markdown="1">
<summary><strong>Expand acronym definitions and solver roles</strong></summary>

| Acronym | Expansion | Role in this guide |
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
| DFT/ AIMD | Density functional theory/ ab initio molecular dynamics | Classical electronic and atomistic workflow components with specified QPU interfaces in hybrid implementations. |
| QA | Quantum annealing | Encodes an optimization objective in an accessible annealing Hamiltonian. |
| QAOA | Quantum approximate optimization algorithm | Gate-based optimization of a specified encoded objective, assessed through circuit resources and matched classical performance. |
| QUBO | Quadratic unconstrained binary optimization | Binary objective used by suitable annealing or gate-based optimization mappings, often with penalty terms. |
| STEM/ 4D-STEM | Scanning transmission electron microscopy/ four-dimensional STEM | Raster scan with diffraction data indexed by two scan and two diffraction coordinates. |
| ET/ AET | Electron tomography/ atomic electron tomography | Reconstructs sample volumes or atom-resolved structure from electron-microscopy measurements under a specified forward model. |
| CT | Computed tomography | Projection-based reconstruction used in the cited hybrid annealing examples. |
| ARPES | Angle-resolved photoemission spectroscopy | Relates electronic removal spectra to measured intensities through a photoemission and instrument model. |
| EELS | Electron energy-loss spectroscopy | Measures energy-transfer response linked to appropriate electronic dynamical structure factors. |
| DSF | Dynamical structure factor | Momentum- and frequency-resolved correlation spectrum; the operator differs between density and spin probes. |
| NMR | Nuclear magnetic resonance | Spin spectroscopy with quantum simulation and Hamiltonian-inference applications. |
| NV/ SQUID | Nitrogen-vacancy center/ superconducting quantum interference device | Quantum sensing platforms that interface with a QPU through a specified computational or coherent protocol. |
| EUV/ ILT | Extreme ultraviolet/ inverse lithography technology | Lithographic process and inverse pattern-design workflow; microscopic resist spectroscopy is a separate quantum target. |
| MS/ MS/MS | Mass spectrometry/ tandem mass spectrometry | Mass-to-charge and fragmentation measurements with separate forward and identification tasks. |
| FFT/ QFT | Fast Fourier transform/ quantum Fourier transform | Classical array transform and quantum amplitude transform, respectively; their inputs and outputs differ. |
| Jones/ Mueller | Jones polarization amplitudes/ Mueller Stokes-vector transfer matrix | Optical propagation descriptions; Mueller models accommodate depolarization under appropriate physical constraints. |

</details>

### Physical scale and predicted-output connections

<details markdown="1">
<summary><strong>Expand modeling levels and predicted outputs</strong></summary>

| Modeling level | Represented quantities | Application examples | Direct and indirect connection |
| --- | --- | --- | --- |
| Microscopic quantum | Electronic, spin, photon, phonon, or vibrational states and correlations | Molecular electronic energies, magnetic INS, spectral functions, and coupled-mode dynamics | The quantum solver directly supplies model observables; probe and environment models mediate their connection to measured signals. |
| Atomistic or molecular | Atomic coordinates coupled to a specified electronic/nuclear approximation | Molecular trajectories, fragment energies, reaction channels, and structural refinement | Quantum electronic quantities directly enter energy/force or fragment calculations; nuclear propagation and embedding mediate the complete prediction. |
| Mesoscopic kinetic | Encoded lattice populations or phase-space distributions | QLBM flows and proposed confined-fluid, electronic, or phonon kinetics | The quantum subroutine directly processes its assigned kinetic equation; moment closure and boundary physics determine the physical interpretation. |
| Continuum field | Discretized potentials, electromagnetic fields, displacement, or diffusion variables | FEM/FVM, Maxwell transients, nanodevice electrostatics, and homogenization | Quantum operator actions directly address the discretized equations; constitutive and coarse-graining models mediate the connection to a nanoscale device. |
| Inverse inference and process design | Reduced parameters, images, structural candidates, or design variables | Tomography, ptychography, spectral fitting, and lithographic design | The quantum subproblem directly contributes an estimate or update; acquisition, instrument response, regularization, and process models determine the inferred or designed result. |

</details>

### Direct and indirect connections

<details markdown="1">
<summary><strong>Expand physical, computational, and terminology connections</strong></summary>

```
├── Hardware and Solvers
│   ├── Hamiltonian solver
│   │   └── Direct: Supplies electronic energies or spin correlations
│   ├── Embedding approximation
│   │   └── Indirect: Modifies outputs by changing the presented Hamiltonian
│   ├── GPU sparse algebra
│   │   └── Direct: Solves the classical projected SQD problem
│   ├── ASIC control
│   │   └── Indirect: Affects scientific accuracy via implemented gates and measurements
│   └── Classical analog coprocessor
│       ├── Direct: Approximates the assigned numerical operation
│       └── Indirect: Influences the hybrid solution via noise and conversion errors
├── Quantum Models and Dynamics
│   ├── QLBM
│   │   └── Implements lattice kinetic structure linked to Boltzmann's statistical description via a specified quantum encoding and evolution strategy
│   ├── QCPMD
│   │   └── Combines a QPU electronic representation with the coupled-dynamics idea of Car–Parrinello molecular dynamics
│   └── Vibronic coupling
│       └── Identifies direct interaction between nuclear vibrational modes and electronic states
├── Observables and Reconstruction Workflows
│   ├── QPU-computed electronic spectral function
│   │   ├── Direct: Supplies a material-response quantity
│   │   └── Indirect: Mediates connection to ARPES intensity via photoemission matrix elements and instrument convolution [26, 27]
│   ├── Quantum density correlations
│   │   └── Supply a dynamical structure factor utilized by an EELS scattering model [28]
│   ├── Reconstruction workflow
│   │   ├── Direct: QPU processes the assigned inverse or optimization subproblem
│   │   └── Indirect: Alignment, regularization, and calibration modify the inferred structure
│   └── Constraints and Inference
│       ├── Acquisition: Defined by detector response, sampling, and measurement coverage
│       └── Inference: Uses acquired data and justified priors to deduce the requested structure
├── Inference and Forward Models
│   ├── Quantum resist spectra
│   │   └── Supply microscopic inputs to a lithographic-process model coupling electron transport, chemistry, and development [35]
│   ├── Spin-Hamiltonian inference
│   │   └── Connects simulated correlators to measured data using a fitted forward model [30]
│   └── Shared mathematical roles
│       ├── Examples: Thin-film ellipsometry, magnetic-field inversion, mass-spectrum assignment
│       ├── Commonalities: Shared forward-model/ inverse-inference relationships supporting cross-references
│       └── Distinctions: Distinct physical operators, measurement models, and reconstruction objectives per workflow
└── Terminology and Etymology
    ├── Tomography
    │   └── Greek roots: Section writing or recording
    ├── Ptychography
    │   └── Greek roots: Fold writing or recording reflecting overlapping diffraction information
    ├── Ellipsometry
    │   └── Reference: Measurement derived from the polarization ellipse
    ├── 4D-STEM
    │   └── Structure: Two real-space scan coordinates alongside two diffraction coordinates
    └── Spectrotomography
        ├── Definition: Spatial reconstruction linked to spectral information
        └── Requirement: Quantum-assisted placement demands treatment of both reconstruction and the relevant spectral model
```

The complete workflow is selected as physical model → encoded operator/state → quantum subroutine → classical or analog coprocessing → requested observable → validation at matched accuracy.

</details>

[Back to solver router](#quick-start-solver-selection-router) · [Back to contents](#table-of-contents)

## References

1. Robledo-Moreno et al., *Chemistry beyond the scale of exact diagonalization on a quantum-centric supercomputer*, Science Advances (2025). [Paper](https://arxiv.org/abs/2405.05068).
2. Das et al., *Quantum Computations on Fusion Blanket Molten Salts* (2026 preprint). [Paper](https://arxiv.org/abs/2606.30402). The abstract reports fragment agreement alongside much larger fragmentation errors in conformational and binding energies.
3. Lee et al., *Benchmarking quantum simulation with neutron-scattering experiments* (2026 preprint). [Paper](https://arxiv.org/abs/2603.15608).
4. Chiesa et al., *Quantum hardware simulating four-dimensional inelastic neutron scattering*, Nature Physics (2019). [Paper](https://www.nature.com/articles/s41567-019-0437-4).
5. Deiml and Peterseim, *Quantum Realization of the Finite Element Method*. [Paper](https://arxiv.org/abs/2403.19512).
6. Bravo-Prieto et al., *Variational Quantum Linear Solver*, Quantum (2023). [Paper](https://quantum-journal.org/papers/q-2023-11-22-1188/).
7. Harrow, Hassidim, and Lloyd, *Quantum algorithm for solving linear systems of equations*. [Paper](https://arxiv.org/abs/0811.3171).
8. Gilyén et al., *Quantum singular value transformation and beyond*. [Paper](https://arxiv.org/abs/1806.01838).
9. *Digital Quantum Simulation of Wavepacket Correlations in a Chemical Reaction*, Entropy (2026). [Paper](https://doi.org/10.3390/e28020144). The reported circuit results use classical simulation [NUM].
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
23. Lee and Jun, *Quantum-assisted tomographic image refinement with limited qubits for high-resolution imaging*, EPJ Quantum Technology (2026). [Paper](https://doi.org/10.1140/epjqt/s40507-026-00545-4). Regional D-Wave hybrid CT reconstruction; proposed electron-tomography extension requires target-specific validation.
24. *Solving complex nanostructures with ptychographic atomic electron tomography*, Nature Communications (2023). [Paper](https://doi.org/10.1038/s41467-023-43634-z). Electron-imaging and classical reconstruction baseline [C].
25. *Ptychography of pure quantum states*, Scientific Reports (2019). [Paper](https://doi.org/10.1038/s41598-019-52415-y). Quantum-state tomography through overlapping projections.
26. Granet et al., *Spectral functions on a quantum computer through system-environment interaction* (2026 preprint). [Paper](https://arxiv.org/abs/2605.01440). Reports a 27-site, 54-qubit Quantinuum H2 model demonstration.
27. *Computational framework chinook for angle-resolved photoemission spectroscopy*, npj Quantum Materials (2019). [Paper](https://doi.org/10.1038/s41535-019-0194-8). Classical photoemission matrix-element framework.
28. Kunitsa et al., *Quantum simulation of electron energy loss spectroscopy for battery materials*, Journal of Chemical Physics (2025). [Paper](https://doi.org/10.1063/5.0300557), [preprint](https://arxiv.org/abs/2508.15935). Quantum DSF algorithm and fault-tolerant resource estimates.
29. Seetharam et al., *Digital quantum simulation of NMR experiments*, Science Advances (2023). [Paper](https://doi.org/10.1126/sciadv.adh2594), [preprint](https://arxiv.org/abs/2109.13298). Four-qubit trapped-ion zero-field NMR demonstration.
30. *Quantum Computation of Molecular Structure Using Data from Challenging-To-Classically-Simulate Nuclear Magnetic Resonance Experiments*, PRX Quantum (2022). [Paper](https://doi.org/10.1103/PRXQuantum.3.030345), [preprint](https://arxiv.org/abs/2109.02163). Quantum-assisted inference of spin-Hamiltonian parameters.
31. Zhai et al., *Generalised quantum computational spectroscopy on a quantum chip*, Nature Communications (2026). [Paper](https://doi.org/10.1038/s41467-026-74936-7). Ancilla-assisted autocorrelation spectroscopy demonstrated on a silicon-photonic processor.
32. Rudnicki et al., *Fundamental quantum limits in ellipsometry*, Optics Letters (2020). [Paper](https://doi.org/10.1364/OL.392955), [preprint](https://arxiv.org/abs/2007.10440). Quantum sensing analysis of ellipsometric measurement precision.
33. Koopman and Grimme, *From QCEIMS to QCxMS: A Tool to Routinely Calculate CID Mass Spectra Using Molecular Dynamics*, Journal of the American Society for Mass Spectrometry (2021). [Paper](https://doi.org/10.1021/jasms.1c00098). Classical quantum-chemical fragmentation simulation.
34. Tsai, Nuckels, and Wang, *Integrating Quantum Computing into De Novo Metabolite Identification*, Journal of Systemics, Cybernetics and Informatics (2023). [Paper](https://iiisci.org/Journal/PDV/sci/pdfs/ZA381UC23.pdf). Algorithmic proposal for a selected metabolite-identification stage.
35. Kharazi et al., *Quantum Simulations for Extreme Ultraviolet Photolithography* (2026 preprint, version 2, revised 20 August 2026). [Paper](https://arxiv.org/abs/2602.20234v2).
36. *Gradient-based inverse extreme ultraviolet lithography*, Applied Optics (2015). [Paper](https://doi.org/10.1364/AO.54.007284). Classical inverse-EUV baseline incorporating optical and resist effects.

[Back to contents](#table-of-contents)

## Document version

| Edition | Date |
| --- | --- |
| 2026-10-05 | 5 October 2026 |

[Back to contents](#table-of-contents)
