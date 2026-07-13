# ⚛️ Quantum Algorithms Study Tracker

Welcome to my Quantum Computing journey! This repository serves as a tracker and codebase for my progress in learning, deriving, and implementing the major quantum algorithms. 

The study plan is structured sequentially: moving from foundational concepts (like phase kickback) to master subroutines (like QFT), and finally scaling up to NISQ and Fault-Tolerant applications.

---

## 🛠️ Prerequisites
Before diving into the algorithms, these mathematical and conceptual foundations must be solidified:
- [ ] **Linear Algebra:** Vectors, matrices, eigenvalues/eigenvectors, tensor products.
- [ ] **Complex Numbers:** Arithmetic, polar coordinates, Euler's formula.
- [ ] **Dirac Notation:** Bra-ket notation ($|\psi\rangle$).
- [ ] **Basic Quantum Gates:** Pauli (X, Y, Z), Hadamard (H), Phase (S, T), and CNOT.
- [ ] **Setup Environment:** Install [Qiskit](https://qiskit.org/) or [PennyLane](https://pennylane.ai/).

---

## 🗺️ The Roadmap

### Phase 1: The Foundations (Phase Kickback & Interference)
*Learning how quantum computers use interference to eliminate wrong answers and amplify correct ones.*
- [ ] **1. Deutsch-Jozsa Algorithm**
  - [ ] Understand the "Constant vs. Balanced" problem.
  - [ ] Grasp the concept of Phase Kickback.
  - [ ] Write and simulate the circuit.
- [ ] **2. Bernstein-Vazirani Algorithm**
  - [ ] Understand how it extracts a hidden bitstring in one query.
  - [ ] Write and simulate the circuit.
- [ ] **3. Simon’s Algorithm**
  - [ ] Understand the Hidden Subgroup Problem and period finding.
  - [ ] Prove the exponential speedup concept.
  - [ ] Write and simulate the circuit.

### Phase 2: The Core Engines (Master Subroutines)
*The fundamental mathematical engines that power almost all advanced quantum algorithms.*
- [ ] **4. Quantum Fourier Transform (QFT)**
  - [ ] Understand the math translating the computational basis to the phase basis.
  - [ ] Build a 3-qubit QFT circuit from scratch.
- [ ] **5. Quantum Phase Estimation (QPE)**
  - [ ] Understand how QPE uses inverse-QFT to find eigenvalues.
  - [ ] Implement a QPE circuit to estimate the phase of a simple gate (e.g., T-gate).

### Phase 3: The Crown Jewels
*The algorithms that put quantum computing on the map.*
- [ ] **6. Shor’s Algorithm (Factoring)**
  - [ ] Understand the classical reduction from factoring to period finding.
  - [ ] Implement modular exponentiation.
  - [ ] Factor a small number (e.g., 15) using a simulator.
- [ ] **7. Grover’s Algorithm (Search)**
  - [ ] Understand the Oracle and the Diffusion Operator.
  - [ ] Grasp the geometry of Amplitude Amplification.
  - [ ] Search a small unstructured database (e.g., 2 or 3 qubits).
- [ ] **8. Quantum Counting**
  - [ ] Combine Grover's Oracle with QPE.
  - [ ] Implement a circuit to count the number of solutions without finding them.

### Phase 4: Simulating Nature
*Feynman's original vision: using quantum systems to simulate quantum systems.*
- [ ] **9. Hamiltonian Simulation (Trotterization)**
  - [ ] Understand the Schrödinger equation and time evolution.
  - [ ] Implement first-order Trotter-Suzuki decomposition.
- [ ] **10. Variational Quantum Eigensolver (VQE)**
  - [ ] Understand the hybrid quantum-classical loop.
  - [ ] Learn about Ansätze (parameterized circuits) and objective functions.
  - [ ] Find the ground state energy of a simple molecule (e.g., $H_2$).

### Phase 5: Near-Term Optimization (NISQ Era)
*Solving business and logistics problems on today's noisy quantum hardware.*
- [ ] **11. Quantum Approximate Optimization Algorithm (QAOA)**
  - [ ] Map a classical optimization problem (e.g., MaxCut) to an Ising Hamiltonian.
  - [ ] Implement the alternating cost and mixer layers.
  - [ ] Solve a small graph problem.
- [ ] **12. Variational Quantum Classifiers (VQC)**
  - [ ] Implement classical-to-quantum data encoding (feature maps).
  - [ ] Train a parameterized circuit to classify a simple dataset (e.g., Iris dataset).

### Phase 6: Fault-Tolerant Data & Linear Algebra
*The distant future of AI and Big Data, requiring error-corrected qubits.*
- [ ] **13. Harrow-Hassidim-Lloyd (HHL) Algorithm**
  - [ ] Understand how it solves linear equations ($Ax = b$).
  - [ ] Combine QPE, controlled rotations, and inverse-QPE.
  - [ ] Implement a 2x2 HHL toy model.
- [ ] **14. Quantum Principal Component Analysis (QPCA)**
  - [ ] Understand density matrices as data representations.
  - [ ] Study the theoretical speedup over classical PCA.
- [ ] **15. Quantum Support Vector Machines (QSVM)**
  - [ ] Understand the "Quantum Kernel Trick".
  - [ ] Implement a QSVM to separate non-linear classical data.

### Phase 7: Advanced Topologies (Graph Theory)
*Navigating spatial and network complexities.*
- [ ] **16. Quantum Random Walks**
  - [ ] Understand the Coin and Shift operators.
  - [ ] Implement a discrete-time quantum walk on a line.
- [ ] **17. Element Distinctness (Ambainis' Algorithm)**
  - [ ] Understand how quantum walks yield the $O(N^{2/3})$ speedup for finding duplicates.

---

## 📚 Resources & References
* **Textbook:** [Quantum Computation and Quantum Information (Nielsen & Chuang)](https://en.wikipedia.org/wiki/Quantum_Computation_and_Quantum_Information) - *The standard Bible of QC.*
* **Interactive:** [IBM Qiskit Course](https://learning.quantum.ibm.com/)
* **Machine Learning:** [Xanadu PennyLane Demos](https://pennylane.ai/qml/demonstrations/)

---
*Tracker inspired by the pedagogical sequence of quantum information science.*