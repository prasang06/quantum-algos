This is the **Bernstein-Vazirani (BV) Algorithm**, introduced in 1992. 

### The Background: Classical vs. Quantum
Imagine a black-box function (an oracle) that hides a secret string of $n$ bits, which we will call **$s$** (e.g., $s = 1011$). 

Every time you input an $n$-bit string ($x$) into the box, the box calculates the **bitwise dot product** of your input $x$ and the secret string $s$, modulo 2, and returns the result (either a 0 or 1).

$$ f(x) = s \cdot x \pmod 2 $$
*(In binary, this means $f(x) = (s_0 \times x_0) \oplus (s_1 \times x_1) \oplus \dots \oplus (s_{n-1} \times x_{n-1})$)*

**The Classical Approach:**
To find a hidden $n$-bit string, a classical computer must query the box $n$ times. To find the first bit, you input $1000...$; to find the second, you input $0100...$; and so on. 

**The Quantum Approach:**
Using the Bernstein-Vazirani algorithm, a quantum computer can find the entire secret string $s$ in exactly **1 query**, no matter how long the string is.

Here is the step-by-step mathematical derivation.

---

### Step 1: Initialization
The setup is identical to the Deutsch-Jozsa algorithm. We have an $n$-qubit input register initialized to $|0\rangle^{\otimes n}$ and a 1-qubit target register initialized to $|1\rangle$.

$$ |\psi_0\rangle = |0\rangle^{\otimes n} |1\rangle $$

We apply a Hadamard gate ($H$) to every qubit. The input register becomes an equal superposition of all possible $n$-bit strings, and the target qubit becomes the $|-\rangle$ state.

$$ |\psi_1\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} |x\rangle \otimes |-\rangle $$

---

### Step 2: The Oracle Query (Phase Kickback)
We pass the state through the quantum oracle $U_f$. Just like in Deutsch-Jozsa, the oracle adds $f(x)$ to the target qubit:
$$ U_f(|x\rangle |y\rangle) = |x\rangle |y \oplus f(x)\rangle $$

Because our target qubit is in the $|-\rangle$ state, the exact same **Phase Kickback** phenomenon occurs. The value of $f(x)$ becomes a phase on the $|x\rangle$ states:
$$ U_f(|x\rangle |-\rangle) = (-1)^{f(x)} |x\rangle |-\rangle $$

Substitute the definition of our specific function $f(x) = s \cdot x$:
$$ |\psi_2\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} (-1)^{s \cdot x} |x\rangle |-\rangle $$

The target qubit $|-\rangle$ has done its job. We can discard it and focus only on the $n$-qubit input register:
$$ |\psi_2\text{ (input)}\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} (-1)^{s \cdot x} |x\rangle $$

*Stop and look at this state: The secret string $s$ is now perfectly encoded into the **phases** (positive or negative signs) of the superposition!*

---

### Step 3: The Second Hadamard (Interference)
To pull $s$ out of the phases and into a state we can measure, we apply a Hadamard gate to all $n$ qubits of the input register again ($H^{\otimes n}$).

Remember the master formula for applying $H^{\otimes n}$ to any state $|x\rangle$:
$$ H^{\otimes n} |x\rangle = \frac{1}{\sqrt{2^n}} \sum_{z=0}^{2^n-1} (-1)^{x \cdot z} |z\rangle $$

Let's apply this formula to our current state $|\psi_2\rangle$:
$$ |\psi_3\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} (-1)^{s \cdot x} \left( \frac{1}{\sqrt{2^n}} \sum_{z=0}^{2^n-1} (-1)^{x \cdot z} |z\rangle \right) $$

Multiply the constants ($\frac{1}{\sqrt{2^n}} \times \frac{1}{\sqrt{2^n}} = \frac{1}{2^n}$) and combine the exponents. Since we are dealing with base $-1$, multiplying $(-1)^{s \cdot x}$ by $(-1)^{x \cdot z}$ allows us to add the exponents: $(s \cdot x) \oplus (x \cdot z)$. 
By factoring out the $x$, this simplifies to $x \cdot (s \oplus z)$.

Rearranging the sums to group by the final states $|z\rangle$, we get:
$$ |\psi_3\rangle = \sum_{z=0}^{2^n-1} \left[ \frac{1}{2^n} \sum_{x=0}^{2^n-1} (-1)^{x \cdot (s \oplus z)} \right] |z\rangle $$

The term in the brackets is the **probability amplitude** for measuring any specific bitstring $z$.

---

### Step 4: Measurement and Perfect Interference
Let's figure out what state we are actually going to measure by looking at the inner sum:
$$ \text{Inner Sum} = \sum_{x=0}^{2^n-1} (-1)^{x \cdot (s \oplus z)} $$

There are two scenarios for the string $z$ that we might measure:

#### Scenario A: We measure the exact secret string ($z = s$)
If $z$ happens to equal the secret string $s$, then $s \oplus z = 00...0$ (because any binary string XOR'd with itself is 0). 
If $s \oplus z = 0$, then $x \cdot 0 = 0$ for *every* possible $x$.
The inner sum becomes:
$$ \sum_{x=0}^{2^n-1} (-1)^0 = \sum_{x=0}^{2^n-1} 1 = 2^n $$
The total amplitude for the state $|s\rangle$ is $\frac{1}{2^n} \times 2^n = \mathbf{1}$.

Since $|1|^2 = 1$, there is a **100% probability** of measuring the state $|s\rangle$.

#### Scenario B: We measure any other string ($z \neq s$)
If $z$ is *not* equal to the secret string $s$, then $s \oplus z$ results in a non-zero bitstring. 
When you take the dot product of all possible $x$'s with a non-zero bitstring, exactly half of the dot products will result in $0$ (yielding $+1$), and the other half will result in $1$ (yielding $-1$).
The inner sum becomes:
$$ \left(2^{n-1} \times (+1)\right) + \left(2^{n-1} \times (-1)\right) = \mathbf{0} $$
The total amplitude for any wrong state is $\frac{1}{2^n} \times 0 = \mathbf{0}$.

### The Conclusion
Because of **perfect constructive interference**, the amplitude of the correct secret string $s$ amplifies to $1$. Because of **perfect destructive interference**, the amplitudes of every single incorrect string cancel out to $0$.

When you measure the input register at the end of the circuit, you will read out exactly the secret string **$s$**, having queried the oracle only **1 time**. 

### Why this matters for your progress:
Notice how the circuit for Bernstein-Vazirani is *literally identical* to the circuit for Deutsch-Jozsa! 
1. Initialize to $|0\rangle^{\otimes n} |1\rangle$
2. Hadamards on all
3. Oracle
4. Hadamards on input register
5. Measure

The only difference is the math happening *inside* the Oracle. This teaches a vital lesson in quantum computing: **Hadamard sandwiches ($H \rightarrow U_f \rightarrow H$) are the universal tool for turning hidden mathematical patterns into measurable states.** 
