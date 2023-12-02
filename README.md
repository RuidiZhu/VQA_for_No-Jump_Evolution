# Variational Quantum Algorithms for No-Jump Evolution

Quantum computers hold promise to solve certain problems faster and more efficiently than classical computers. However, building a large quantum computer is a very challenging task, and one of the most significant challenges is the presence of noise and decoherence. Due to the large levels of noise, limited numbers of qubits and limited connectivity in the current quantum devices, the availability of fault-tolerant quantum computers still appears to be at least many years or even decades away, preventing us from implementing those famously proposed quantum algorithms, such as Shor's algorithm, Hamiltonian simulations and HHL algorithm, to a usefully large scale. A key technological question is therefore how to make best use of today’s noisy intermediate-scale quantum (NISQ) devices to achieve quantum advantage and perform useful applications.

Variational quantum algorithms(VQAs) have emerged as a leading strategy to obtain near-term advantage on NISQ devices. VQAs are hybrid quantum-classical algorithms, which usually involve using parameterized quantum circuits to compute some cost functions $C(\vec{\theta})$ (which are generally difficult to compute classically), but parameters $\vec{\theta}$ are optimized using a classical computer. This approach has the advantage of keeping the quantum circuit depth shallow and hence mitigating noise without the requirement of fault-tolerant schemes. VQAs are arguably the quantum analogue of those highly successful machine learning methods, such as neural networks.

The most famous example of VQAs is the so-called variational quantum eigensolver (VQE), which can be used to find the ground state of a given Hamiltonian. But VQA is actually a much broader class of algorithms than VQE, with a diverse range of applications, such as dynamical quantum simulation, principal component analysis and acceleration of quantum compilation.

In this tutorial, I will use VQA to find the optimized quantum circuits to mitigate the unwanted no-jump evolution that appeared in the dual-rail encodings of superconducting qubits. I will numerically simulate the parameterized quantum circuits in the presence of the no-jump evolution, and find the parameters that minimize the infidelity of the circuits with the help of the classical optimizers.

The plan of the tutorial is as follows: First, I will introduce the phenomena of no-jump evolution that occurs in superconducting dual-rail qubits. Then, I will briefly explain the gates and circuits that can be implemented in superconducting dual-rail qubits. Afterwards, I will outline the structure of VQA and demonstrate a simple example of applying VQA to optimize a simple circuit in the presence of no-jump evolution. Finally, I will demonstrate how to use VQA to optimize a more complicated circuit.

The work presented in this tutorial was inspired by the paper [[K. Heya, et al. 2018]](https://arxiv.org/pdf/1810.12745.pdf), but the idea and circuit implementation of this work are mostly original, and unparalleled by existing literature on this topic.

## A Brief Introduction to Superconducting Dual-rail Qubits

Superconducting qubits are regarded as one of the leading candidates for quantum computing hardware due to many advantages, such as strong coupling between qubits, fast gate operations and good scalability. However, as a trade-off of the strong couplings, it is difficult to isolate the superconducting qubits from the external environment, resulting in decoherence -- one of the major issues with superconducting qubits. Therefore, it is natural to think of modifying the hardware architecture to have longer qubit coherence time and better error correction capabilities. Superconducting microwave cavities are attractive candidates for hardware-efficient error correction. Microwave cavity resonators typically have much longer lifetimes (up to $\sim2s$) than the traditional nonlinear superconducting qubits containing Josephson junctions ($\sim300 \mu s$).

<img width="481" alt="dual_rail" src="https://github.com/Superrudder/VQA-for-No-Jump-Evolution/assets/86409906/8dc92a65-4751-48fb-96f6-79b0d94167ad">

As shown in the picture, the dual-rail superconducting qubit is composed of two superconducting cavities, which are coupled by a switchable beamsplitter interaction (i.e. linear operations just like those achieved in linear optics). A single transmon ancilla is dispersively coupled to one of the cavities and acts as a resource for non-Gaussian operations (i.e. nonlinear operations that allow one to have universal gate sets).

The dual-rail superconducting qubits presented here have a similar encoding to that used in linear quantum optics platforms [[I. Chuang, et al. 1995]](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.52.3489), but with access to a set of quantum non-demolition (QND) measurements (i.e. measuring the state without destroying the logical photons) and nonlinearity controls afforded by the circuit quantum electrodynamics (cQED) toolkit.

The work presented in this tutorial will be based on this superconducting dual-rail encoding.

## Python Package
I will use QuTip package extensively in this simulation. Make sure you install the QuTip package:
```
    pip install qutip
```

## Acknowledgement

I thank for stimulating discussion with Takahiro Tsunoda (postdoc in Robert Scheokolf's group).




