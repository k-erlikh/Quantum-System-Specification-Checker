# Quantum-System-Specification-Checker
Quantum vs. Classical solution for checking system specifications

Requires: Python 3.12.1

Use ```pip install -r requiremnets.txt``` to install all dependencies.

Classical Solution: 

- ```classic_check.py```
- ```classic_check.ipynb```
  

Quantum Solution:

- ```quantum_check.py```
- ```quantum_check.ipynb```

Examples:

```examples.ipyn```: Contains general examples for both classical and quantum solutions with diagrams
- quantum_check.py
- quantum_check.ipyn

Testing:

```test_time_complexity.ipynb```: Tests the rutnime complexities of the classical and quantum solution

```test_theoretical_valies.ipynb```: Produces data and for running the quantum solution on the AerSimulator

```test_experimental_values.ipynb```: Produces data for running the quantum solution on a fake backend, and produces graphs comparing the experimental vs. theoretical data

All data csv files produced can be found in ```src/data```. Figures generated can be found in ```figures/theoretical_figures```, ```figures/circuit_diagrams``` and ```figures/experimental_figures```.

- test_time-complexity.ipynb
- test_theoretical_values.ipynb
- test_experimental_values.ipynb