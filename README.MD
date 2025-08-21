# Install Implementation of the LigTMap
- Install in Ubuntu linux system
- Set your project directory in alias, all the data and scripts will be in this directory:
`    exprot project='/home/dxiao/ssd1/VICTR/scripts/compare/LigTMap'
    cd $project`
- Clone the LigTMap repository:
`    git clone https://github.com/xdn-github/LigTMap.git
    cd LigTMap`
- Install Anaconda and create a new environment(Advised to use mamba to speed up the installation):
Notice: don't need to install pychem as I add code to ligtmap.py directly in my repo instead of the original one.
`    conda create -n LigTMap3 python=2.7
    conda activate LigTMap3
    conda install rdkit==2016.03.4 -c rdkit  # (by the way install boost 1.56.0)
    conda install -c conda-forge openbabel=3.0.0
    conda install -c conda-forge scikit-learn=0.19.2
    conda install -c conda-forge pandas=0.23.4
    mamba install -c conda-forge openbabel=3.0.0 scikit-learn=0.19.2 pandas=0.23.4
    python -m pip install pybel==0.12.2
    python -m pip install numpy==1.11.3
    python -m pip install oddt
    python -m pip install pybel==0.12.2 numpy==1.11.3 oddt`
- Install MOPAC 2016
`    mkdir mopac
    chmod 777 mopac
    cd mopac
    wget http://openmopac.net/MOPAC2016_for_Linux_64_bit.zip
    unzip MOPAC2016_for_Linux_64_bit.zip
    chmod +x MOPAC2016.exe
    alias mopac="$project/mopac/MOPAC2016.exe"
    export LD_LIBRARY_PATH=$project/mopac:$LD_LIBRARY_PATH
    source ~/.bashrc
    mopac "Example data set.mop"  # Test MOPAC installation
    cd ..  # Go back to the LigTMap directory`
- Idk who needs libboost_python27.so.1.68.0 but just install boost 1.68.0 and the bug will be fixed.
`    wget https://jaist.dl.sourceforge.net/project/boost/boost/1.68.0/boost_1_68_0.tar.gz?viasf=1
    tar -zxvf 'boost_1_68_0.tar.gz?viasf=1'
    cd boost_1_68_0
    ./bootstrap.sh --prefix=$(pwd)
    ./b2 -j 4 --with-python
    ./b2 -j 4
    ./b2 install
    export LD_LIBRARY_PATH=$project/boost_1_68_0/stage/lib:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=$project/boost_1_68_0/lib:$LD_LIBRARY_PATH
    export PATH=$PATH:$project/boost_1_68_0`
- Install boost 1.59.0 for PSOVina 2.0, Notice rdkit install boost 1.56.0 while installing, it just works to install three versions of boost.
`    wget https://zenlayer.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz?viasf=1
    tar -zxvf 'boost_1_59_0.tar.gz?viasf=1'
    cd boost_1_59_0
    ./bootstrap.sh --prefix=$project/boost_1_59_0
    ./b2 -j 4
    ./b2 install
    export LD_LIBRARY_PATH=$project/boost_1_59_0/lib:$LD_LIBRARY_PATH
    cd ..  # Go back to the LigTMap directory`
- Install PSOVina 2.0
`    wget https://master.dl.sourceforge.net/project/psovina/psovina-2.0.tar.gz?viasf=1
    tar -zxvf 'psovina-2.0.tar.gz?viasf=1'
    cd psovina-2.0/build/linux/release
    sed -i 's|^BASE=.*|BASE=$project/boost_1_59_0|' Makefile
    export PATH=$project/psovina-2.0/build/linux/release:$PATH
    make
    cd ../../../  # Go back to the LigTMap directory`
- Install MGLTools
`    wget https://ccsb.scripps.edu/download/548/ -O mgltools_x86_64Linux2_1.5.6.tar.gz
    tar -zxvf mgltools_x86_64Linux2_1.5.6.tar.gz
    cd mgltools_x86_64Linux2_1.5.6
    ./install.sh
    source ./initMGLtools.sh
    cd ..  # Go back to the LigTMap directory`
- If re-open the terminal, you need to run the following commands to set up the environment variables
If u wanna not set the environment variables every time, you can add the above commands to your ~/.bashrc file.
and then run 'source ~/.bashrc' to apply the changes:
Notice: don't add mgltools_x86_64Linux2_1.5.6/bin to PATH, because it will cause python conflict with Anaconda.
let python become python2.5 instead of python2.7.
`    exprot project='/home/dxiao/ssd1/VICTR/scripts/compare/LigTMap'  # Set your project directory
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
    export MGLTools="$project/mgltools_x86_64Linux2_1.5.6"`