# qiskit-fall-fest-2021
This repository is used for competing in the UT Austin Qiskit Fall fest 2021\
 **Team Members:** Trisha Agrawal, James Larsen, Jordi Ramos, Erika Tan

## Our Project
Our project aims to teach quantum computing concepts by comparing random number generation in classical and quantum computers! We created a self-explanatory Python notebook with examples of how to generate random numbers and how the quantum circuits would look for different RNG methods. Our RNG methods are divided into separate stages: 

1. Pseudorandom number generation (classical computing)
2. Simple RNG by exploiting the properties of qubits (quantum computing)
3. More advanced RNG and verification of the stability of a quantum system through the CHSH game (quantum computing)

By honing into this simple concept, we hope that people will be able to not only learn important concepts such as superposition and qubits, but also apply them to a real world problem!

## Usage Guide
Our project is a self-explanatory Python notebook which can be run in Jupyter, Google Colab, etc. Our code cells are supplemented by text explanations and diagrams. Please refer to [`fall_fest.ipynb`](fall_fest.ipynb)

## Qiskit Implementation

We used the Qiskit package to implement a simple quantum circuit (Stage 1) and the CHSH game protocol (Stage 2). The CHSH game is an well-known non-local game in quantum information. The Qiskit textbook has some information on it but we still had to implement the game using mainly our own code.

This is the CHSH game protocol: Alice does nothing to her qubit if x = 0 and she applies a <img src="https://render.githubusercontent.com/render/math?math=\frac{\pi}{4}"> counterclockwise rotation towards |1⟩ if x = 1. Bob applies a <img src="https://render.githubusercontent.com/render/math?math=\frac{\pi}{8}"> counterclockwise rotation towards |1⟩ if y = 0 and he applies a <img src="https://render.githubusercontent.com/render/math?math=\frac{\pi}{8}"> *clockwise* rotation towards |1⟩ if y = 1. Alice and Bob both measure 
their qubits in the {|0⟩,|1⟩} basis and output whatever they see. 
This strategy wins <img src="https://render.githubusercontent.com/render/math?math=\cos^2\left(\frac{\pi}{8}\right)">(approximately 85%) of the time, 
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
Some team members had zero quantum computing experience before this hackathon, so we learned a lot about the basic concepts. One team member is currently in the intro QIS class in which he learned about the CHSH game. This project was a way for us to implement this nonlocal game for a practical use. We also learned how to use the Qiskit library.

Two of the most important quantum information concepts are superposition and entanglement. We were able to learn those concepts and apply it in our project. We learned that quantum information theory can be understood by us and be applied to our everyday life. We are glad that we were part of this hackathon and we all plan to be further involved with quantum computing.