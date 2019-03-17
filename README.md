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
Read and follow the README instructions in the link above.

## Plot the generated spectra and the generated text output of NiO
Read and follow the README instructions in the link above.

## Installation help of prerequisite libraries and compilers
The information below is probably of interest if you experience problems in executing the program package.

### Practical help for usage on local computer

#### Installing missing Python 3.x 
If Python 3.x is not installed it can be downloaded at e.g.: [https://www.anaconda.com/download](https://www.anaconda.com/download). 
This will also install many useful Python libraries. 
If only some Python libraries are missing, please look in the section below.

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
If this is successful, we might still have to tell where the binaries are.
This is done by typing:
```bash
export PATH=${PATH}:/usr/local/openmpi/bin
```
Finally, try to install mpi4py:
```bash
pip install mpi4py
```





### Practical help for usage on computer clusters
#### On Beskow:
Load a module with all needed Python libraries installed, e.g.:
```bash
module load anaconda/py36/4.3
module load git/2.16.2
```
There might be a need to create a virtualenv. If needed, type e.g.:
```bash
conda create -n custom
source activate custom
conda install mpi4py
```
Start an interactive node (or create a job script):
```bash
interactive -t 00:30:00 -N 1 -A $ACCOUNT_NAME
```
Once in the interactive session, activate the virtual enviroment:
```bash
source activate custom
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib
```
Run the program with 2 MPI ranks:
```bash
aprun -n 2 Ni_NiO_1bath.py > output.txt 
```
If RAM memory is an issue, one can execute the program like this:
```bash
aprun -n 8 -N 2 -d 1 Ni_NiO_1bath.py > output.txt 
```
Here 8 MPI ranks are used, 2 MPI ranks per node and without OpenMP threading.

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
Load modules, e.g.:
```bash
module load Python/3.6.7-env-nsc1-gcc-2018a-eb
module load buildenv-gcc/2018a-eb
module load git
```
Some Python libraries are not installed in the loaded Python module so we have to create a virtual environment:
```bash
virtualenv --system-site-packages impurityModelEnv
```
Now a folder called `impurityModelEnv` should exist.
Activate the virtual environment (that we just created):
```bash
. impurityModelEnv/bin/activate
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
Start an interactive node (or create a job script):
```bash
interactive -t 00:30:00 -N 1 -A $ACCOUNT_NAME
```
Once in the interactive session, activate the virtual enviroment:
```bash
. impurityModelEnv/bin/activate
```
Run the script:
```bash
Ni_NiO_1bath.py
```
Run the script using MPI, with e.g. 2 MPI ranks:
```bash
mpiexec -n 2 Ni_NiO_1bath.py
```
After simulations, deactivate the virtual environment:
```bash
deactivate 
```

#### OpenMP
From experience it seems simulations run faster without OpenMP threading.
This can typically be enforced by typing:
```bash
export OMP_NUM_THREADS=1
```

## Practical help with matplotlib errors
In Python, when importing `matplotlib.pylab`, an error similar to:
```bash
ImportError: Matplotlib qt-based backends require an external PyQt4, PyQt5, PySide or PySide2 package to be installed, but it was not found.
```
might happen. In this case, a solution might be to change the backend used by matplotlib. This can be done by editing the `matplotlibrc` file (usually stored in `~/.config/matplotlib/matplotlibrc`). Search for `backend` and type e.g.:
```bash
backend      : Qt5Agg
```  
Another, more temporary, solution is to set the backend while inside the Python session:
```python
import matplotlib
matplotlib.use('Qt5Agg')
import matplotlib.pylab as plt
```

