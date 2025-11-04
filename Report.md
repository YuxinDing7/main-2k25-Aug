## 2.1

The MW7 problem is mathematically formulated as follows:

$$
\begin{aligned}
&\text{minimize }
\begin{cases}
f_1(\mathbf{x}) = g_3(\mathbf{x}) x_1, \
f_2(\mathbf{x}) = g_3(\mathbf{x}) \sqrt{1 - (f_1(\mathbf{x}) / g_3(\mathbf{x}))^2}
\end{cases} \
&\text{subject to: }
\begin{cases}
c_1(\mathbf{x}) = (1.2 + 0.4 \sin(4l)^{16})^2 - f_1^2 - f_2^2 \ge 0, \
c_2(\mathbf{x}) = f_1^2 + f_2^2 - (1.15 - 0.2 \sin(4l)^8)^2 \ge 0,
\end{cases} \
&\text{where } g_3(\mathbf{x}) = 1 + \sum_{i=m}^n 2(x_i + (x_{i-1}-0.5)^2 - 1)^2, \quad l = \arctan(f_2/f_1), \
&0 \le x_i \le 1.
\end{aligned}
$$

### 2.1.1 Experimental Principles

The NSGA-II algorithm consists of several key steps. First, through Fast Non-Dominated Sorting, the population is divided into different levels of non-dominated fronts, allowing each individual to be ranked according to its dominance relationship in the population. Then, Crowding Distance is calculated to maintain population diversity and prevent premature convergence to local optima. Finally, Tournament Selection is used to select parents based on individual rank and crowding distance, providing candidates for crossover and mutation operations.

For constraint handling, this experiment employs the Parameter-less Constraint Handling method. The core idea of this method is to prioritize feasibility. Specifically, when comparing two individuals, a feasible individual is considered superior to an infeasible one; if both are infeasible, the individual with a smaller constraint violation is preferred; and if both are feasible, the traditional Pareto dominance relationship is used for comparison. This method does not require any penalty coefficients and can adaptively balance feasibility and optimality, improving algorithm performance on complex constrained problems.

In terms of genetic operators, the experiment uses Simulated Binary Crossover (SBX) and Polynomial Mutation. The crossover probability of SBX is set to 0.9, with a distribution index of 20, to generate new candidate solutions from parents. The mutation probability of Polynomial Mutation is set to 0.1, with the same distribution index, to introduce random perturbations and enhance exploration of the search space. These operations together ensure that the algorithm can explore new solution areas while exploiting high-quality solutions for optimization.

### 2.1.2 Experimental Settings

| Parameter                               | Value |
| --------------------------------------- | ----- |
| Population size (pop_size)              | 100   |
| Maximum generations (max_gen)           | 300   |
| Crossover probability (p_c)             | 0.9   |
| Mutation probability (p_m)              | 0.1   |
| Distribution indices ((\eta_c, \eta_m)) | 20    |

**Evolutionary process**:

1. Fast non-dominated sorting + constraint handling
2. Tournament selection → Crossover → Mutation → Evaluation
3. Merge parents and offspring → Environmental selection, maintaining constant population size

### 2.1.3 Experimental Results

Number of feasible solutions: approximately 100% of the final population.

Objective values range:

f1: [0.000, 1.150]

f2: [0.000, 1.150]

### 2.1.4 Discussion

In the experiment, some segments of the Pareto front may not be fully covered. The possible reasons include:

1. **Complex and narrow feasible region**: MW7's feasible area forms a thin ring, making crossover and mutation prone to generating infeasible solutions.
2. **Loss of population diversity**: Premature convergence may cause some regions of the front to remain unexplored.
3. **Boundary individual disruption**: Individuals on the constraint edges are likely to produce infeasible offspring during crossover.
4. **Random factors**: The initial population or random seeds may influence the coverage of the front.

Possible improvements include:

1. Introducing a **Repair Operator** to project infeasible solutions back into the feasible region.
2. Using **adaptive mutation rates** to enhance search near constraint boundaries.
3. Applying **Deb's Constraint-Domination Principle** in selection.
4. Increasing the population size or number of generations to enhance diversity.


