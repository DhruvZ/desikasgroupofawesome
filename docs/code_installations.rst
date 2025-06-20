Code Installation Notes
**********
.. contents:: Section Contents
    :local:

Powderday on HiPerGator 
============



Manual Installation With gcc Compilers
-----------------

[1] As of August 2022, this is the preferred way to install (i.e., least headache).  Thanks to Prerak Garg for these.::

First, load up the compilers that we'll use throughout::

  >module load gcc/9.3.0 openmpi/4.1.1 libz/1.2.11 hdf5/1.10.1 git/2.30.1

  
yt::

  >cd $HOME
  >git clone https://github.com/yt-project/yt
  >cd yt
  >pip install -e .



fsps and python-fsps

The development version of python-fsps now includes the Fortran FSPS source code. You can get both via::

fsps::

  >cd $HOME
  >git clone https://github.com/cconroy20/fsps
  >cd fsps/src

in the Makefile set F90=$(FC) and this will ensure that the compilers
`fsps <https://code.google.com/p/fsps/source/checkout>`_ uses are what
you have module loaded.::
  
  >make clean
  >make

then in your .bashrc set the analog to::
  
  >export SPS_HOME=/Users/desika/fsps


python fsps::

>CC=gcc F90=gfortran F77=gfortran python setup.py install



hyperion

As of commit 7cae6d0, a bug has been introduced with the __version__ module. Once cloned, checkout stable commit 4170c6c::

  >cd $HOME
  >git clone https://github.com/hyperion-rt/hyperion.git
  >cd hyperion
  >git checkout 4170c6cc3009893e2b591e133baeb9927122aef1
  >python setup.py install
  >git submodule init
  >git submodule update

  >./configure --prefix=$HOME/local

  >make
  >make install

hyperion dust::

  >cd $HOME
  >wget http://pypi.python.org/packages/source/h/hyperion-dust/hyperion-dust-0.1.0.tar.gz
  >tar -xzvf hyperion-dust-0.1.0.tar.gz
  >cd hyperion-dust-0.1.0
  >python setup.py build_dust

  
powderday::

  >git clone https://github.com/dnarayanan/powderday.git
  >conda install numpy scipy cython h5py matplotlib psutil joblib six astropy scikit-learn ipython
  >cd powderday
  >python setup.py install

  


Manual Installation With Intel Compilers (updated for el8 installation by Dhruv as of 9/13/2023)
-----------------

[2] The first set of instructions for the University of Florida
HiPerGator3.0 facility is to employ intel compilers, and to compile
everything manually.  This allows the greatest flexibility, as well as
the ability to use private forks of individual codes.

First, load up the compilers that we'll use throughout::

  >module load intel/2020.0.166
  >module load openmpi/4.1.5
  >module load hdf5/1.14.1
  >module load git

yt::

  >cd $HOME
  >git clone https://github.com/yt-project/yt
  >cd yt
  >pip install -e .

python fsps and fsps::

  >cd $HOME
  >git clone --recursive https://github.com/dfm/python-fsps.git
  >cd python-fsps
  >cd src/fsps/libfsps/src
  >make clean
  >make

python-fsps now comes pre-packaged with fsps, so we don't need to install them separately anymore. The --recursive option on the git clone pulls the appropriate version of fsps as well. First let's compile fsps. In the Makefile set F90=$(FC) and this will ensure that the compilers
`fsps <https://code.google.com/p/fsps/source/checkout>`_ uses are what
you have module loaded.

Set in your .bashrc the analog to::

  >export SPS_HOME=$HOME/python-fsps/src/fsps/libfsps

Now we can install python-fsps::

  >cd $HOME/python-fsps
  >CC=icc F90=ifort python setup.py install


hyperion::

  >cd $HOME
  >git clone https://github.com/hyperion-rt/hyperion.git
  >cd hyperion
  >python setup.py install
  >git submodule init
  >git submodule update
  >./configure --prefix=$HOME/local
  >make
  >make install

Make sure that whatever directory you put for configure is in your $PATH. You can check whether this is true by typing $PATH in the command line and looking for the <location>/bin directory.

hyperion dust::

  >cd $HOME
  >wget http://pypi.python.org/packages/source/h/hyperion-dust/hyperion-dust-0.1.0.tar.gz
  >tar -xzvf hyperion-dust-0.1.0.tar.gz
  >cd hyperion-dust-0.1.0
  >python setup.py build_dust
  
powderday::

  >git clone https://github.com/dnarayanan/powderday.git
  >cd powderday
  >python setup.py install

Installations With Intel Compilers (updated for el9 installation by Dhruv as of 6/10/2025)
-----------------

[3] The set of instructions for the University of Florida
HiPerGator3.0 facility to employ intel compilers, and to compile
everything. This installation will list versions and commits where appropriate. This relies more on pip installs than previous attempts. The python version used for this installation is python 3.10.18. 

First, load up the compilers that we'll use throughout::

  >module load intel/2025.1.0
  >module load openmpi/5.0.7
  >module load hdf5/1.14.6
  >module load git

miscellaneous packages::

  >conda install numpy scipy cython h5py matplotlib psutil joblib six astropy scikit-learn ipython
  >pip install synphot extinction
  >conda install mpi4py

yt (version 4.4.0)::

  >cd $HOME
  >pip install yt

pygadgetreader (commit bea9c958592b6435d8fd907007e19a15be76486a)::

  >cd $HOME
  >git clone https://github.com/dnarayanan/pygadgetreader.git
  >cd pygadgetreader
  >python setup.py install

caesar (commit c7139f6adc6f4ad10ee5d2827db8407c0445aa0d)::

  >cd $HOME
  >git clone https://github.com/dnarayanan/caesar.git
  >cd caesar
  >python setup.py install

python fsps and fsps (version 0.4.7,commit 30c1fec374ffccb4556d32c9f1ddb73789b06ad9)::

  >cd $HOME
  >git clone --recursive https://github.com/dfm/python-fsps.git
  >cd python-fsps
  >git submodule init
  >git submodule update
  >cd $HOME
  >python -m pip install fsps

python-fsps now comes pre-packaged with fsps, so we don't need to install them separately anymore. The --recursive option on the git clone pulls the appropriate version of fsps as well. Here I've installed it with pip because of issues in the past, but there is everything you need to do the manual installation in this download.

Set in your .bashrc the analog to::

  >export SPS_HOME=$HOME/python-fsps/src/fsps/libfsps


hyperion (Desika's fork, commit 434191330f5d4c7f9d2dd55b6420bb039f97c6f4)::

  >cd $HOME
  >git clone https://github.com/dnarayanan/hyperion.git
  >cd hyperion
  >git submodule init
  >git submodule update
  >pip install ".[recommended]"
  >conda deactivate
  >module load ufrc
  >./configure --prefix=$HOME/<location>
  >make
  >make install
  >module unload ufrc

Make sure that whatever directory you put for configure is in your $PATH. You can check whether this is true by typing $PATH in the command line and looking for the <location>/bin directory. The ufrc step is to prevent compiling the binaries in an environment to prevent unexpected behavior. Reactivate your environment at this point.

hyperion dust (commit 66b04df0d06bbd2338ad180bb0ed247cc8d1fe29)::

  >cd $HOME
  >git clone https://github.com/hyperion-rt/hyperion-dust.git
  >cd hyperion-dust
  >python setup.py build_dust
  >cd dust_files
  >wget https://github.com/hyperion-rt/paper-galaxy-rt-model/blob/master/dust/big.hdf5
  >wget https://github.com/hyperion-rt/paper-galaxy-rt-model/blob/master/dust/vsg.hdf5
  >wget https://github.com/hyperion-rt/paper-galaxy-rt-model/blob/master/dust/usg.hdf5
  
powderday (commit 00e0fae87e2751ab7a9dcc5760368b818fb1647f) ::

  >git clone https://github.com/dnarayanan/powderday.git
  >cd powderday
  >python setup.py install





  

Conda Installation
-----------------
  
[4] The final set of instructions use gcc, and the conda installation
of `Hyperion <https://docs.hyperion-rt.org/en/stable/index.html>`_.  Thanks to Paul Torrey
for these.::

  >module load openmpi/4.1.1 libz/1.2.11 hdf5/1.10.1 conda/4.12.0 git/2.30.1 gcc
  >conda install -c conda-forge hyperion
  >python -c "import hyperion" (just to ensure no errors thrown)
  >hyperion (just to ensure command is found)
  >python -m pip install fsps
  >[set $SPS_HOME variable in .bashrc)
  >cd $HOME
  >git clone https://github.com/dnarayanan/powderday.git
  >cd powderday
  >python setup.py install

then fix import six line in the equivalent of all of these::

  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/model/model.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/util/validator.py 
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/conf/conf_files.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/filter/filter.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/dust/dust_type.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/model/model_output.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/densities/flared_disk.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/densities/alpha_disk.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/densities/bipolar_cavity.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/densities/ulrich_envelope.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/densities/power_law_envelope.py 
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/densities/ambient_medium.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/model/sed.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/model/image.py
  >vi /home/paul.torrey/.conda/envs/pd_gcc/lib/python3.8/site-packages/hyperion/grid/yt3_wrappers.py



Caesar on HiPerGator
============


AREPO
============

The current best set of modules to compile AREPO for hpg2-default are::

  
  module purge
  module load intel/2020.0.166
  module load openmpi/4.1.5
  module load python/3.11.4
  module load fftw/3.3.10
  module load hdf5/1.14.1
  module load grackle/3.2.1
  module load gsl/2.6
  

Alternatively the best set of modules for hpg-default are::

  module load gcc/9.3.0 openmpi/4.1.1 gsl/2.6  fftw

following this::

  make build
  
GIZMO
============
