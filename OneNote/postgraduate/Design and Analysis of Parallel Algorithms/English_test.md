**Distributed-Memory Parallel Sorting**  
**Class Test Cheat Sheet (English Edition)**
 
**1. PRAM vs Distributed Memory (MPI)**  
PRAM assumptions:

- Shared memory
- No explicit communication
- Concurrent reads/writes resolved by the model
- Compare-exchange is trivial

Distributed memory (MPI):

- No shared memory
- All data must be explicitly communicated
- Must describe data distribution, communication partners, and final collection

**Whenever converting a PRAM algorithm to MPI, always answer:**

1. How are the inputs initially distributed?
2. How do processors communicate during the algorithm?
3. How is the final result collected or distributed?
 
**2. Compare-Split Operation (Core of Distributed Odd-Even Sort)**  
Steps:

1. Each rank holds n/p sorted items
2. Two neighboring ranks exchange their blocks (size n/p each)
3. Each merges the two sorted blocks (cost Θ(n/p))
4. The lower-ranked processor keeps the smaller half
5. The higher-ranked processor keeps the larger half

Parallel time of one step: **Θ(n/p)**  
Applied in each odd-even phase.
 
**3. Distributed Odd-Even Transposition Sort**  
Algorithm:

- First locally sort the n/p items on each rank
- Then perform **p rounds** of communication
- Odd rounds: odd ranks compare-split with right neighbor
- Even rounds: even ranks compare-split with right neighbor
- Each phase costs Θ(n/p)

Total parallel time:￼

![Exported image](Exported%20image%2020260730182312-0.png)  
![Exported image](Exported%20image%2020260730182312-1.png)  

**5. PRAM Compare-Exchange vs MPI Compare-Split**  
PRAM:

- Works on single elements
- No communication cost
- Accesses shared memory directly
- Constant-time operation

MPI:

- Works on whole blocks of size n/p
- Requires explicit communication
- Must merge sorted blocks
- Cost is Θ(n/p)
 
**6. Input Distribution (Typical Exam Question)**  
To run a distributed sorting algorithm, must specify:

- Whether input starts on rank 0
- Whether we need to scatter it
- Whether ranks start with contiguous blocks of size n/p
- Whether redistribution is needed before sorting
 
**7. Output Distribution (Another Typical Exam Question)**  
Must specify:

- Whether final sorted array is gathered to rank 0
- Or left distributed across ranks
- Or broadcast so all ranks get the full array
 
**8. Communication Complexity**  
For compare-split:

- Message size: n/p
- Each phase sends and receives n/p elements
- p phases → total communication Θ(n)

Bandwidth per phase:

![Exported image](Exported%20image%2020260730182313-2.png)  

**Final Quick-Memorization Summary**  
If time is short, memorize these lines:  
**Q1: Inputs?**  
Either already distributed, or must scatter from rank 0.  
**Q2: Communication?**  
Odd-even neighbor exchanges; send/recv n/p items; compare-split → merge → keep half.  
**Q3: Output?**  
Either gather to rank 0, broadcast, or keep distributed.  
**Cost:**  
T = Θ(n), C = Θ(pn), cost-optimal if p = O(log n).  
**compare-split cost:**  
Θ(n/p)