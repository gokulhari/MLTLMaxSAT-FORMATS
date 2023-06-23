# Abstract

Mission-time Linear Temporal Logic (MLTL) is a popular language for specifications in safety-critical cyber-physical systems. MLTL is a variant of Linear Temporal Logic (LTL) with finite interval bounds on temporal operators. It is often hard to satisfy all requirements, and prioritization and trade-offs are integral to designing complex systems. Given a set of specifications, the maximum satisfiability problem (MaxSAT) asks to find the maximum number of simultaneously satisfiable specifications. Considering the significant advances in MaxSAT for boolean logic, we develop translations of MLTL to boolean logic. Given an MLTL formula $\phi$ of length $|\phi|$ with maximum interval length $m$, our first translator runs in $\mathcal O(m^{|\phi|})$ time. Performance tests of satisfiability checks on MaxSAT instances show that this exponential translation performs significantly better than the best satisfiability checking approaches reported recently in the literature on random instances. Furthermore, we report an improved translation algorithm that runs in $\mathcal O(|\phi|^2m^2)$ time that outperforms the best approaches reported thus far on both real and synthetic (random) specifications. The new approach is embarrassingly parallelizable to a factor of $|\phi|m$. We contribute to (1) an easy-to-implement translation from MLTL to boolean logic that runs in $\mathcal O(m^{|\phi|})$ time, and (2) an efficient translation that runs in $\mathcal O(|\phi|^2m^2)$ time, and we prove their correctness and runtime. Lastly, (3) we consider example cases of using existing boolean MaxSAT solvers to solve the MLTL MaxSAT problem.

## What this artifact includes
This artifact shares everything needed to reproduce Figures 1a-1d, 2a-2d and Table 2, which covers all results in this paper. To get started, download the Docker image, and start an interactive terminal using it. The Docker image has Ubuntu 22.04 installed in it, with everything installed. We also provide the Dockerfile used to produce this image. In the interactive terminal, navigate to the home folder:
```bash
cd /home/gokul/
ls
```
You should see two folders listed: `artifact` and `MLTLMaxSAT-FORMATS`. The first one is the [artifact](https://temporallogic.org/research/CAV19/) of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1). The second one is our implementation of the two Boolean translations. As we note in the paper, we were able to port the SMT translation of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1), for the SMV translation, we directly use the artifact of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1). You may verify that the fast and slow algorithms are implemented in the `MLTLMaxSAT-FORMATS/src/Translators.cpp` and `MLTLMaxSAT-FORMATS/include/Translators.h` files.  


## Using our data to plot:
Next navigate to the folder `MLTLMaxSAT-FORMATS` folder:
```bash
cd MLTLMaxSAT-FORMATS/
```
The full reproduction of results takes about 34 hours. We included the data, so that you can simply plot, without regenerating the data. The data is stored at `MLTLMaxSAT-FORMATS/BenchmarkResults/`. To do simply plot, navigate to the plots folder and run the `plotter.py` Python script.
```bash
cd plots/
python3 plotter.py
```   
This will regenerate the plots 1a-1d, and 2a-2d by using the existing data in `MLTLMaxSAT-FORMATS/BenchmarkResults/`. 

## Regenerating data by yourself:

It took us 34 hours in total to produce the data. As this might be too long, we have arranged a shorter version that you can plot in about 2 hours. The short version reproduces 3 out of 5 curves in figures 2a-2d, i.e., the ones marked as SMT, Bool-slow and Bool-fast (as these are the fastest approaches, the plots are faster as well).  

### The short version for quick reproduction
Go inside the directory `MLTLMaxSAT-FORMATS`. And run:
```bash
bash runShort.sh
```

### The full version
Go inside the directory `MLTLMaxSAT-FORMATS`. And run:
```bash
bash runAll.sh
```

### To reproduce Table 2
The tough MaxSAT instances in Table 2 are included 


We use the following open-source works, and the copyright belongs to the respective owners:
1. Z3 theorem prover
2. Antlr for parsing
3. The [artifact](https://temporallogic.org/research/CAV19/) of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1)
4. NASA-ATF specifications, taken from the [artifact](https://temporallogic.org/research/TACAS18/) of [Dureja and Rozier](https://link.springer.com/chapter/10.1007/978-3-319-89960-2_17).