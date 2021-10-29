---
layout: page
title: Virtual Environments
---

This is a brief walk-through on how to set up conda virtual environments.  The specific use case here is the diffusion imaging pre-processing pipeline.

### Preliminaries

This all assumes that you are using the bash shell and your PATH is pointing to the Anaconda distribution.  For the UCAIR servers, edit your `.bash_profile` file to include
```
export PATH=/APPS/anaconda3/bin:${PATH}
```

### Setting up a fresh virtual environment

Sure, you could start with an existing virtual environment.  But if you are wanting to start fresh, we will start here.

```terminal
conda create --name pipeline python=3.6
source activate pipeline
```

Note, we could use any name for this environment.  I just chose `pipeline` for this example.  Also, for this example I chose python 3.6.  You could choose other versions.  I just like 3.6.  Also, I reference the `$HOME` variable, which is just your home directory.  

### Installing mrtrix3

This is where things get a bit more complicated.  First, we need to clone this to somewhere convenient.  I use a folder named `scratch/` for my code building.

```terminal
cd ${HOME}/scratch
git clone https://github.com/MRtrix3/mrtrix3.git
cd mrtrix3
```

Since we are working on CentOS, we are natively given some pretty ancient compilers (running `gcc --version` tells we are running 4.8.5).  We need something more modern to compile this code.  We can get more modern GNU compilers through a quick `conda install`.  Specifically, we need to get C++ compilers.  Thus, we run

```terminal
conda install -c conda-forge gxx_impl_linux-64 
```
This will install the necessary binaries in `${HOME}/.conda/envs/pipeline/bin`.  However, notice that the GNU people are careful not to shadow the default `g++` compiler so the binaries are called  `x86_64-conda_cos6-linux-gnu-gcc` and `x86_64-conda_cos6-linux-gnu-g++`.

But we aren't done with compilers.  Next we need to set the appropriate variable names so the `configure` file actually finds them.  To do this

```terminal
export CXX=${HOME}/.conda/envs/pipeline/bin/x86_64-conda_cos6-linux-gnu-g++
```

I have ran into issues before where it fails when looking for the compiler because the `LD` variable set.  If that happens, just unset by `unset LD`.  Check this works by `echo $LD`.

The next step is to install the eigen linear algebra package.  This can be found on the conda-forge channel.  Please also set the `EIGEN_CFLAGS` path as well.

```terminal
conda install -c conda-forge eigen
export EIGEN_CFLAGS="-isystem ${HOME}/.conda/envs/pipeline/include/eigen3"
```

This software also needs the zlib library as well.  This should be installed into your virtual environment when you set it up, but you need to make sure the mrtrix install can find it.  You will need to set the include and library flags for zlib.  

```terminal
export ZLIB_CFLAGS="-isystem ${HOME}/.conda/envs/pipeline/include"
export ZLIB_LDFLAGS="-L${HOME}/.conda/envs/pipeline/lib -lz"
```

Now this is all set up, we are ready to build.  Mrtrix (unfortunately) runs on a `configure` file (as opposed to `cmake`, which is superior).  Note:  we can't run the GUI stuff over a remote connection so we will compile with the `-nogui` flag.  If you don't turn on this flag, you will run into issues with missing qt files.  

```terminal
./configure -nogui
./build
```

Finally, we need to move these files into the virtual environment so our system can actually find them.  The official way of doing this is a bit more complicated and would be clean if we were working on a system-wide install, but for a virtual environment the following should be totally sufficient.

```terminal
cp -r bin/* ${HOME}/.conda/envs/pipeline/bin/
cp -r lib/* ${HOME}/.conda/envs/pipeline/lib/
cp -r share/* ${HOME}/.conda/envs/pipeline/share/
```

And that's it!  At this point you can delete `scratch/mrtrix3` if you want.  

### FSL installation

The official FSL documentation isn't great.  Here is what I did.  First, I added a directory in `${HOME}/scratch/` to work in.

```terminal
mkdir ${HOME}/scratch/fsl_install
cd ${HOME}/scratch/fsl_install
```

The installation itself takes time, but isn't too painful.  First, download the installer.  Then run it.  However, you need to use python 2.X.  So if you built your virtual environment with python 3.X, you will need to modify your python call a bit.

```terminal
wget https://fsl.fmrib.ox.ac.uk/fsldownloads/fslinstaller.py
python2.7 fslinstaller.py
```

It will take about an hour to run the installation procedure.  During the install you will be prompted for the install directory for `fsl`.  The entire FSL suite is huge (around 3.5 Gb), so don't put it in your `${HOME}` directory otherwise you will immediately eat up your storage.  