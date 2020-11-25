# How to use the RWTH Aachen cluster

## Getting an Aachen account

For this I reccomend following the **Get access** specified by Christof Seiler [here](https://github.com/ChristofSeiler/RWTH_Compute_Cluster).  

## Connecting to server

### Create VPN connection with RWTH Aachen

For this, install Cisco AnyConnect. 

Connect to **vpn.rwth-aachen.de** using a Split Tunnel connection. With your user and pw. 

### Ssh the server

For this use the following command on the terminal.

``` shell
ssh user@login18-1.hpc.itc.rwth-aachen.de
```

## Useful commands

### Checking hardware/server load

For checking RAM in Gigabytes:

``` shel
free -g 
```

For checking processor:

``` bash
lscpu
```

To check for **used corehours** (out of the 2000 monthly allowed):

``` shell
r_wlm_usage
```

To check current server load:

``` bash
squeue -w linuxihdc160,linuxihdc161
```

### Submission and handling of jobs

To submit job job.sh:

``` shell
sbatch job.sh
```

To check for the state of your current batches:

``` shell
squeue -u$(whoami)
```

To cancel job 12345:

``` shell
scancel 12345
```

### Running a remote jupyter notebook

You should copy the following template and paste it into a `jobscript.sh` file. 

``` shell
#!/usr/local_rwth/bin/zsh

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=8G
#SBATCH --account="um_dke"
#SBATCH --gres=gpu:1
#SBATCH --time=1-0:00:00
#SBATCH --job-name=jupyter-notebook
#SBATCH --output=jupyter-notebook-%J.log


echo "------------------------------------------------------------"
echo "SLURM JOB ID: $SLURM_JOBID"
echo "Running on nodes: $SLURM_NODELIST"
echo "------------------------------------------------------------"
module switch intel gcc
module load python/3.6.0

#Install jupyter as local user
#Add the local python pip path to the front of the PATH variable
export PATH=$HOME/.local/bin:$PATH

# only run the following two lines the first time you're using a jupyter notebook
# pip3.6 install --user --upgrade pip 
# pip3.6 install --user jupyter

# set a random port for the notebook, in case multiple notebooks are
# on the same compute node.
NOTEBOOKPORT=`shuf -i 8000-8500 -n 1`

# set a random port for tunneling, in case multiple connections are happening
# on the same login node.
TUNNELPORT=`shuf -i 8501-9000 -n 1`

# set a random access token
TOKEN=`cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 49 | head -n 1`

echo "On your local machine, run:"
echo ""
echo "ssh -L8888:localhost:$TUNNELPORT $SLURM_SUBMIT_HOST -N -4"
echo ""
echo "and point your browser to http://localhost:8888/?token=$TOKEN"
echo "Change '8888' to some other value if this port is already in use on your PC,"
echo "for example, you have more than one remote notebook running."
echo "To stop this notebook, run 'scancel $SLURM_JOB_ID'"

# set a random port for the notebook, in case multiple notebooks are
# on the same compute node.
NOTEBOOKPORT=`shuf -i 8000-8500 -n 1`

# set a random port for tunneling, in case multiple connections are happening
# on the same login node.
TUNNELPORT=`shuf -i 8501-9000 -n 1`

# set a random access token
TOKEN=`cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 49 | head -n 1`

echo "On your local machine, run:"
echo ""
echo "ssh -L8888:localhost:$TUNNELPORT $(whoami)@$SLURM_SUBMIT_HOST -N -4"
echo ""
echo "and point your browser to http://localhost:8888/?token=$TOKEN"
echo "Change '8888' to some other value if this port is already in use on your PC,"
echo "for example, you have more than one remote notebook running."
echo "To stop this notebook, run 'scancel $SLURM_JOB_ID'"

# Set up a reverse SSH tunnel from the compute node back to the submitting host (login01 or login02)
# This is the machine we will connect to with SSH forward tunneling from our client.
ssh -R$TUNNELPORT\:localhost:$NOTEBOOKPORT $SLURM_SUBMIT_HOST -N -f

# Start the notebook
srun -n1 jupyter-notebook --no-browser --port=$NOTEBOOKPORT --NotebookApp.token=$TOKEN --log-level WARN
# To stop the notebook, use 'scancel'
```

Important to note are the lines:

``` shell
# pip3.6 install --user --upgrade pip 
# pip3.6 install --user jupyter
```

That should be commented out on the first time running a remote jupyter notebook.

To create the jobscript file do:

``` shell
nano jobscript.sh
```

Paste the shell file above and press <kbd> control </kbd>+<kbd>X</kbd> and then <kbd>Y</kbd> to save the file.

To start the notebook run:

 ``` shell
sbatch jobscript.sh
 ```

Which will generate a log file of the form `jupyter-notebook-12345.log` open this file which will include instructions on how to open the notebook on your laptop.


Copy the line that specifies where **1111** will be a different port and **user** will be your Aachen user: 

On your **local system**:

``` shell
ssh -L8888:localhost:1111 user@login18-1.hpc.itc.rwth-aachen.de -N -4
```

Then in your preferred browser simply go to [localhost:8888/](localhost:8888/)  

On your **remote system**:

To close the notebook run the following command by first substituting 12345 for the job number that was given to you after running `sbatch`:

``` shell
scancel 12345
```

### Directory sync

Inspired by [this](https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps). 

For this we will use `rsync`:

``` shell
rsync -aP ~directory/ user@login18-1.hpc.itc.rwth-aachen.de:directory
```


### Mounting remote drives to local system
This functionality will allow mounting your drives on the Aachen cluster to your local system. Moving files and accessing it through GUI becomes much easier with this.
Inspired by [this](https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh) 

On your **local system**:
You will need sshfs, For linux,
```shell
sudo apt-get install sshfs
```
For other operating systems, check Installing sshfs here: https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh 

```
sudo mkdir /mnt/dke #The /mnt/dke can be any path where you want to mount your remote drive
sudo sshfs -o allow_other,default_permissions user@login18-1.hpc.itc.rwth-aachen.de:directory /mnt/dke
```
**user** above is your username and **directory** is the remote drive path you want to mount for example ```/home/user```
