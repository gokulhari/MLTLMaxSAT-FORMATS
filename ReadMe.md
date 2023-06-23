# Abstract

Mission-time Linear Temporal Logic (MLTL) is a popular language for specifications in safety-critical cyber-physical systems. MLTL is a variant of Linear Temporal Logic (LTL) with finite interval bounds on temporal operators. It is often hard to satisfy all requirements, and prioritization and trade-offs are integral to designing complex systems. Given a set of specifications, the maximum satisfiability problem (MaxSAT) asks to find the maximum number of simultaneously satisfiable specifications. Considering the significant advances in MaxSAT for boolean logic, we develop translations of MLTL to boolean logic. Given an MLTL formula $\phi$ of length $|\phi|$ with maximum interval length $m$, our first translator runs in $\mathcal O(m^{|\phi|})$ time. Performance tests of satisfiability checks on MaxSAT instances show that this exponential translation performs significantly better than the best satisfiability checking approaches reported recently in the literature on random instances. Furthermore, we report an improved translation algorithm that runs in $\mathcal O(|\phi|^2m^2)$ time that outperforms the best approaches reported thus far on both real and synthetic (random) specifications. The new approach is embarrassingly parallelizable to a factor of $|\phi|m$. We contribute to (1) an easy-to-implement translation from MLTL to boolean logic that runs in $\mathcal O(m^{|\phi|})$ time, and (2) an efficient translation that runs in $\mathcal O(|\phi|^2m^2)$ time, and we prove their correctness and runtime. Lastly, (3) we consider example cases of using existing boolean MaxSAT solvers to solve the MLTL MaxSAT problem.

## What this artifact includes
This artifact shares everything needed to reproduce Figures 1a-1d, 2a-2d and Table 2, which covers all results in this paper. To get started, download the Docker image, and start an interactive terminal using it. The Docker image has Ubuntu 22.04 installed in it, with all necessary packages installed. We also provide the Dockerfile used to produce this image. In the interactive terminal, navigate to the home folder:
```bash
cd /home/gokul/
ls
```
You should see two folders listed: `artifact` and `MLTLMaxSAT-FORMATS`. The first one is the [artifact](https://temporallogic.org/research/CAV19/) of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1). The second one is our implementation of the two Boolean translations. As we note in the paper, we were able to port the SMT translation of [Li et al.](https://link.springer.com/chapter/10.1007/978-3-030-25543-5_1) into our code 

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

As the full reproduction of results takes a long time,





## To fully reproduce (takes about 34 hours)
To make a full run and regenerate all plots, do the following. Assuming strictly that you have navigated in the terminal to this current directory where this ReadMe.md is present:
```bash
./runAll
cd plots
python3 plotter.py
```
The plots must have been generated in the `plots/` folder.

## A shorter run with main results (takes about 2 hours)
```bash
./runShort
cd plots
python3 plotter.py
``` 
This one only compares the fastest 3 approaches, i.e., the SMT, bool-slow and bool-fast approaches.

# Full installation

In the current folder, do the following:
```bash
mkdir build
cd build
cmake ..
make -j8
```
Open-source packages are automatically downloaded and installed by cmake.

We also make comparisons with Li et al.'s approaches, hence use these steps in order: 
1. Download their [artifact](https://temporallogic.org/research/CAV19/artifact.tar.xz). 
2. Go to artifact/translators/src/ and type `make` in a terminal. This will generate an executable, `MLTLConvertor`
3. Make a note of the path to the executable in Step 2. Say the path is `path/to/MLTLConvertor`.
4. Download the appropriate NuXmv file compatible with your OS from [here](https://nuxmv.fbk.eu/download.html). Note the path to NuXmv as `path/to/NuXmv`
4. Go to the file `Translators/src/runner.cpp`, and update strings mltlConverterExe to `path/to/MLTLConvertor` and nuXmvExe to `path/to/NuXmv` in Lines 14 - 20.
5. Run 
```bash
./runAll
cd plots
python3 plotter.py
```
