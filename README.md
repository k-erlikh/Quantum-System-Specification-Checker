# Quantum-System-Specification-Checker

## Overivew 

In system engineering, system specifications determine the conditions under which a system will be created. The requirements detail the constraints, functionality, and performance of a system. On an industry level, ensuring that a system complies with the specifications is essential to the engineers, clients, and users. Classical computers have methods of checking system requirements, but experience an exponential increase in run-time complexity as more variables are added [1]. Our study explores a proposed quantum algorithm outlined by Zidan, Eisa, Qasymeh, and Ismail Shoman, which produces an exponential runtime complexity speed-up with 8 or more specifications [1].

The proposed algorithm models system specifications requirements using Boolean functions, which can be visualized in truth tables. Each specification variable is represented with an $X_i$ value, where:

$X_i$ = <br>{ <br>
&nbsp;&nbsp;&nbsp; $X_i$ = 0 <br>
&nbsp;&nbsp;&nbsp; $X_i$ = 1 <br>
}

A Boolean function can have either 2 or 3 conditions, defining a set of specification requirements. An example of a compliant system with its corresponding truth table can be observed in Table 1, where each variable is indicated with $X_i$ and each Boolean function with an $F_i$ variable. The last column denotes if the system is compliant, given the following $X$ values. A non-compliant system can be observed in Table 2, where no combination of $X_0…X_i$ values can be True.

### Truth table example for a consistent system with 3 variables and 2 functions  
**F₁ = ¬X₀X₂**, **F₂ = ¬X₀¬X₁**

| X0 | X1 | X2 | F1 | F2 | Bool  |
|----|----|----|----|----|-------|
| 0  | 0  | 0  | 0  | 1  | False |
| 0  | 0  | 1  | 1  | 1  | True  |
| 0  | 1  | 0  | 0  | 0  | False |
| 0  | 1  | 1  | 1  | 0  | False |
| 1  | 0  | 0  | 0  | 0  | False |
| 1  | 0  | 1  | 0  | 0  | False |
| 1  | 1  | 0  | 0  | 0  | False |
| 1  | 1  | 1  | 0  | 0  | False |


Using the Boolean model to represent system specifications, the paper proposes algorithms for both the classical and quantum methods. The authors also discuss the time complexity of each algorithm, represented in Table 3 [1].

### Comparison of the complexity of the classical and quantum algorithms

| Algorithm            | Complexity |
|----------------------|------------|
| Classical algorithm  | O(m·2ⁿ)    |
| Quantum algorithm    | O(m·e⁻²)   |


Firstly, let's explore the classical algorithm. The $m$ value represents the number of Boolean functions being evaluated. The $n$ value represents the number of specification variables in the system. As mentioned earlier, the complexity of the classical algorithm increases exponentially with the number of variables in the system. A significant performance slowdown is observed as more specification options are added. The poor performance of this classical algorithm allows room for a new idea: using the features of quantum algorithms to solve the same problem with higher efficacy. 

Now, we will discuss the quantum algorithm. In Table 3., the quantum algorithm relies on two variables: $m$ being the number of boolean functions, and $ϵ$ being the error tolerance. The error tolerance value is determined by the number of runs $M$, also known as shots, required to measure the concurrence $C$ value. For the quantum algorithm results to be valid, they must be within the determined error tolerance for that number of shots $M$. Later, we will explain how the concurrence is used to evaluate the consistency of a system with $m$ Boolean functions. The error tolerance is calculated using the relation:

### Error Model


M = (1/2) ϵ²


The study uses a set of three *M* values, also known as shots: 64, 128, and 256 [1].  
The corresponding error values for each shot are:

- 64  → 0.0884  
- 128 → 0.0625  
- 256 → 0.0442  

These values are used during our theoretical and experimental runs using the quantum circuit and for further analysis presented in the Results section.

### Virtual Concurrence Measurement

The virtual concurrence *C* is a measurement used to determine whether the system is compliant or non-compliant. It is evaluated using an *M* operator, which requires qubits:


| $a_1$ $a_2$⟩


and a decoupled replica:


| $a_3$ $a_4$⟩


Each set of qubits is entangled using a CNOT gate, and then both  
| $a_1$ $a_2$⟩ and | $a_3$ $a_4$⟩ are measured.

In a theoretical simulation, this yields four quantum states:


|0000⟩, |0011⟩, |1100⟩, |1111⟩


Once measured, the probabilities P of these states are computed and processed using:

C = sqrt(2( $P_{0011}$ + $P_{1100}$))

This equation provides the concurrence for the quantum circuit.

The system is **consistent** if all of the following conditions are met:

1. C > 0  
2. $P_{0011}$ ≠ 0 and $P_{1100}$ ≠ 0

Otherwise, the system is inconsistent.

### Quantum Circuit Construction (Generalized Algorithm)

Following the proposed algorithm by the authors [1], the quantum circuit is constructed as follows:

1. **Initialize register**

   A quantum register of *n* qubits is prepared. A Hadamard gate is applied to each qubit to place it in superposition.

2. **Prepare Boolean function registers**

   Prepare *m* quantum registers, each representing a Boolean function.

   - Each function is implemented as an oracle $U_{f_i}$, where i = 1 * m.
   - If the Boolean function has **two conditions**, a Toffoli (CCX) gate is used.
   - If the Boolean function has **three conditions**, a multi-controlled X (MCX) gate is used.
   - Control qubits correspond to the *n* qubits.
   - The target is applied to the corresponding *m* qubit.

   The referenced paper discusses the computational advantages of using CCX and MCX gates[1].

3. **Prepare ancillary registers**

   Prepare:
   - an *aux* register (used for optimization),
   - an *a1* register,
   - an *a2* register.

   These registers are used within the measurement oracle $M_z$.

4. **Apply multi-controlled operations**

   - Apply a multi-controlled X gate to all *m* qubits, targeting the *aux* qubit.
   - Apply a controlled Hadamard gate where:
     - control = *aux*
     - target = *a1*

5. **Entangle measurement qubits**

   Apply a CNOT gate between *a1* and *a2*.

6. **Create decoupled replica**

   Repeat steps 1–5 with a decoupled replica (required for the $M_z$ oracle).

7. **Measurement**

   Measure:
   - *a1*
   - *a2*
   - mirrored *a1*
   - mirrored *a2*

   using a classical register.

---

The classical register outputs the required quantum states, which are then substituted into the concurrence equation to determine system consistency.

## Requirements and Files 

Requires: Python 3.12.1 <br> Recommended system: Linux systems due to some Linux-specific Python libraries

To install all dependencies, run: ```pip install -r requirements.txt```

### Classical Solution: 
- ```src/classic_check.ipynb```

### Quantum Solution:
- ```src/quantum_check.ipynb```


### Examples of classical and quantum algorithms:

- ``` src/examples.ipyn```: Contains general examples for both classical and quantum solutions with diagrams
- quantum_check.ipyn 

### Tests:

```src/test_time_complexity.ipynb```: Tests the runtime complexities of the classical and quantum solution

```src/test_theoretical_valies.ipynb```: Produces data for running the quantum solution on the AerSimulator

```src/test_experimental_values.ipynb```: Produces data for running the quantum solution on a fake backend, and produces graphs comparing the experimental vs. theoretical data

All data csv files produced can be found in ```src/data```. Figures generated can be found in ```figures/theoretical_figures```, ```figures/circuit_diagrams```, ```figures/time_complexity``` and ```figures/experimental_figures```

## References
[1] M. Zidan, A. M. Eisa, M. Qasymeh, and M. A. I. Shoman, “A quantum algorithm for
system specifications verification,” IEEE Internet of Things Journal, vol. 11, no. 14,
pp. 24775–24794, 2024.
