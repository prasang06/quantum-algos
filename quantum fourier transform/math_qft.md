The **Quantum Fourier Transform (QFT)** is the quantum equivalent of the classical Discrete Fourier Transform (DFT). Its job is to take a state from the **computational basis** (where data is stored in $0$s and $1$s) and transform it into the **phase basis** (where data is stored in the angles/phases of superpositions).

Here is the step-by-step mathematical derivation, which beautifully reveals exactly how to build the circuit.

---

### Step 1: The Master Formula
Classically, a Fourier transform takes a list of numbers and extracts their frequencies. 
In quantum mechanics, the QFT acts on a quantum state $|x\rangle$ (an $n$-qubit integer ranging from $0$ to $2^n - 1$) and maps it to a superposition of all states $|y\rangle$, applying a very specific phase to each:

$$ QFT|x\rangle = \frac{1}{\sqrt{2^n}} \sum_{y=0}^{2^n-1} e^{2\pi i \frac{x y}{2^n}} |y\rangle $$

At first glance, this looks like a massive, tangled sum. If a classical computer tried to calculate this for 50 qubits, it would need to do $2^{50}$ calculations. But we can rewrite this sum into a **tensor product** to see how a quantum computer does it in just $O(n^2)$ steps.

---

### Step 2: Breaking down into Binary
To understand how this maps to individual qubits, we must express our inputs and outputs in binary.
*   Let the output $y$ be represented as binary bits: $y = y_1 y_2 \dots y_n$. 
    *(Mathematically: $y = y_1 2^{n-1} + y_2 2^{n-2} + \dots + y_n 2^0$)*
*   Let the input $x$ be represented as binary bits: $x = x_1 x_2 \dots x_n$.

We also need to introduce **binary fractions**. Just like $0.15$ in base-10 means $\frac{1}{10} + \frac{5}{100}$, a binary fraction $0.x_1 x_2$ means $\frac{x_1}{2} + \frac{x_2}{4}$.

---

### Step 3: Deriving the "Product Form" (The Magic Trick)
Let's substitute our binary definition of $y$ into the exponent of the Master Formula:
$$ e^{2\pi i \frac{x y}{2^n}} = e^{2\pi i x \left( \frac{y_1 2^{n-1} + y_2 2^{n-2} + \dots + y_n 2^0}{2^n} \right)} $$

Divide each term by $2^n$:
$$ = e^{2\pi i x \left( \frac{y_1}{2} + \frac{y_2}{4} + \dots + \frac{y_n}{2^n} \right)} $$

Because $e^{a+b} = e^a \times e^b$, we can split this massive exponent into individual pieces for each qubit $y_k$:
$$ = e^{2\pi i x \frac{y_1}{2}} \times e^{2\pi i x \frac{y_2}{4}} \times \dots \times e^{2\pi i x \frac{y_n}{2^n}} $$

Now, plug this factored exponent back into the Master Formula. Because the probabilities perfectly factor out, we can pull the sum apart into a **tensor product** ($\otimes$) of $n$ individual qubits:

$$ QFT|x\rangle = \frac{1}{\sqrt{2^n}} \bigotimes_{k=1}^{n} \left( |0\rangle + e^{2\pi i \frac{x}{2^k}} |1\rangle \right) $$

**This is the most important equation in Phase 2.** It tells us that the QFT doesn't entangle the output qubits! Every single output qubit is in its own independent superposition: $|0\rangle + \text{phase} |1\rangle$.

---

### Step 4: Simplifying the Phases
Let's look at the phase on a single output qubit: $e^{2\pi i \frac{x}{2^k}}$. 
Remember that $x$ is an integer ($x_1 x_2 \dots x_n$). When we divide $x$ by $2^k$, we are basically shifting the binary point to the left by $k$ places.

For example, if we divide $x$ by $2^1$ (which is $2$):
$ \frac{x}{2} = x_1 x_2 \dots x_{n-1} . x_n $

In complex math, $e^{2\pi i \times \text{Integer}} = 1$. It equals a full $360^\circ$ rotation, so the integer part just spins us in a complete circle and vanishes! **Only the decimal (fractional) part matters.**

So, for the first qubit, the phase is just the decimal part: $0.x_n$.
For the second qubit, the phase is $0.x_{n-1}x_n$.

Writing out the final state of all $n$ qubits from left to right, the QFT transforms $|x\rangle$ into:
$$ \frac{1}{\sqrt{2^n}} \left(|0\rangle + e^{2\pi i 0.x_n} |1\rangle\right) \otimes \left(|0\rangle + e^{2\pi i 0.x_{n-1}x_n} |1\rangle\right) \otimes \dots \otimes \left(|0\rangle + e^{2\pi i 0.x_1x_2\dots x_n} |1\rangle\right) $$

---

### Step 5: Turning the Math into a Circuit
How do we actually build this state on a quantum computer? We only need two gates:
1.  **Hadamard Gate ($H$):** Creates the $|0\rangle + |1\rangle$ superposition and applies the first bit of the phase ($0.x_k$).
2.  **Controlled-Phase Gate ($R_k$):** Adds the smaller binary fractions to the phase depending on the state of the other qubits. The rotation angle is $\frac{2\pi}{2^k}$.

Let's look at the very last qubit in our math equation: **$|0\rangle + e^{2\pi i 0.x_1 x_2 \dots x_n} |1\rangle$**

1.  **Apply $H$ to $q_1$:** It puts $q_1$ into a superposition. If $q_1$ started as `1`, it applies a negative sign (which is exactly a phase of $e^{2\pi i 0.x_1}$). The state is now $|0\rangle + e^{2\pi i 0.x_1}|1\rangle$.
2.  **Apply Controlled-$R_2$ from $q_2$ to $q_1$:** If $q_2$ is `1`, it adds $\frac{2\pi i}{4}$ to the phase. The phase is now $0.x_1 x_2$.
3.  **Apply Controlled-$R_3$ from $q_3$ to $q_1$:** If $q_3$ is `1`, it adds $\frac{2\pi i}{8}$. The phase is now $0.x_1 x_2 x_3$.
4.  **Repeat to $q_n$:** The phase becomes exactly $0.x_1 x_2 \dots x_n$.

You then move to $q_2$, apply an $H$ gate, and apply controlled-phase gates from $q_3 \dots q_n$. You repeat this until every qubit is processed.

*(Note: The physical circuit builds the mathematical equation in reverse order, so we usually apply a series of **SWAP gates** at the very end of the circuit to reverse the physical qubits so they match the mathematical output).*

### Why this is a Superpower
Look at the final output again. The original number $x$ is now entirely encoded in the **angles (phases)** of the qubits. 
*   The last qubit rotates halfway around the sphere for every $1$ added to $x$.
*   The second-to-last qubit rotates a quarter-way around.
*   The first qubit rotates a microscopic fraction of a degree.

If $x$ is a repeating pattern (a period), the QFT acts like a magnifying glass, causing constructive interference exactly at the frequency of that pattern, and destructive interference everywhere else.

This exact mathematical extraction of a repeating phase is how Peter Shor managed to factor RSA keys!