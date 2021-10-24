# qiskit-fall-fest-2021
This repository is used for competing in the UT Austin Qiskit Fall fest 2021\
 **Team Members:** Trisha Agrawal, Erika Tan, James Larsen, Jordi Ramos

## Our Project

## Usage Guide

## Qiskit Implementation

We used the Qiskit package to implement the CHSH game protocol. The CHSH game is an well-known non-local game in quantum information. The Qiskit textbook has some information on it but we still had to implement the game using mainly our own code.

This is the CHSH game protocol: Alice does nothing to her qubit if <img src="https://render.githubusercontent.com/render/math?math=x=0"> and she applies a <img src="https://render.githubusercontent.com/render/math?math=\frac{\pi}{4}"> counterclockwise rotation towards <img src="https://render.githubusercontent.com/render/math?math=|1\rangle"> if <img src="https://render.githubusercontent.com/render/math?math=x=1">. Bob applies a <img src="https://render.githubusercontent.com/render/math?math=\frac{\pi}{8}"> counterclockwise rotation towards <img src="https://render.githubusercontent.com/render/math?math=|1\rangle"> if <img src="https://render.githubusercontent.com/render/math?math=y=0"> and he applies a <img src="https://render.githubusercontent.com/render/math?math=\frac{\pi}{8}"> *clockwise* rotation towards <img src="https://render.githubusercontent.com/render/math?math=|1\rangle"> if <img src="https://render.githubusercontent.com/render/math?math=y=1">. Alice and Bob both measure 
their qubits in the <img src="https://render.githubusercontent.com/render/math?math=\{|0\rangle,|1\rangle\}"> basis and output whatever they see. 
This strategy wins <img src="https://render.githubusercontent.com/render/math?math=\cos^2\left(\frac{\pi}{8}\right) \approx 85\%"> of the time, 
while any classical strategy can win with probability of at most <img src="https://render.githubusercontent.com/render/math?math=\frac{3}{4}">

After carefully verifying the algorithm by hand, we decided to implement a function where apply apply a Hadamard gate and a CNOT gate to two qubits so that they are entangled, and we will perform a rotation gate on either of the two qubits based on the inputs.

This is the code we used for implementing the circuit:
```
def make_chsh(x, y):
    """Create quantum circuit for CHSH protocol.
      input -- x: the x input for the CHSH non-local game
               y: the y input for the CHSH non-local game

      output -- qc: CHSH circuit created
    """
    # initialize quantum circuit with 2 qubits and 2 classical bits
    qc = QuantumCircuit(2,2)

    # apply Hadamard gate and control-not
    qc.h(0)
    qc.cx(0, 1)
    
    # apply conditional rotations
    if x == 1:
        qc.ry(np.pi/2, 0)
    if y == 0:
        qc.ry(np.pi/4, 1)
    elif y == 1:
        qc.ry(-np.pi/4, 1)
    qc.measure(range(2),range(2))
    
    return qc
```
This is what the circuit looks like when x = 1 and y = 1 :

![](pictures/CHSH.png)


## What We Learned