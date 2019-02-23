# PAMAE: Parallel k-Medoids Clustering with High Accuracy and Efficiency

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

