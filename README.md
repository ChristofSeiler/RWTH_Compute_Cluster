# RWTH Compute Cluster

## Get Access

1. Request a DKE cluster account [here](https://fse.maastrichtuniversity.nl/lo-fse/site/requests/request-dke-cluster-access/)
2. You will recieve one email from Aachen and one from Maastricht
3. Following instruction in Aachen email to activate and create the cluster account
4. Reply to Maastricht email with newly created User ID and wait for reply
5. Go to your Aachen account [here](https://www.rwth-aachen.de/selfservice), select the `Accounts and Passwords` page, and set a new password for `	Hochleistungsrechnen RWTH Aachen` and `	WLAN/VPN` services

## Login

1. Establish VPN connection to Aachen following instructions [here](https://doc.itc.rwth-aachen.de/display/VPN/VPN+%28ab+MacOS+10.7%29+AnyConnect)
2. Login with your new RWTH Aachen account: 

```bash
ssh <Your User ID>@login18-1.hpc.itc.rwth-aachen.de
```

## Usage

* Check manual for commands [here](https://doc.itc.rwth-aachen.de/display/CC/Using+the+Batch+System)
* You need to specify the UM project in your job scripts `submit.sh`:

```{bash}
sbatch submit.sh
```

## Storage

* Backed up: `$HOME`
* Not backed up: `$WORK` and `$HPCWORK`
