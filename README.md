# RWTH Compute Cluster

It seems that the Aachen cluster is not accessible to researchers and students form Maastricht University anymore. Try out the new Maastricht cluster:

* [DSRI](https://dsri.maastrichtuniversity.nl/docs/access-dsri/)

## Get Access

1. Request a DKE cluster account [here](https://fse.maastrichtuniversity.nl/lo-fse/site/requests/request-dke-cluster-access/)
2. You will recieve one email from Aachen and one from Maastricht
3. Following instruction in Aachen email to activate and create the cluster account
4. Reply to Maastricht email with newly created User ID and wait for reply
5. Go to your Aachen account [here](https://www.rwth-aachen.de/selfservice), select the `Accounts and Passwords` page, and set a new password for `	Hochleistungsrechnen RWTH Aachen` and `	WLAN/VPN` services

## Login

1. Establish VPN connection to Aachen following instructions [here](https://help.itc.rwth-aachen.de/en/service/vbf6fx0gom76)
2. Login with your new RWTH Aachen account: 

```bash
ssh <Your User ID>@login18-1.hpc.itc.rwth-aachen.de
```

## Usage

* Check manual for commands [here](https://doc.itc.rwth-aachen.de/display/CC/Using+the+Batch+System)
* Here an example `submit.sh` job script:

```{bash}
#!/bin/bash

#SBATCH --job-name=cytoeffect_short
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --mem-per-cpu=4GB
#SBATCH --time=01:00:00
#SBATCH --account=um_dke

R -e "rmarkdown::render('Reanalysis_Aghaeepour2017_Poisson.Rmd')"
```

* Submit the script to the scheduler:

```{bash}
sbatch submit.sh
```

* Check status (change `YOUR_USERNAME` to your username):

```{bash}
squeue -u YOUR_USERNAME
```

## Example

First install RStudio. This will allow you to knit Rmd files.

```{bash}
cd $HOME
RSTUDIO_VERSION=rstudio-server-rhel-1.2.1335-x86_64
mkdir $RSTUDIO_VERSION
cd $RSTUDIO_VERSION
wget https://download2.rstudio.org/server/centos6/x86_64/${RSTUDIO_VERSION}.rpm
rpm2cpio ${RSTUDIO_VERSION}.rpm | cpio -idmv
cd ..
echo "export RSTUDIO_VERSION=$RSTUDIO_VERSION" >> .zprofile
echo 'export PATH=$HOME/${RSTUDIO_VERSION}/usr/lib/rstudio-server/bin/pandoc:$PATH' >> .zprofile
source .zprofile
```

Now clone a repository and submit a job script. This example should take about 30 min. It will request one node with 8 cores for 1 hour.

```{bash}
git clone https://github.com/ChristofSeiler/blish_cytoeffect_tutorial.git
cd blish_cytoeffect_tutorial
sbatch submit.sh
```

## Storage

* Backed up: `$HOME`
* Not backed up: `$WORK` and `$HPCWORK`
