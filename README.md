# Generative-AI-with-LLMs-Practices-on-Perlmutter
This repository is intended to share Generative AI with Large Language Models (LLMs) practices that I have performed on Perlmutter. The Jupyter codes are originated from the [online "Generative AI with LLMs" course](https://www.coursera.org/learn/generative-ai-with-llms) offered by the DeepLeanring.AI. Generative AI with LLMs refers to the use of large language models like GPT-3 for generating human-like content, spaning text, images and even code. LLMs are trained on a vast amount of data and code, and usually carefully prompt-engineered or fine-tuned to suit specific downstream tasks such as Chatbots, Translation, Question Answering and Summarization.

**Contents**
* [NERSC Perlmutter Supercomputer](#nersc-perlmutter-supercomputer)
* [Installing Conda](#installing-conda)


## NERSC Perlmutter Supercomputer
[Perlmutter](https://docs.nersc.gov/systems/perlmutter/), located at [NERSC](https://www.nersc.gov/) in [Lawrence Berkeley National Laboratory](https://www.lbl.gov/), is a HPE Cray EX supercomputer with ~1,500 AMD Milan CPU nodes and ~6000 Nvidia A100 GPUs (4 GPUs per node). It debuted as the world 5th fastest supercomputer in the Top500 list in June 2021. Please refer to [Perlmutter Architecture](https://docs.nersc.gov/systems/perlmutter/architecture/) for the architecutural details of Perlmutter including system specifications, system performance, node specifications and interconnect. [Slurm](https://slurm.schedmd.com/) is adopted for cluster/resource management and job scheduling. 

<p align="center"><img src="https://user-images.githubusercontent.com/84169368/218645916-30e920b5-b2cf-43ad-9f13-f6a2568c0e37.jpg" width=550/></p>

## Installing Conda
Once logging in to Perlmutter, you will need to have either [Anaconda](https://www.anaconda.com/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html) installed on your scratch directory. Anaconda is distribution of the Python and R programming languages for scientific computing, aiming to simplify package management and deployment. Anaconda comes with +150 data science packages, whereas Miniconda, a small bootstrap version of Anaconda, comes with a handful of what's needed.

Note that we will assume that your NERSC account information is as follows:
```
- Username: elvis
- Project Name : ddlproj
- Account (CPU) : m1234
- Account (GPU) : m1234_g
```
so, you will have to replace it with your real NERSC account information.

1. Download Anaconda or Miniconda. Miniconda comes with python, conda (package & environment manager), and some basic packages. Miniconda is fast to install and could be sufficient for distributed deep learning training practices. 
```
# (option 1) Anaconda 
perlmutter:login15>$ cd $SCRATCH 
perlmutter:login15>$ wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
```
```
# (option 2) Miniconda 
perlmutter:login15>$ cd $SCRATCH
perlmutter:login15>$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
2. Install Miniconda. By default conda will be installed in your home directory, which has a limited disk space. Please refer to [File System Overview](https://docs.nersc.gov/filesystems/) for more details of NERSC storage systems . You will install it on the /global/common/software/&lt;myproject&gt; directory and then create a conda virtual environment on your own scratch directory. 
```
perlmutter:login15>$ chmod 755 Miniconda3-latest-Linux-x86_64.sh
perlmutter:login15>$ ./Miniconda3-latest-Linux-x86_64.sh

Welcome to Miniconda3 py39_4.12.0

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>                               <======== press ENTER here
.
.
.
Do you accept the license terms? [yes|no]
[no] >>> yes                      <========= type yes here 

Miniconda3 will now be installed into this location:
/global/homes/s/swhwang/miniconda3        

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/global/homes/s/swhwang/miniconda3] >>> /global/common/software/ddlproj/elvis/miniconda3  <======== type /global/common/software/myproject/$USER/miniconda3 here
PREFIX=/global/common/software/dasrepo/swhwang/miniconda3
Unpacking payload ...
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /global/common/software/ddlproj/elvis/miniconda3
.
.
.
Preparing transaction: done
Executing transaction: done
installation finished.
Do you wish the installer to initialize Miniconda3
by running conda init? [yes|no]
[no] >>> yes         <========== type yes here
.
.
.
If you'd prefer that conda's base environment not be activated on startup,
   set the auto_activate_base parameter to false:

conda config --set auto_activate_base false

Thank you for installing Miniconda3!
```

