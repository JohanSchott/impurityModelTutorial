# Spectroscopy tutorial
This tutorial provides information of how to install needed libraries for the package in [https://github.com/JohanSchott/impurityModel](https://github.com/JohanSchott/impurityModel) and how to use the package on a laptop as well as on a computer cluster.

## Install spectroscopy program
Choose machine: laptop or login to a computer cluster, e.g. Beskow, Tetralith, …

Download the spectroscopy program by typing:
```bash
git clone https://github.com/JohanSchott/impurityModel.git
```
For instructions of how to get started with the spectroscopy program, read the README file on the webpage: 
[https://github.com/JohanSchott/impurityModel](https://github.com/JohanSchott/impurityModel)

## Run simulations of NiO
Follow the README instructions in the link above.

## Plot the generated spectra and the generated text output of NiO
Follow the README instructions in the link above.

## Installation help

### Practical help for usage on local computer
#### Installing missing Python libraries
If not all needed Python libraries are installed the program will stop and complain.
We can try to install the missing libraries.
For example, if the library `numpy` is missing, type:
```bash
pip install numpy
```
If Python is installed with Anaconda we can alternatively install the library with:
```bash
conda install numpy
```
Note, mpi4py might be a bit tricky to install, as it requires MPI compilers. 
Below follows instructions for how to install gfortran and open MPI compilers.

#### How to install gfortran compiler on mac
First check if gfortran already exists:
```bash
which gfortran
```
If not installed, we can install it using Homebrew.
- Install Homebrew from the terminal (if not already installed):
```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- Update Homebrew and make sure everything is ok.
```bash
brew update
brew doctor
```
- Install gfortran from gcc, and swig:
```bash
brew install gcc
brew install swig
```
Alternative gfortran installation: Download .dmg file from:
[https://github.com/fxcoudert/gfortran-for-macOS/releases](https://github.com/fxcoudert/gfortran-for-macOS/releases)
  

#### How to install Open-MPI compiler
Download .tar.gz file from:
[https://www.open-mpi.org/software/ompi/v3.1/](https://www.open-mpi.org/software/ompi/v3.1/)
Follow instructions in the `INSTALL` file:
```bash
cd openmpi-3.1.3/
mkdir /usr/local/openmpi
./configure --prefix=/usr/local/openmpi
make all
make install
```
If this is successful, try to install mpi4py:
```bash
pip install mpi4py
```





### Practical help for usage on computer clusters
#### On Beskow:
Load a module with all needed Python libraries installed, e.g.:
```bash
module load anaconda/py36/4.3
```
Start an interactive node, or create a job script.
Then, setup Python by typing:
```bash
source activate custom
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib
```
Run the program:
```bash
aprun -n 2  NiO.py 
```

For plotting, there might be some issues on Beskow. 
Therefor, it might be better to do plotting from Tegner instead. 
Login at Tegner and load the Python module:
```bash
module load anaconda/py36/5.0.1 
```
Move to folder where the simulations are done and type:
```
plotSpectra.py
```

#### On Tetralith:
Load a module with all needed Python libraries installed, e.g.:
```bash
module load Python/3.6.7-env-nsc1-gcc-2018a-eb
```
Some libraries are missing here so we have to create a virtual environment:
```bash
virtualenv --system-site-packages test
```
Activate the virtual environment (that we just created):
```bash
. test/bin/activate
```
Install the following libraries (this is done only once):
```bash
pip install sympy
pip install PyQt5
```
Deactivate the virtual environment (that we created):
```bash
deactivate
```
Start an interactive node, or create a job script.
Then, setup Python by typing:
```bash
. test/bin/activate
```
Run the program:
```bash
mpirun -n 2  NiO.py 
deactivate 
```


  












