# LigTMap Installation Guide

## 1. System Requirements
- Ubuntu Linux system recommended

## 2. Set Project Directory
```bash
export project="/home/dxiao/ssd1/VICTR/scripts/compare/LigTMap"
cd $project
```

## 3. Clone LigTMap Repository
```bash
git clone https://github.com/ShirleyWISiu/LigTMap.git
cd LigTMap
```

## 4. Install Anaconda & Create Environment
> Note: No need to install pychem, code is added directly to ligtmap.py in this repo.

```bash
conda create -n LigTMap3 python=2.7
conda activate LigTMap3
conda install rdkit==2016.03.4 -c rdkit  # Installs boost 1.56.0
conda install -c conda-forge openbabel=3.0.0
conda install -c conda-forge scikit-learn=0.19.2
conda install -c conda-forge pandas=0.23.4
mamba install -c conda-forge openbabel=3.0.0 scikit-learn=0.19.2 pandas=0.23.4
python -m pip install pybel==0.12.2 numpy==1.11.3 oddt
```

## 5. Install MOPAC 2016
```bash
mkdir mopac
chmod 777 mopac
cd mopac
wget http://openmopac.net/MOPAC2016_for_Linux_64_bit.zip
unzip MOPAC2016_for_Linux_64_bit.zip
chmod +x MOPAC2016.exe
alias mopac="$project/mopac/MOPAC2016.exe"
export LD_LIBRARY_PATH=$project/mopac:$LD_LIBRARY_PATH
source ~/.bashrc
mopac "Example data set.mop"  # Test MOPAC installation
cd ..
```

## 6. Install Boost 1.68.0 (for libboost_python27.so.1.68.0)
```bash
wget https://jaist.dl.sourceforge.net/project/boost/boost/1.68.0/boost_1_68_0.tar.gz?viasf=1
tar -zxvf 'boost_1_68_0.tar.gz?viasf=1'
cd boost_1_68_0
./bootstrap.sh --prefix=$(pwd)
./b2 -j 4 --with-python
./b2 -j 4
./b2 install
export LD_LIBRARY_PATH=$project/boost_1_68_0/stage/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$project/boost_1_68_0/lib:$LD_LIBRARY_PATH
export PATH=$PATH:$project/boost_1_68_0
```

## 7. Install Boost 1.59.0 (for PSOVina 2.0)
> Note: RDKit installs boost 1.56.0 automatically. Three boost versions can coexist.

```bash
wget https://zenlayer.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz?viasf=1
tar -zxvf 'boost_1_59_0.tar.gz?viasf=1'
cd boost_1_59_0
./bootstrap.sh --prefix=$project/boost_1_59_0
./b2 -j 4
./b2 install
export LD_LIBRARY_PATH=$project/boost_1_59_0/lib:$LD_LIBRARY_PATH
cd ..
```

## 8. Install PSOVina 2.0
```bash
wget https://master.dl.sourceforge.net/project/psovina/psovina-2.0.tar.gz?viasf=1
tar -zxvf 'psovina-2.0.tar.gz?viasf=1'
cd psovina-2.0/build/linux/release
sed -i 's|^BASE=.*|BASE=$project/boost_1_59_0|' Makefile
export PATH=$project/psovina-2.0/build/linux/release:$PATH
make
cd ../../../
```

## 9. Install MGLTools
```bash
wget https://ccsb.scripps.edu/download/548/ -O mgltools_x86_64Linux2_1.5.6.tar.gz
tar -zxvf mgltools_x86_64Linux2_1.5.6.tar.gz
cd mgltools_x86_64Linux2_1.5.6
./install.sh
source ./initMGLtools.sh
cd ..
```

## 10. Environment Variables Setup
If you reopen the terminal, run the following commands to set up environment variables. To avoid setting them every time, add them to your `~/.bashrc` and run `source ~/.bashrc`.

> **Warning:** Do NOT add `mgltools_x86_64Linux2_1.5.6/bin` to PATH, as it may cause Python version conflicts (python2.5 instead of python2.7).

```bash
export project="/home/dxiao/ssd1/VICTR/scripts/compare/LigTMap"  # Set your project directory
export PATH="$HOME/.local/bin:$PATH"
alias mopac="$project/mopac/MOPAC2016.exe"
export LD_LIBRARY_PATH=$project/mopac:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$project/boost_1_68_0/stage/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=$project/boost_1_68_0/lib:$LD_LIBRARY_PATH
export PATH=$project/boost_1_68_0:$PATH
export LD_LIBRARY_PATH=$project/boost_1_59_0/lib:$LD_LIBRARY_PATH
export PATH=$project/psovina-2.0/build/linux/release:$PATH
alias pmv="$project/mgltools_x86_64Linux2_1.5.6/bin/pmv"
alias adt="$project/mgltools_x86_64Linux2_1.5.6/bin/adt"
alias vision="$project/mgltools_x86_64Linux2_1.5.6/bin/vision"
alias pythonsh="$project/mgltools_x86_64Linux2_1.5.6/bin/pythonsh"
source "$project/mgltools_x86_64Linux2_1.5.6/initMGLtools.sh"
export LIGTMAP="$project"
export MGLTools="$project/mgltools_x86_64Linux2_1.5.6"
```

