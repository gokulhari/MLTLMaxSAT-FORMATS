# Abstract

Mission-time Linear Temporal Logic (MLTL) is a popular language for specifications in safety-critical cyber-physical systems. MLTL is a variant of Linear Temporal Logic (LTL) with finite interval bounds on temporal operators. It is often hard to satisfy all requirements, and prioritization and trade-offs are integral to designing complex systems. Given a set of specifications, the maximum satisfiability problem (MaxSAT) asks to find the maximum number of simultaneously satisfiable specifications. Considering the significant advances in MaxSAT for boolean logic, we develop translations of MLTL to boolean logic. Given an MLTL formula $\phi$ of length $|\phi|$ with maximum interval length $m$, our first translator runs in $\mathcal O(m^{|\phi|})$ time. Performance tests of satisfiability checks on MaxSAT instances show that this exponential translation performs significantly better than the best satisfiability checking approaches reported recently in the literature on random instances. Furthermore, we report an improved translation algorithm that runs in $\mathcal O(|\phi|^2m^2)$ time that outperforms the best approaches reported thus far on both real and synthetic (random) specifications. The new approach is embarrassingly parallelizable to a factor of $|\phi|m$. We contribute to (1) an easy-to-implement translation from MLTL to boolean logic that runs in $\mathcal O(m^{|\phi|})$ time, and (2) an efficient translation that runs in $\mathcal O(|\phi|^2m^2)$ time, and we prove their correctness and runtime. Lastly, (3) we consider example cases of using existing boolean MaxSAT solvers to solve the MLTL MaxSAT problem.

## To get started
- In Linux or Mac, make sure, you have CMake 3.22.1, and Java Runtime environment. Then just run `./installer.sh` in the terminal, it will take about 15 minutes to install.
- I have not tested on Windows, but you can use Docker. The dockerfile is in `environment/Dockerfile`. This will do everything needed to get you started.
- After installation is over, go to `build/` director and type './main' to get started.

## This artifact
This artifact shares everything needed to reproduce Figures 1a-1d, 2a-2d and Table 2, which covers all results of our paper. To get started, download the Docker image, and start an interactive terminal using it. The Docker image has Ubuntu 22.04, with everything installed. In the interactive terminal, navigate to the home folder:
```bash
cd /home/gokul/
ls
```
You should see two folders listed amongst others: `artifact` and `MLTLMaxSAT-FORMATS`. The first one is the [artifact](https://temporallogic.org/research/CAV19/) of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1). The second one is our implementation of the two Boolean translations. As we note in the paper, we ported the SMT translation of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1), whereas we directly use the artifact of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1) for the SMV translation. The fast and slow algorithms are implemented in `MLTLMaxSAT-FORMATS/src/Translators.cpp` and `MLTLMaxSAT-FORMATS/include/Translators.h` files.  


## Using our data to plot:
Next navigate to the folder `MLTLMaxSAT-FORMATS` folder:
```bash
cd MLTLMaxSAT-FORMATS/
```
The full reproduction of results takes about 34 hours. We included the data, so that you can simply plot, without regenerating the data. The data is stored at `MLTLMaxSAT-FORMATS/BenchmarkResults/`. Navigate to the plots folder and run the `plotter.py` script:
```bash
cd plots/
python3 plotter.py
```   
This will regenerate the plots 1a-1d, and 2a-2d by using the existing data in `MLTLMaxSAT-FORMATS/BenchmarkResults/`. 

## Regenerating data by yourself:

It took us 34 hours in total to produce the data. As this might be too long, we arranged a shorter version that you can generate in about 2 hours. 

In summary, there are five appraoches that we consider: The SMT (of Li et al.), the slow and fast boolean transaltions, and the IC3 and BMC appraoches (of Li et al., both of which use the SMV translation). These five approaches are used on two sets of data: real NASA-ATF and random instances. However, the real instances have ternary operators that aren't accomodated by the Li et al.'s parsers, and hence, the we do not have results with the SMV translation for real instances.   

The short version reproduces 3 out of 5 curves in figures 2a-2d, i.e., the ones marked as SMT, Bool-slow and Bool-fast (these are the fastest approaches, consequently, the data is generated faster as well).  

### The short version for quick reproduction
Go inside the directory `MLTLMaxSAT-FORMATS`, and run:
```bash
bash runShort.sh
```

### The full version
Go inside the directory `MLTLMaxSAT-FORMATS`, and run:
```bash
bash runAll.sh
```

### To reproduce Table 2
Explore that the test cases of Table 2 are available in `MLTLMaxSAT-FORMATS/Benchmarks/MaxSATCases/`, from inside `MLTLMaxSAT-FORMATS/`:
```bash
ls Benchmarks/MaxSATCases/
``` 
and see the test cases listed. Each line in an `.mltl` file is an MLTL MaxSAT clause. To run any desired case, navigate to the `MLTLMaxSAT-FORMATS/build/` directory in the terminal and type:
```bash
./MaxSAT ../Benchmarks/MaxSATCases/testP4L10M100.mltl
``` 
likewise for other test cases in that folder. The MaxSAT executable needs the location of the file in other words.

## Detailed description of files

There are 6 executables in the build folder:
1. `main`; executes the fast translation to Boolean logic
2. `main2`; executes Li et al.'s SMT translation 
3. `main3`; executes the slow translaltion to Boolean logic.
4. `genMLTLBenchmarks`; takes LTL formula files and converts them to MLTL formulas by assigning random intervals. 
5. `benchmarker`; takes as input an file with a list of MLTL formulas (only separated by the newline character), and runs `main`, `main2` and `main3` on each of those formulas.  It also runs a SAT check using the Z3 solver, measures the time, and stores the results in the BenchmarkResults directory. The outputs are three files with the same name as the input file, but with extensions `.prop`, `.propS`, and `.smt2`, for the results with the fast Boolean translator, slow Boolean translator, and Li et al.'s SMT translation respectively.
6. `MaxSAT`; solves for the MLTL-MaxSAT problem. It takes as input a file with a list of MLTL formulas. Each clause must be in one line. The output is from z3, saying SAT and number of clauses that are satisfiable. Suppose there are 16 clauses, and MaxSAT returns "objectives (2)", that means that 16 - 2 = 14 clauses are simultaneously satisfiable. 

## LTL Syntax preferences
We use Antlr for parsing. Users can modify [LTL.g4](./LTL.g4) to suit their preferred syntax. In specific, the operator precedence is such that operators on the top of the defition of an LTL expression (`expr` in [LTL.g4](./LTL.g4)) have higher precedence over ones at the bottom.    

# Misc.

We use the following open-source works, and the copyright belongs to the respective owners:
1. Z3 theorem prover
2. Antlr for parsing
4. [NuXmv](https://nuxmv.fbk.eu)
3. The [artifact](https://temporallogic.org/research/CAV19/) of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1)
4. NASA-ATF specifications, taken from the [artifact](https://temporallogic.org/research/TACAS18/) of [Dureja and Rozier](https://link.springer.com/chapter/10.1007/978-3-319-89960-2_17).