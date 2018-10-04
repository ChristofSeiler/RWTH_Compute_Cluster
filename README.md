# Maastricht Computing

## Get Access

1. Request a DKE cluster account [here](https://fse.maastrichtuniversity.nl/lo-fse/site/requests/request-dke-cluster-access/)
2. You will recieve one email from Aachen and one from Maastricht
3. Following instruction in Aachen email to activate and create the cluster account
4. Reply to Maastricht email with newly created User ID and wait for reply
5. Go to your Aachen account [here](https://www.rwth-aachen.de/selfservice) and set passwords for all accounts on the `Accounts and Passwords` page

## Run Jobs

1. Establish VPN connection to Aachen following instructions [here](https://doc.itc.rwth-aachen.de/pages/viewpage.action?pageId=3475772)
2. Login with your new RWTH Aachen account: 

```bash
ssh -l <Your User ID> cluster.rz.rwth-aachen.de
```

or 

```bash
ssh -l <Your User ID> login.hpc.itc.rwth-aachen.de
```

3. You need to specify the UM project in your job scripts:

```{bash}
BSUB -P um_dke
```
