# Generative-AI-with-LLMs-Practices-on-Perlmutter
Generative AI with LLMs refers to the use of large language models like GPT-3 for generating human-like content, spaning text, images and even code. LLMs are trained on a vast amount of data and code, and usually carefully prompt-engineered or fine-tuned to suit specific downstream tasks such as Chatbots, Translation, Question Answering and Summarization.

This repository is intended to share Generative AI with Large Language Models (LLMs) practices that I have performed on Perlmutter. The Jupyter codes are originated from the online [Generative AI with LLMs](https://www.coursera.org/learn/generative-ai-with-llms) course offered by the DeepLeanring.AI. Generative AI with LLMs refers to the use of large language models like GPT-3 for generating human-like content, spaning text, images and even code. LLMs are trained on a vast amount of data and code, and usually carefully prompt-engineered or fine-tuned to suit specific downstream tasks such as Chatbots, Translation, Question Answering and Summarization.

**Contents**
* [NERSC Perlmutter Supercomputer](#nersc-perlmutter-supercomputer)
* [Installing Conda](#installing-conda)
* [Creating a Conda Virtual Environment](#creating-a-conda-virtual-environment)
* [Running Jupyter](#running-jupyter)
* [Building a GenAI Shifter Container Image](#building-a-genai-shifter-container-image)
* [Running Jupyter with the Shifter Container for Generative AI Practices](#running-jupyter-with-the-shifter-container-for-generative-ai-practices)
* [Lab Excecises](#lab-exercises)
* [Reference](#reference)

 
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
2. Install Miniconda. By default conda will be installed in your home directory, which has a limited disk space. Please refer to [File System Overview](https://docs.nersc.gov/filesystems/) for more details of NERSC storage systems . You will install it on your scratch directory (/pscratch/sd/s/elvis) and then create a conda virtual environment there. 
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

[/global/homes/s/swhwang/miniconda3] >>> /pscratch/sd/s/elvis/miniconda3  <======== type /pscratch/sd/s/elvis/miniconda3 or $SCRATCH/miniconda3 here
PREFIX=/pscratch/sd/s/elvis/miniconda3
Unpacking payload ...
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /pscratch/sd/s/elvis/miniconda3
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
3. finalize installing Miniconda with environment variables set including conda path.
```
perlmutter:login15>$ source ~/.bashrc    # set conda path and environment variables 
perlmutter:login15>$ conda config --set auto_activate_base false
perlmutter:login15>$ which conda
/pscratch/sd/s/elvis/miniconda3/condabin/conda
perlmutter:login15>$ conda --version
conda 23.9.0
```
## Creating a Conda Virtual Environment
You want to create a virtual envrionment with a python version 3.10 for Generative AI Practices.
```
[glogin01]$ conda create -n genai python=3.10
Retrieving notices: ...working... done
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /pscratch/sd/s/elvis/miniconda3/envs/genai

  added / updated specs:
    - python=3.10
.
.
.
Proceed ([y]/n)? y    <========== type yes 


Downloading and Extracting Packages:

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate genai
#
# To deactivate an active environment, use
#
#     $ conda deactivate
```

## Running Jupyter
[Jupyter](https://jupyter.org/) is free software, open standards, and web services for interactive computing across all programming languages. Jupyterlab is the latest web-based interactive development environment for notebooks, code, and data. The Jupyter Notebook is the original web application for creating and sharing computational documents. NERSC provides a [JupyterHub service](https://docs.nersc.gov/services/jupyter/#jupyterhub) that allows you to run a Jupyter notebook on Perlmutter. JupyterHub is designed to serve Jupyter notebook for multiple users to work in their own individual workspaces on shared resources. Please refer to [JupyterHub](https://jupyterhub.readthedocs.io/en/stable/) for more detailed information. You can also run your own jupyter notebook server on a compute node (*not* on a login node), which will be accessed from the browser on your PC or laptop through SSH tunneling. 
<p align="center"><img src="https://github.com/hwang2006/KISTI-DL-tutorial-using-horovod/assets/84169368/34a753fc-ccb7-423e-b0f3-f973b8cd7122"/>
</p>

In order to do so, you need to add the horovod-enabled virtual envrionment that you have created as a python kernel.
1. activate the horovod-enabled virtual environment:
```
perlmutter:login15>$ conda activate genai
```
2. install Jupyter on the virtual environment:
```
(genai) perlmutter:login15>$ conda install jupyter chardet cchardet
(genai) perlmutter:login15>$ pip install jupyter-tensorboard
```
3. add the virtual environment as a jupyter kernel:
```
(genai) perlmutter:login15>$ pip install ipykernel 
(genai) perlmutter:login15>$ python -m ipykernel install --user --name genai
```
4. check the list of kernels currently installed:
```
(genai) perlmutter:login15>$ jupyter kernelspec list
Available kernels:
  pytho       /global/common/software/ddlproj/evlis/miniconda3/envs/craympi-hvd/share/jupyter/kernels/python3
  horovod     /global/u1/s/elvis/.local/share/jupyter/kernels/horovod
```
5. launch a jupyter notebook server on a compute node 
- to deactivate the virtual environment
```
(genai) perlmutter:login15>$ conda deactivate
```
- to create a batch script for launching a jupyter notebook server: 
```
perlmutter:login15>$ cat jupyter_run.sh
#!/bin/bash
#SBATCH -A m1234_g
#SBATCH -C gpu
#SBATCH -q regular
#SBATCH -t 8:00:00
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --gpus-per-node=1
#SBATCH -c 32

export SLURM_CPU_BIND="cores"

#removing the old port forwading
if [ -e port_forwarding_command ]
then
  rm port_forwarding_command
fi

#getting the node name and port
SERVER="`hostname`"
PORT_JU=$(($RANDOM + 10000 )) # random number greaten than 10000

echo $SERVER
echo $PORT_JU

echo "ssh -L localhost:8888:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov" > port_forwarding_command
echo "ssh -L localhost:8888:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov"
#echo "ssh -L localhost:${PORT_JU}:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov" > port_forwarding_command
#echo "ssh -L localhost:${PORT_JU}:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov"

echo "load module-environment"
module load  cudnn/8.7.0  nccl/2.15.5-ofi  evp-patch

echo "execute jupyter"
source ~/.bashrc
#conda activate genai
#cd $SCRATCH/ddl-projects #Root/Working directory for the jupyter notebook server
cd $SCRATCH  #Root/Working directory of the jupyter notebook server
jupyter lab --ip=0.0.0.0 --port=${PORT_JU} --NotebookApp.token=${USER}
#bash -c "jupyter lab --ip=0.0.0.0 --port=${PORT_JU} --NotebookApp.token='${USER}'"
echo "end of the job"
```
- to submit and launch a jupyter notebook server on a worker node
```
perlmutter:login15>$ sbatch jupyter_run.sh
Submitted batch job 5494200
```
- to check if the jupyter notebook server is up and running
```
perlmutter:login15>$ squeue -u $USER
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            5494200  gpu_ss11 jupyter_   elvis PD       0:00      1 (Priority)
perlmutter:login15>$ squeue -u $USER
             JOBID       PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
            5494200       gpu_ss11  jupyter_    elvis  RUNNING       0:02   8:00:00      1 nid001140
perlmutter:login15>$ cat slurm-XXXXXX.out
.
.
[I 2023-02-14 08:30:04.790 ServerApp] Jupyter Server 1.23.4 is running at:
[I 2023-02-14 08:30:04.790 ServerApp] http://nid######:#####/lab?token=...
.
.
```
- to check the SSH tunneling information generated by the jupyter_run.sh script 
```
perlmutter:login15>$ cat port_forwarding_command
ssh -L localhost:8888:nid######:##### $USER@neuron.ksc.re.kr
```
6. open a SSH client (e.g., Putty, PowerShell, Command Prompt, etc) on your PC or laptop and log in to Perlmutter just by copying and pasting the port_forwarding_command:
```
C:\Users\hwang>ssh -L localhost:8888:nid######:##### elvis@perlmutter-p1.nersc.gov
Password(OTP):
Password:
```
7. open a web browser on your PC or laptop to access the jupyter server
```
- URL Address: localhost:8888
- Password or token: elvis   # your username on Perlmutter
```
<p align="center"><img src="https://user-images.githubusercontent.com/84169368/218938419-f38c356b-e682-4b1c-9add-6cfc29d53425.png"/></p> 

## Building a GenAI Shifter Container Image
You can build a [Shifter](https://docs.nersc.gov/development/shifter/how-to-use) container image for Generativ AI practices. NERSC currently does not provide users with a platform to create their own Docker containers. Consequently, you are required to construct your images on your personal systems (e.g., PC or Laptop) before transferring them to NERSC. Please refer to the [Shifter for Beginner Tutorial](https://docs.nersc.gov/development/shifter/shifter-tutorial/#push-your-docker-image-to-the-docker-hub-registry) for more detailed information. 
- edit a Dockerfile:
```
C:\Users\elvis> cat Dockerfile
FROM nvcr.io/nvidia/pytorch:22.09-py3

#install miniconda
#pin to python 3.10 for rapids compatibility
#https://repo.anaconda.com/miniconda/Miniconda3-py310_23.9.0-0-Linux-x86_64.sh
ENV installer=Miniconda3-py310_23.9.0-0-Linux-x86_64.sh

RUN wget https://repo.anaconda.com/miniconda/$installer && \
    /bin/bash $installer -b -p /opt/miniconda3          && \
    rm -rf $installer

ENV PATH=/opt/miniconda3/bin:$PATH

RUN \
    conda install jupyter chardet cchardet -y                   && \
    conda install -c conda-forge jupytext -y                    && \
    pip install jupyter-tensorboard ipykernel                   && \
    pip install torch==1.13.0 torchdata transformers datasets   && \
    pip install evaluate rouge_score loralib peft               && \
    pip install git+https://github.com/lvwerra/trl.git@25fa1bd
```
- build a docker image:
```
C:\Users\elvis> docker build -t qualis2006/genai-pytorch-22.09-py3 .
```
- list docker images:
```
C:\Users\elvis> docker images
REPOSITORY                                      TAG       IMAGE ID       CREATED         SIZE
qualis2006/genai-pytorch-22.09-py3              latest    c4afb5c76399   9 minutes ago   22.9GB
```
- push your Docker image to the Docker Hub registry. You'll have to access the Docker public registry by using docker login, where you'll be prompted to enter your username and password:
```
# log in to the Docker Hub
C:\Users\elvis> docker login

# push your image to the Docker Hub
C:\Users\elvis> docker push qualis2006/genai-pytorch-22.09-py3:latest
```
- pull your Docker image onto NERSC via Shifter. Shifter is configured to retrieve images directly from Docker Hub using the shifterimg pull command, which seamlessly converts your Docker image into the Shifter format:
```
# note that you are on a login node on Perlmutter.
perlmutter:login15>$ shifterimg pull qualis2006/genai-pytorch-22.09-py3:latest

# make sure that your image is pulled successfully at NERSC
perlmutter:login15>$ shifterimg images | grep qualis2006
perlmutter docker     READY    8002b54070   2023-11-05T00:42:30 qualis2006/genai-pytorch-22.09-py3:latest
```

## Running Jupyter with the Shifter Container for Generative AI Practices
You can launch a Jupyter notebook server using the GenAI container image by submitting and running it on a compute node. You can then access it through the SSH tunneling mechanizm by opening a browser on your PC or labtop. Please be aware that with the Shifter container image, there is no need to install the Miniconda3 on your scratch directory and build the conda virtual environment for Generative AI practices. 
- create a batch script for launching a jupyter notebook server:
```
perlmutter:login15>$  cat jupyter_run_shifter.sh
#!/bin/bash
#SBATCH --image=qualis2006/genai-pytorch-22.09-py3:latest
#SBATCH -A m1234_g
#SBATCH -C gpu
##SBATCH -q regular
#SBATCH -q shared
#SBATCH -t 8:00:00
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --gpus-per-node=1
#SBATCH -c 32

export SLURM_CPU_BIND="cores"

#removing the old port forwading
if [ -e port_forwarding_command ]
then
  rm port_forwarding_command
fi

#getting the node name and port
SERVER="`hostname`"
PORT_JU=$(($RANDOM + 10000 )) # random number greaten than 10000

echo $SERVER
echo $PORT_JU

echo "ssh -L localhost:8888:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov" > port_forwarding_command
echo "ssh -L localhost:8888:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov"
#echo "ssh -L localhost:${PORT_JU}:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov" > port_forwarding_command
#echo "ssh -L localhost:${PORT_JU}:${SERVER}:${PORT_JU} ${USER}@perlmutter-p1.nersc.gov"

echo "load module-environment"
module load  cudnn/8.7.0  nccl/2.15.5-ofi  evp-patch

echo "execute jupyter"
source ~/.bashrc
#conda activate genai
#cd $SCRATCH/ddl-projects #Root/Working directory for the jupyter notebook server
cd $SCRATCH  #Root/Working directory for the jupyter notebook server
shifter --module=gpu jupyter lab --no-browser --ip=0.0.0.0 --port=${PORT_JU} --NotebookApp.token=${USER}
#bash -c "jupyter lab --ip=0.0.0.0 --port=${PORT_JU} --NotebookApp.token='${USER}'"
echo "end of the job"
```
- launch a jupyter notebook server by submitting the batch script to a worker node:
```
perlmutter:login15>$ sbatch jupyter_run_shifter.sh
Submitted batch job XXXXXX
```
- check if the jupyter notebook server is up and running:
```
perlmutter:login15>$ squeue -u $USER
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
          XXXXXXXX shared_gp jupyter_    elvis  R   00:00:10      1 nid001512
```
## Lab Exercises
Now, you are ready to do Generative AI with LLMs practices. After cloning this GitHub repository on your scratch directory (e.g., $PSCRATCH), you should able to see the lab exercised files through the Jupyter Notebook interface that you have opened above. You could start with the *Lab_1_summarize_dialogue.ipynb* just by clickihng it that covers prompting and prompt engineering practices. Instruction and LoRA PEFT fine-tunings are discussed in Lab2 and RLHF practices are in Lab3.  

![20240112_151939](https://github.com/hwang2006/Generative-AI-with-LLMs/assets/84169368/9099a04b-f95c-4dd2-b234-caa4497d0484)

## Reference
[[DeepLearning.AI Online Course] Generative AI with Large Language Models](https://www.coursera.org/learn/generative-ai-with-llms)  
