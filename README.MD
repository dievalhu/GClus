# GClus: Enforcing Explicit Size Constraints in Graph Clustering through Structural Refinement

## Overview

This repository contains the experimental materials and supplementary documentation for the paper:

**GClus: Enforcing Explicit Size Constraints in Graph Clustering through Structural Refinement**

GClus is a graph clustering algorithm that integrates strict, user-defined size limits into the community detection process. It combines structural metrics (k-truss, clique density, and conductance) with iterative split-and-merge refinements to ensure that communities remain both structurally cohesive and balanced in size.

---

## A) Datasets

To evaluate GClus, we employ a diverse collection of graph benchmarks. For real-world data, we consider well-known networks such as Dolphins<sup>1</sup>, Karate<sup>2</sup>, Polbooks<sup>3</sup>, and Les Misérables<sup>4</sup>. These networks provide established ground truths or meaningful community divisions that are commonly used for clustering evaluation:

### Real-World Networks

- **Dolphins**  
  Originally introduced by Lusseau et al. <sup>1</sup>, the Dolphins network captures the association patterns among 62 bottlenose dolphins. Comprising 159 undirected edges, this network exemplifies a social system with moderate sparsity and clearly defined subgroups. The ground truth for this dataset (typically representing two communities) provides an established benchmark for evaluating clustering performance in terms of both adherence to size constraints and community cohesiveness.

- **Karate Club**  
  The Karate club network, first documented by Zachary<sup>2</sup>, models the social interactions among 34 members of a karate club. With 78 edges connecting the nodes, this dataset is renowned for its distinct bifurcation into two communities. Despite its relatively small size, the Karate network poses a significant challenge due to the presence of overlapping attributes and varied interaction strengths, making it an ideal candidate for testing the precision and stability of size-constrained clustering approaches.

- **Polbooks**  
  Based on co-purchasing patterns of books on politics as described in <sup>3</sup>, the Polbooks network contains 105 nodes and 441 edges. The denser structure and the presence of three naturally occurring communities test the ability of clustering algorithms to balance between high internal connectivity and strict size restrictions. This dataset is particularly useful for understanding how the algorithm scales with increasing edge density and more complex community arrangements.

- **Les Misérables**  
  This dataset is derived from the character co-occurrence network in Victor Hugo’s *Les Misérables*<sup>4</sup>. With 77 nodes and 254 edges, the graph offers a unique environment due to its literary origin. Unlike the previous datasets, *Les Misérables* is sourced through the Python NetworkX package, and it does not come with weights or explicit ground truth community labels. This presents a scenario where clustering must be performed on a dynamically constructed network from textual data, challenging the algorithm’s adaptability and robustness in less structured settings.

### Synthetic Networks

Synthetic benchmarks were generated with explicit size restrictions to test the robustness of GClus under varying densities and geometries.

- **Aggregation** – A benchmark network modeling a clear aggregation phenomenon with communities that form distinct clusters, despite some uneven group sizes.  
- **Bridge** – This dataset consists of two densely connected clusters linked by a weak bridge of edges, providing a clear test case for size-constrained merging and splitting.  
- **Compound** – A more challenging synthetic network featuring multiple communities with varying interconnectivity, useful for evaluating the robustness of our refinement steps.  
- **Flame** – Composed of two groups that exhibit a flame-like spatial distribution, this dataset tests the algorithm’s ability to respect somewhat irregular group shapes.  
- **Jain** – A classic benchmark consisting of two well-separated communities in the form of moons.  
- **Spiral** – A synthetic instance with three clusters arranged in a spiral pattern, challenging the algorithm to partition intertwined groups while enforcing size constraints.  
- **Two Diamonds** – A network with two “diamond”-shaped communities, exemplifying clear intra-cluster concentrations with a hard cardinality requirement.  
- **Sticks** – This instance consists of three communities with stick-like structures, where connectivity is more elongated and enforcing precise group sizes is nontrivial.  
- **Five Cluster** – A more complex synthetic dataset featuring five distinct groups with a wide range of sizes, highlighting the algorithm’s ability to scale to numerous communities.  
- **Noisy Circles** – This dataset is based on circular clusters with added noise, representing a scenario in which the intrinsic community structure is less clear and size constraints must guide the partitioning.

---

## B) Experimental Environment

All experiments were conducted on a dedicated workstation to ensure reproducibility and accurate performance evaluations. The hardware configuration consisted of an Intel Core i7-10510 CPU with 8 physical cores clocked at up to 4.9 GHz, 12 GB of DDR4 RAM, and a 1 TB NVMe solid-state drive.  For software, we used Python 3.12 as the primary programming language, taking advantage of its extensive ecosystem for numerical computing and network analysis. The experimental framework was set up within a Conda environment (version 24.9.1) to ensure dependency management and reproducibility. Key Python libraries included NetworkX for graph manipulation, NumPy for efficient numerical computations, and SciPy for optimization routines. The algorithmic implementations, such as the community extraction, refinement, and balancing modules of GClus, were thoroughly tested using this environment, ensuring consistent performance across multiple runs.

To monitor execution times and memory usage, we incorporated profiling tools (e.g., Python’s built-in `time` module and `memory_profiler`) into our evaluation pipeline. Execution times were measured in milliseconds, and memory consumption was tracked in kilobytes throughout the application’s runtime.  This enabled us to capture detailed performance metrics under various experimental configurations.

In order to replicate the experimental environment and reproduce the results, the following setup procedure is provided. It details the required dependencies, installation steps, and testing commands used during the evaluation process.

## _**Dependencies**_  
Libraries: networkx, matplotlib, scikit-learn, pymetis, pyvis.  

## _**Installation**_  
1. Install [Miniconda3](https://docs.anaconda.com/miniconda/). During installation, check the option to add conda to the PATH; installation for Windows systems.  
2. Install Miniconda3 for Linux systems using the following commands:  
   ```sh
   apt update -y
   apt upgrade -y
   wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
   bash Miniconda3-latest-Linux-x86_64.sh
   source ~/miniconda3/bin/activate
   conda init
   ```
3. Verify the correct installation of Miniconda3.  
   ```sh
   conda --version
   ```
4. Open the terminal in the project's root folder and run the following commands:  
   ```sh
   pip install networkx
   pip install matplotlib
   pip install scikit-learn
   pip install pyvis
   conda update conda
   conda install conda-forge::pymetis
   ```
## _**Algorithm Testing**_  
1. From the root folder, the clustering algorithm can be tested on both Windows and Linux systems using:  
   ```sh
   python test.py
   ```
---

## C) Cluster Structures

In Figure 2 we show the cluster structures obtained by the two algorithms (GClus and SMKNN) on the real-world networks (Table 1).

<div align="center">
  <img src="images/structures.png" 
       alt="Fig. 2: Comparison of Nodes Grouped by the Clustering Algorithms GClus (first row) and SMKNN (second row) for Karate, Dolphins, and Polbooks graphs." 
       width="95%">
  <br>
  <sub><b>Fig. 2.</b> Comparison of nodes grouped by the clustering algorithms GClus (first row) and SMKNN (second row) for Karate, Dolphins, and Polbooks graphs.</sub>
</div>

---

## D) Execution Time and Memory Usage

For the real-world datasets (see Table 2 and Figure 3), our experiments reveal that GClus’s additional operations—such as iterative split-and-merge refinement and node adjustment—introduce a modest overhead in both execution time and memory usage compared to SMKNN.  For example, on the Karate and Dolphins networks, while the overall runtime difference is within a few milliseconds (e.g., GClus takes roughly 24–27 ms as opposed to SMKNN’s 16–25 ms), GClus tends to use slightly less or comparable memory due to its targeted local adjustments that prevent unnecessary global computations. In larger and more complex instances such as Polbooks and *Les Misérables*, even though GClus’s iterative refinement phase increases its overall runtime and memory consumption (with execution times differing by roughly 10–30 ms and memory usage showing similar moderate increases), the cost remains in a competitive range with SMKNN.  

For synthetic datasets, where graphs are typically larger or exhibit more challenging community structures, the computational impact of enforcing size constraints becomes more pronounced.  In datasets like Compound, Five Cluster, or Spiral, GClus’s iterative balancing requires additional passes that can roughly double the execution time compared to SMKNN.  The memory overhead in these cases also increases, as GClus must maintain extra intermediate structures during the split-and-merge process; however, this increase is generally linear with the graph size and acceptable given the added benefit of strict size conformity.  

Overall, while GClus demonstrates slightly higher computational demands on complex synthetic instances, its runtime and memory footprint remain competitive—especially when the application’s priority is on achieving balanced, size-constrained clusters.

<table>
  <tr>
    <td align="center" width="50%">
      <img src="images/Time-1.png" alt="Execution time (ms) comparison between GClus and SMKNN" width="95%"><br>
      <sub><b>Fig. 3a.</b> Execution time (ms).</sub>
    </td>
    <td align="center" width="50%">
      <img src="images/Memory-1.png" alt="Memory (KB) comparison between GClus and SMKNN" width="95%"><br>
      <sub><b>Fig. 3b.</b> Memory (KB).</sub>
    </td>
  </tr>
</table>

---

## D) Computational Complexity and Convergence Analysis

This appendix provides an analysis of the computational complexity and convergence behavior of \gclus. The analysis is intended to clarify the cost of each stage of the algorithm and the conditions under which the iterative refinement process terminates.

\subsection{Notation}

Let $G=(V,E)$ be an undirected and unweighted graph, where $n=|V|$ is the number of nodes and $m=|E|$ is the number of edges. Let $k$ be the number of communities to be obtained, $h$ the maximum allowed community size, $l$ the minimum allowed community size, and $i_{\max}$ the maximum number of refinement iterations. We denote by $d(v)$ the degree of node $v$, and by $\Delta$ the maximum degree in the graph.

\subsection{Computational Complexity}

The \gclus pipeline consists of two main stages: initial community extraction and iterative refinement.

\paragraph{Pivot selection.}
The pivot selection step computes the degree centrality of each node and sorts the nodes according to this score. Computing all degrees requires $O(n+m)$ time. Sorting the nodes requires $O(n\log n)$ time. Therefore, the total cost of pivot selection is:

\[
O(n+m+n\log n).
\]

\paragraph{Initial community extraction.}
For each pivot node, \gclus builds an initial community using the \texttt{GC-Base} and \texttt{GC-BranchBoundP} procedures. In \texttt{GC-Base}, candidate nodes are selected from the neighborhood of the current community and ranked according to the expansion score:

\[
S(v,C)=\alpha\tau(v)+\beta\Delta(v,C).
\]

If trussness values are precomputed, evaluating this score for a candidate node can be done in time proportional to the number of candidate-community connections. In the worst case, a community may expand until it reaches the upper bound $h$, and each expansion step may inspect the neighborhood of the current community. Thus, for one community, the cost can be bounded by:

\[
O(h\Delta).
\]

For $k$ communities, the initial extraction phase is bounded by:

\[
O(kh\Delta),
\]

assuming trussness scores are already available. If trussness values are computed from scratch, an additional preprocessing cost is required. This cost depends on the specific truss decomposition implementation. In general, truss decomposition is more expensive than degree computation because it requires triangle-based support calculations.

\paragraph{Branch-and-bound refinement.}
The \texttt{GC-BranchBoundP} procedure evaluates candidate nodes from the boundary set $R$. In the worst case, $|R|$ can be bounded by $O(h\Delta)$ for a community of size at most $h$. If the cohesion score is updated incrementally, each candidate can be evaluated efficiently. Thus, the cost per community is bounded by:

\[
O(h\Delta),
\]

and across $k$ communities by:

\[
O(kh\Delta).
\]

\paragraph{Non-clustered node assignment.}
Let $n_u$ be the number of unassigned nodes after initial extraction. For each unassigned node, \gclus evaluates its possible assignment to communities with available capacity. In the worst case, each unassigned node is compared with all $k$ communities. Therefore, the assignment step has complexity:

\[
O(n_u k).
\]

If conductance is recomputed from scratch for each candidate insertion, the cost may increase depending on the number of incident edges. With incremental updates, the practical cost is closer to the number of inspected node-community connections.

\paragraph{Merge small communities.}
The merge step considers communities whose size is below the lower bound $l$ and searches for a compatible neighboring community. In the worst case, each community may be compared with the remaining $k-1$ communities. Therefore, the merge phase is bounded by:

\[
O(k^2),
\]

plus the cost of evaluating inter-community conductance. If conductance is computed using stored boundary information, this cost is reduced; otherwise, it depends on the number of crossing edges inspected.

\paragraph{Split large communities.}
The split phase is applied to communities whose size exceeds the upper bound $h$. Let $s_i=|C_i|$ be the size of a community selected for splitting, and let $m_i$ be the number of edges in the induced subgraph $G[C_i]$. Constructing the induced subgraph requires:

\[
O(s_i + m_i).
\]

The cost of the actual partitioning step depends on the graph partitioning strategy used. If a simple recursive bisection or local partitioning heuristic is used, the cost is typically near-linear in the size of the induced subgraph. Thus, for all communities selected for splitting, the total cost can be approximated by:

\[
O\left(\sum_i (s_i + m_i)\right).
\]

In the worst case, this can approach $O(n+m)$ per refinement iteration.

\paragraph{Node adjustment.}
The adjustment step moves weakly integrated nodes from oversized communities to communities with available capacity. In the worst case, each node may be considered for reassignment and compared with up to $k-1$ alternative communities. This gives a bound of:

\[
O(nk)
\]

per refinement iteration. If shortest-path distances to pivots are recomputed during this phase, the cost increases. Using breadth-first search from each pivot in an unweighted graph costs:

\[
O(k(n+m)).
\]

However, if pivot distances are precomputed once per iteration and reused, this cost is paid once per iteration rather than once per reassignment.

\paragraph{Overall complexity.}
Combining the previous components, the initial construction stage has approximate complexity:

\[
O(n+m+n\log n+kh\Delta),
\]

assuming trussness values are available or precomputed separately. The iterative refinement stage has an approximate per-iteration cost of:

\[
O(n_u k + k^2 + n + m + nk),
\]

where the $O(n+m)$ term accounts for possible induced-subgraph construction and local splitting operations. Therefore, for at most $i_{\max}$ refinement iterations, the total complexity can be summarized as:

\[
O(n+m+n\log n+kh\Delta+i_{\max}(n_u k+k^2+n+m+nk)).
\]

Since $n_u \leq n$, this can be simplified as:

\[
O(n+m+n\log n+kh\Delta+i_{\max}(nk+k^2+n+m)).
\]

This expression shows that the main computational cost comes from the iterative refinement phase, especially when the number of communities is large or when repeated split, merge, and reassignment operations are required.

\subsection{Memory Complexity}

The algorithm stores the graph structure, the current list of communities, pivot nodes, candidate sets, and auxiliary information such as degrees, trussness scores, and conductance-related values. Storing the graph requires:

\[
O(n+m).
\]

The community assignment structure requires $O(n)$ memory, since each node belongs to one community in the final partition. Candidate sets and temporary induced subgraphs may require additional memory. In the worst case, if large induced subgraphs are constructed during splitting, the temporary memory cost can approach:

\[
O(n+m).
\]

Therefore, the overall memory complexity is:

\[
O(n+m),
\]

with additional temporary overhead during the split and reassignment phases.

\subsection{Convergence and Termination}

\gclus is a heuristic algorithm and does not guarantee convergence to a globally optimal graph partition under size constraints. This is expected because the problem combines structural community detection with cardinality constraints, which may involve competing objectives.

The algorithm terminates under two conditions:

\begin{enumerate}
    \item all communities satisfy the required size bounds and no further reassignment improves the current partition; or
    \item the maximum number of refinement iterations $i_{\max}$ is reached.
\end{enumerate}

Each refinement step modifies the current partition through one of four operations: assigning unclustered nodes, merging small communities, splitting oversized communities, or reassigning weakly integrated nodes. These operations are designed to reduce violations of the size constraints or improve local structural consistency. However, because improving size feasibility may reduce modularity or AMI, and improving structural quality may increase size deviation, the algorithm optimizes a trade-off rather than a single global objective.

Let the size deviation of a partition be defined as:

\[
D(\mathcal{C})=\sum_{i=1}^{k} \left||C_i|-s_i\right|,
\]

where $s_i$ is the target size of community $C_i$. The refinement phase attempts to reduce $D(\mathcal{C})$ by moving nodes from oversized communities to undersized ones, splitting communities larger than $h$, and merging communities smaller than $l$. In practice, this process tends to stabilize after a finite number of iterations because the number of possible node assignments is finite and the algorithm stops after $i_{\max}$ iterations.

Thus, \gclus guarantees finite termination by construction. It also provides a practical convergence mechanism toward a size-feasible or near-feasible partition. Nevertheless, it does not guarantee that the final partition is the global optimum with respect to modularity, conductance, AMI, or any combined constrained objective.

\subsection{Discussion}

The complexity analysis explains the empirical behavior observed in the experiments. On small and medium graphs, the additional cost of enforcing size constraints is limited because the split, merge, and reassignment operations are applied to relatively small induced subgraphs. On larger or structurally irregular datasets, the refinement phase becomes more expensive because more iterations and node movements are required to reduce size deviations.

The convergence behavior also explains the observed trade-off between size feasibility and clustering quality. When the prescribed target sizes are aligned with the natural community structure, \gclus can satisfy the size constraints while preserving strong structural quality. When the prescribed sizes conflict with the intrinsic graph structure, the algorithm may need to move nodes across natural boundaries, which can reduce AMI or modularity. This behavior is consistent with the design goal of \gclus: prioritizing explicit cardinality control while maintaining reasonable structural cohesion.





---

<sup>1.</sup> **Dolphins:** Lusseau, D., Schneider, K., Boisseau, O.J., Haase, P., Slooten, E., Dawson, S.M.: 
The bottlenose dolphin community of doubtful sound features a large proportion of long-lasting associations: can geographic isolation explain this unique trait?
Behavioral Ecology and Sociobiology 54, 396–405 (2003)

<sup>2.</sup> **Karate:** Zachary, W.W.: An information flow model for conflict and fission in small groups.
Journal of anthropological research 33(4), 452–473 (1977)

<sup>3.</sup> **Polbooks:** Newman, M.E.: Modularity and community structure in networks. 
Proceedings ofthe national academy of sciences 103(23), 8577–8582 (2006)

<sup>4.</sup> **Les Misérables:** Cardel´us, J., Serrano, J., Mateo, J.: gsbm: Generalized Stochastic Block Model for Network Data (2023), 
https://cran.r-project.org/web/packages/gsbm/vignettes/les miserables.html, 
r package version 0.1.0











