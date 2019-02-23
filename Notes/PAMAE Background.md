# PAMAE: Background

### K Medoids

* High computational complexity
* More robust to noise or outliers than K-Means
* Most common realization - **Partitioning Around Medoids** - quadratic time complexity
* K-Means - Linear time complexity with respect to the number of objects



---



### Efficiency problem of K-Medoids

* **Global search vs local search**

  local search improves efficiency but suffers from local optima problem

* **Entire data vs sampled data**

  * use of sampled data significantly improves efficiency
  * if the set of true medoids is far from a sampled cluster -> results deviate from optima
  * CLARA and CLARANS - variations of PAM based on sampling

* **Non-parallel processing vs parallel processing**

  * PAM-MR, FAMES-MR, CLARA-MR
    * FAMES-MR - local search
    * CLARA-MR - sampling
    * PAM-MR - local search to parallelize medoid update process 
  * Higher efficiency than base algorithms - do not guarantee high accuracy
  * **Recent algorithms - GREEDI, MR-KMEDIAN** 
    * not free from efficiency problems
    * GREEDI - global search over each partition ~ sample
      * accuracy relatively high compared to earlier algorithms in a small dataset
      * efficiency issues
    * K-MEDIAN 
      * constructs single best sample by parallel iterative sampling
      * accuracy - acceptable if n(clusters) high

---



* ***Global search* and *entire dataset*** 
  * main ingredients for high accuracy
  * simultaneous use - harms accuracy
  * **Apply them individually through two phases - Parallel Seeding and Parallel Refinement**
* **Phase 1** 
  * Generate random samples
  * Run k-medoids (PAM) - parallel global search on samples
  * select best among set of seeds (**CLARIFY**)
* **Phase 2**
  * aim - reducing possible errors induced by sampling
  * partitions entire data based on seeds - constructs initial clusters
  * updates medoids in parallel locally in each cluster

### Main contributions:

* K-Medoids clustering in 2 phases - achieves high accuracy and efficiency - provides formal justifications for the use of sampling + refinement
* Theoretical proof of soundness of PAMAE wrt accuracy
* evaluated PAMAE on 4 real-world datasets - lowest error among *parallel k-medoids algorithms*

---



## Related Work

### Non-Parallel K-Medoids Algorithms

* **PAM** 
  * finds most centrally located objects - medoids
  * start - arbitrary selection of initial medoids 
  * iteratively updates medoids - random swapping of medoid objects with non-medoid objects
  * if swapping reduces clustering error - current medoid object replaced with new
  * until no change in set of medoids
  * Time complexity - ***O(k(N - k)<sup>2</sup>)*** where **N - number of objects**
* **CLARA** 
  * applies PAM on 5 random samples of 40+2k objects
  * procedure repeated multiple times as results affected heavily by sampling
  * selects best set as final result
* **CLARANS**
  * considers a graph - every node = potential solution
  * dynamically draws on a sample of neighbors to confine search space
* **MEM**
  * distances for all possible pairs of data objects stored in advance
  * local search to update medoids
* **CVI**
  * density based selection of initial medoids - reduces #iterations
* **IFART**
  * uses Fuzzy Art - unsupervised learning technique - select initial medoids
* **FAMES**
  * find medoids using geometric computation



### Parallel k-Medoids Algorithms

* **PAM-MR** 
  * **MAP** : assigns each object to closest medoid
  * **REDUCE**: updates current medoid to most central object using pair-wise computation
* **FAMES-MR**
  * **MAP**: same as PAM-MR
  * **REDUCE**: geometric technique for each cluster (??)
* **CLARA-MR**
  * 2 mapreduce jobs - run PAM on samples - select best set in parallel
* **GREEDI**
  * greedy algorithm 
  * candidate set of k medoids from partitions of entire data
  * runs greedy algorithm over set of medoids to find final set of k medoids
  * #sample, size(sample) - dependent on size(entire data) => does not completely resolve efficiency issue
* **MR-KMEDIAN** 
  * constructs single best sample - parallel iterative sampling
  * run weighted k-medoids over sample
  * iterative sampling expensive - repeated accesses to entire data to calculate distances between sampled and unsampled objs
  * efficiency - inversely related to k

