Here is the step-by-step mathematical derivation of the **Deutsch-Jozsa algorithm**. 

As outlined in your study plan, this is where you learn the most fundamental trick in quantum computing: **Phase Kickback**.

### The Problem Setup
We are given a black-box function (an "oracle") $f$ that takes an $n$-bit string as input and outputs either a 0 or 1:
$$ f: \{0, 1\}^n \rightarrow \{0, 1\} $$

We are promised the function is either:
1. **Constant:** Outputs the same value (all 0s or all 1s) for every input.
2. **Balanced:** Outputs 0 for exactly half of all possible inputs, and 1 for the other half.

Our goal is to determine which type of function it is using exactly **one query** to the oracle.

---

### Step 1: Initialization
We start with two quantum registers. The first register has $n$ qubits (the input) initialized to $|0\rangle$. The second register has 1 qubit (the target) initialized to $|1\rangle$.

$$ |\psi_0\rangle = |0\rangle^{\otimes n} |1\rangle $$

Next, we apply a Hadamard gate ($H$) to every single qubit in both registers. 
* Applying $H^{\otimes n}$ to $|0\rangle^{\otimes n}$ creates an equal superposition of all possible $n$-bit strings ($x$).
* Applying $H$ to $|1\rangle$ creates the $|-\rangle$ state.

$$ |\psi_1\rangle = \left( \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} |x\rangle \right) \otimes \left( \frac{|0\rangle - |1\rangle}{\sqrt{2}} \right) $$

To make the math cleaner, let's just write the target qubit as $|-\rangle$:
$$ |\psi_1\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} |x\rangle |-\rangle $$

---

### Step 2: The Oracle Query (Phase Kickback)
Now we pass our state through the Quantum Oracle ($U_f$). The standard definition of a quantum oracle is that it adds the function's output to the target qubit using modulo-2 addition (XOR, denoted as $\oplus$):
$$ U_f(|x\rangle |y\rangle) = |x\rangle |y \oplus f(x)\rangle $$

Let's see what happens when we apply $U_f$ to our state $|\psi_1\rangle$. Let's look *only* at the target qubit $|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$ for a specific input $x$:

* **If $f(x) = 0$:** $y \oplus 0 = y$. The state remains $\frac{|0\rangle - |1\rangle}{\sqrt{2}}$.
* **If $f(x) = 1$:** $y \oplus 1$ flips 0 to 1, and 1 to 0. The state becomes $\frac{|1\rangle - |0\rangle}{\sqrt{2}}$, which is equal to **$-\left(\frac{|0\rangle - |1\rangle}{\sqrt{2}}\right)$**.

Mathematically, this means applying $U_f$ to $|x\rangle |-\rangle$ multiplies the state by $(-1)^{f(x)}$. This is **Phase Kickback**: the target qubit remains unchanged, but it "kicks" a negative phase back onto the input qubit!

Applying this to our entire superposition:
$$ |\psi_2\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} (-1)^{f(x)} |x\rangle |-\rangle $$

Since the target qubit $|-\rangle$ is no longer entangled with the input, we can ignore it for the rest of the algorithm. We will only look at the $n$-qubit input register:
$$ |\psi_2\text{ (input)}\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} (-1)^{f(x)} |x\rangle $$

---

### Step 3: The Second Hadamard (Interference)
To extract the answer, we apply a Hadamard gate to all $n$ qubits of the input register again ($H^{\otimes n}$). 

In quantum math, applying $H^{\otimes n}$ to a basis state $|x\rangle$ follows this exact formula:
$$ H^{\otimes n} |x\rangle = \frac{1}{\sqrt{2^n}} \sum_{z=0}^{2^n-1} (-1)^{x \cdot z} |z\rangle $$
*(Note: $x \cdot z$ is the bitwise dot product modulo 2. For example, if $x=101$ and $z=110$, $x \cdot z = (1\times1) \oplus (0\times1) \oplus (1\times0) = 1$.)*

Let's substitute this formula into our state $|\psi_2\rangle$:
$$ |\psi_3\rangle = \frac{1}{\sqrt{2^n}} \sum_{x=0}^{2^n-1} (-1)^{f(x)} \left( \frac{1}{\sqrt{2^n}} \sum_{z=0}^{2^n-1} (-1)^{x \cdot z} |z\rangle \right) $$

Multiply the constants ($\frac{1}{\sqrt{2^n}} \times \frac{1}{\sqrt{2^n}} = \frac{1}{2^n}$) and rearrange the sums so we group everything by the final measured state $|z\rangle$:
$$ |\psi_3\rangle = \sum_{z=0}^{2^n-1} \left[ \frac{1}{2^n} \sum_{x=0}^{2^n-1} (-1)^{f(x) + x \cdot z} \right] |z\rangle $$

The term inside the brackets is the **amplitude** (probability weight) of measuring any specific state $|z\rangle$.

---

### Step 4: Measurement and Analysis
We measure the $n$-qubit input register. Let's look specifically at the probability of measuring the all-zero state, meaning **$z = 00...0$**.

If $z = 0$, then $x \cdot z = 0$ for *every* possible $x$. 
So, the amplitude formula for the state $|00...0\rangle$ simplifies to:
$$ \text{Amplitude of } |00...0\rangle = \frac{1}{2^n} \sum_{x=0}^{2^n-1} (-1)^{f(x)} $$

Now, we evaluate this for our two possible types of functions:

#### Scenario A: The function is Constant
If the function is constant, $f(x)$ is the same for all $x$ (either all 0s or all 1s).
* If $f(x) = 0$, the sum is $\sum_{x=0}^{2^n-1} (+1) = 2^n$. The amplitude is $\frac{2^n}{2^n} = 1$.
* If $f(x) = 1$, the sum is $\sum_{x=0}^{2^n-1} (-1) = -2^n$. The amplitude is $\frac{-2^n}{2^n} = -1$.

In quantum mechanics, probability is the square of the amplitude ($| \pm 1 |^2 = 1$). 
**Result:** If the function is constant, we have a **100% probability** of measuring the state $|00...0\rangle$. Constructive interference destroyed all other possibilities.

#### Scenario B: The function is Balanced
If the function is balanced, exactly half of the inputs yield $f(x) = 0$ (producing $+1$) and the other half yield $f(x) = 1$ (producing $-1$).
* The sum is: $\left(2^{n-1} \times (+1)\right) + \left(2^{n-1} \times (-1)\right) = 0$.
* The amplitude is $\frac{0}{2^n} = 0$.

**Result:** If the function is balanced, we have a **0% probability** of measuring the state $|00...0\rangle$. Destructive interference completely eliminated the all-zero state. 

### Conclusion
By measuring the $n$-qubit input register:
* If you measure `00...0`, the function is **Constant**.
* If you measure **anything else**, the function is **Balanced**. 

You have figured out the nature of the function with just $1$ query, whereas a classical computer would have required $2^{n-1} + 1$ queries!