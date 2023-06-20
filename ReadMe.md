# How to run from this docker image

In this docker image, all necessary packages are already installed. Skip to the next section to install the scratch.

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
