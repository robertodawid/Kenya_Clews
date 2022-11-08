## Running OSeMOSYS

If you've followed the installation instructions below using conda, before running OSeMOSYS you'll
need to activate your conda environment:

    conda activate osemosys

To run OSeMOSYS, first navigate to the folder containing your OSeMOSYS files.
By default, OSeMOSYS results are written into the `res/csv` directory. If this doesn't already exist,
you'll want to create it:

    mkdir -p res/csv

Now enter the following line into your command prompt:

    glpsol -m model.txt -d  data.txt

The results from the model run should be available in the `res/csv` folder.

### Running using other solvers

Running using CBC or CLP is a little more involved, as it requires two separate steps.
First, an LP file is generated using glpsol:

    glpsol -m model.txt -d data.txt --wlp Kenya_clews.lp --check

Then the LP file is passed to the CBC (or CLP) solver to produce a solution file:

    cbc Kenya_clews.lp solve -solu Kenya_clews.sol

The solution file is written out in a format that requires post-processing. We recommend using
the open-source Python package **otoole** to do this for you:

    otoole results cbc csv Kenya_clews.sol results --input_datafile data.txt

## Installation

To use OSeMOSYS, you'll need to install the 
[GNU Linear Programming Kit](https://www.gnu.org/software/glpk/).

The easiest way to do this is to use Anaconda or Miniconda.

### 1. Install conda package manager

First, install [miniconda](https://docs.conda.io/en/latest/miniconda.html#) by following
the instructions [here](https://conda.io/projects/conda/en/latest/user-guide/install/#).

### 2. Create a new environment

Run the following commands to create and activate a new environment containing the `glpk` library:

    conda create -n osemosys python=3.8 glpk
    conda activate osemosys

### 3. Optional - install **otoole** for pre- and post- processing

First, activate the conda environment and then install otoole::

    conda activate osemosys
    pip install otoole

### 4. Optional - install CBC or CLP open-source solvers

On Linux or OSX you can also install the more powerful CBC or CLP
open-source solvers using conda:

    conda install -c conda-forge coincbc coin-or-clp
