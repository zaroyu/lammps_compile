# lammps_compile
Other README file is for the LAMMPS install.
nequip_env.yml is a conda environment and should be loaded. It should include some packages installed via pip, such as torch, but it may not.

In addition to the packages in the conda export nequip_env.yml, the following modules are loaded in .bash_profile:
module load compilers/intel/2021.1 mpi/impi/2021.1 libs/mkl/2021.1 libs/cuda/11.3 utility/standard/cmake/3.19.6 compilers/gcc/9.3.1

Additionally, a python module is loaded to activate the conda environment nequip_env, after which the python module is unloaded.

Instructions are taken from [this page](https://github.com/mir-group/nequip/tree/develop), which may prove useful. However, they are slightly modified, and some of the modifications to the source code have already been made, so the specifics are below:

First, a build directory is made and entered (if one already exists, it should be removed).
Then, the build directory is made using
    mkdir build
and it is entered with
    cd build

After this, a cmake command is run:
    cmake ../cmake -DMKL_INCLUDE_DIR=`python -c "import sysconfig;from pathlib import Path;print(Path(sysconfig.get_paths()[\"include\"]).parent)"` -DCMAKE_PREFIX_PATH=`python -c 'import torch;print(torch.utils.cmake_prefix_path)'`

Once this has completed, while still in the build directory, the following is run:
    make -j$(nproc)