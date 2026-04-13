# High-Performance Computing (TSCC) Usage Guide

## Introduction

Welcome to the Triton Shared Computing Cluster (TSCC) Usage Guide! This manual is crafted to assist students in effectively harnessing the power of TSCC for computational needs.

## Getting Started

### Accessing the Cluster

#### Step 1: Account Activation
- Confirm the activation of your account by consulting with the TAs for your username.
- Your password aligns with your active directory credentials.
- Duo authentication is a prerequisite.

#### Step 2: Cluster Connection via SSH
- **Windows Users:** Utilize [MobaXTerm](https://mobaxterm.mobatek.net/) for SSH connectivity.
  1. Download [MobaXTerm](https://mobaxterm.mobatek.net/download.html) (free version, installer edition)
  2. Unzip (extract all) MobaXterm_Installer_v26.2.zip
  3. Go into the MobaXterm_Installer_v26.2 directory and double click on MobaXterm_installer_26.2.msi
  4. Go through the installation steps.
  5. Open MobaXTerm and click on the "Session" tab.
  6. Click on the SSH tab.
  7. Type in the following into the basic SSH settings:
    - **Remote Host:** login.tscc.sdsc.edu
    - **Username:** Please refer to the TAs for this information.
  8. Click Ok.

- **Command Line Users (Mac/Linux):**
  - Initiate a connection with: `ssh yourUsername@login.tscc.sdsc.edu`

### Step 3: Anaconda Installation
#### Download
- Retrieve the Anaconda installation script using: 
  `curl -O https://repo.anaconda.com/archive/Anaconda3-2025.12-2-Linux-x86_64.sh`

#### Installation
- Execute the script: `bash Anaconda3-2025.12-2-Linux-x86_64.sh`
- Follow the on-screen prompts to install anaconda into an "anaconda3" folder in your local directory.
- Do you want to automatically initialize > yes
- Type `source ~/.bashrc`
- Type "conda" to verify installation

### Step 4: Creating an Anaconda Environment
- **Command:** `conda create -n python_3_11 python=3.11 -y`
  - **Explanation:** This command creates a new environment named "python_3_11" with Python 3.11.
- **Command:** `conda activate python_3_11`
  - **Explanation:** This command activates the environment "python_3_11".
- **Command:** `conda deactivate`
  - **Explanation:** This command deactivates the current environment.

### Step 5: Batch Job Submission
- Example: Executing a "Hello World" bash script.
1. Download run_hello.sb under Day_1 github directory (click on the file and click on the download icon in the top right).
2. Go to the directory where you downloaded run_hello.sb in the terminal.
3. `scp run_hello.sb yourUsername@login.tscc.sdsc.edu:/tscc/nfs/home/yourUsername/`
4. `sbatch run_hello.sb`
5. Type `ls`. You should see a newly created "hello_world.txt" file.
6. Type `head hello_world.txt`. You should see "Hello World" printed on the screen.

### Step 6: Interactive Node Request
- Command: `srun --partition=hotel --pty --nodes=1 --ntasks-per-node=1 --mem 2G -t 00:30:00 -A htl179 --qos=hotel --wait=0 --export=ALL /bin/bash`
  - **Explanation of Parameters:**
    - `--partition=hotel`: Assigns the debug partition.
    - `--pty`: Engages a pseudo-terminal.
    - `--nodes=1`: Designates one node.
    - `--ntasks-per-node=1`: Sets the task count per node to 1.
    - `-t 00:30:00`: Limits the time to 30 minutes.
    - `--mem 2G`: Allocates 2G of memory
    - `-A htl179`: Defines the account.
    - `--wait=0`: Eliminates waiting time.
    - `--export=ALL`: Exports all environmental variables.
    - `--qos=hotel`: Sets the Quality of Service.
    - `/bin/bash`: Initiates a Bash shell post-allocation.

### Step 7: Launching Jupyter on an Interactive Node to run Python code
1. In your base environment, `pip install notebook`: this installs Jupyter.
   
On your TSCC account in your base directory:

`module load shared`

`module load galyleo`

TSCC utilizes a bash script “Galyleo” to serve Jupyter notebooks more securely.

`galyleo launch --account htl179 --qos hotel --cpus 1 --memory 2G --time-limit 00:30:00 --partition hotel --conda-env base --env-modules slurm/tscc/23.02.7 --conda-init /tscc/nfs/home/yourUsername/anaconda3/etc/profile.d/conda.sh`

We are opening a Jupyter notebook in the base environment.

Ctrl + click on the link shown in the terminal to open jupyter.

Create a new notebook called hello_notebook from inside jupyter. 

Specify that you are using a Python3 kernel.

Type `print('Hello, World')` in the first cell and run the cell.

Save your work.
     
### Creating a conda environment to run R code.

1. **Create a New Conda Environment for R:**
   - If you want a separate environment for R, create a new one using: `conda create -n r_env -y`
   - Activate the new environment: `conda activate r_env`

2. **Install R Base:**
   - To install the R base package, run: `conda install -c r r-base -y`
   - This command installs the R language in your Conda environment.

3. **Verify R Installation:**
   - After installation, you can verify the installation by running: `R --version`
   - This command should show the R version installed, confirming that the installation is successful.

4. **Installing R Packages:**
   - To install an R package, you can start R in the terminal by just typing: `R`
   - Once in the R console, install packages using: `install.packages('package_name')`

5. **Exiting R:**
   - To exit the R console, type: `quit()`
   - Save workspace image? [y/n/c]: Choose 'n' if you do not want to save or 'y' to save.

6. **Deactivate Conda Environment:**
   - Once you are done, you can deactivate your Conda environment by typing: `conda deactivate`

### Step 8: Running R code in a Jupyter notebook

#### Install the R Kernel:
- Inside your Conda environment with r-base installed, install the IRkernel package by running `conda install -c r r-irkernel -y`.
- This will allow Jupyter to run R code in addition to Python.

#### Install Jupyter:
- `pip install jupyter`.

#### Hook R up to Jupyter.
- Run R with `R`.
- Link R up to Jupyter with `IRkernel::installspec()`
- Quit R with `q()`.

#### Launch Jupyter in your r environment.
- `galyleo launch --account htl179 --qos hotel --cpus 1 --memory 2G --time-limit 00:30:00 --partition hotel --conda-env r_env --env-modules slurm/tscc/23.02.7 --conda-init /tscc/nfs/home/yourUsername/anaconda3/etc/profile.d/conda.sh`

#### Create a New R Notebook:
- Create a new notebook and name it `hello_world_r.ipynb`.
- Set the kernel to an R kernel.

#### Running R Code:
- In the first cell of the new R notebook, try entering
  Line 1:
  `x <- 'Hello, World' `
  Line 2:
  `print(x)`
- Run the cell to execute the R code and observe the output.

#### Install R Packages:
- If you need additional R packages, you can install them directly in the notebook.
- Use the command `install.packages('package_name')` in a cell to install a package.
- Load the package using `library(package_name)`.

#### Save Your Work:
- Regularly save your notebook to prevent losing your work.
- Jupyter notebooks autosave, but it's a good habit to manually save as well.

