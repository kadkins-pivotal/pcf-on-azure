# Azure poc-test Footprint

*Compiled by Kendall Adkins on December 7th, 2018*

## Default Marketplace Tiles

Pivotal Bosh Director for Azure  
Small Footprint PAS
Microsoft Azure Service Broker

## Tile Additions

Pivotal Application Service for Windows  
Spring Cloud Services  
RabbitMQ  
Single Sign-On  
MySQL for Pivotal Cloud Foundry v2  
CredHub Service Broker  

## Azure Spend Alerts

|Name|Usage|Date Spend|Daily Threshold|% of Threshold|
| --- | --- | --- | --- | --- |
|PA-kadkins	|12/06/2018	|$91.79	   |$50.00	|183.58%|
|PA-kadkins	|12/05/2018	|$93.41	|$50.00	|186.82%|
|PA-kadkins	|12/04/2018	|$93.02	|$50.00	|186.04%|
|PA-kadkins	|11/29/2018	|$60.91	|$50.00	|121.81%|
|PA-kadkins	|11/28/2018	|$100.64	|$50.00	|201.27%|
|PA-kadkins	|11/27/2018	|$85.68	|$50.00	|171.35%|
|PA-kadkins	|11/16/2018	|$97.18	|$50.00	|194.36%|
|PA-kadkins	|11/15/2018	|$103.60	|$50.00	|207.20%|
|PA-kadkins	|11/14/2018	|$99.67	|$50.00	|199.33%|
|PA-kadkins	|11/09/2018	|$118.03	|$50.00	|236.05%|
|PA-kadkins	|11/08/2018	|$116.38	|$50.00	|232.76%|
|PA-kadkins	|11/07/2018	|$141.92	|$50.00	|283.84%|
|PA-kadkins	|11/06/2018	|$110.62	|$50.00	|221.24%|
|PA-kadkins	|11/05/2018	|$128.69	|$50.00	|257.39%|
|PA-kadkins	|11/04/2018	|$90.77	|$50.00	|181.55%|

Average Daily Run Rate = $102.15

## BOSH Scripts

Costs can be controlled by stopping the platform when it is not in use and restarting the platform when it is needed. The following scripts are helpful. Please note that the scripts take many hours to run to you must plan ahead.

### Hard Stop Deployments

There are several lines commented out in this script. They will delete the deployment and cleanup all orphaned data. Uncommenting these and running the script will in effect delete the entire deployment with the exception of the bosh director and the operations manager. This will signficantly reduce the cost when the platform is not in use. However, it increases the time required to spin the platform back up. Recreating the deployments is also not an option if this approach is taken. To spin the platform back up, you must enter opsman and kick off an deployment manually via the web interface. Your configuration settings from the previous deployments will be maintained by opsman.

```bash
#!/bin/sh

startDate=`date`

printf "=== START TIME: $startDate ===\n"

for d in `bosh -e pcf deployments --column=name`
do
   printf "\n--- Stopping deployment $d ---\n"
   bosh -n -e pcf -d $d stop --hard

   #printf "\n--- Deleting deployment $d ---\n"
   #bosh -n -e pcf -d $d delete-deployment --force

done

#printf "\n--- Cleaning bosh ---\n"
#bosh -n -e pcf clean-up --all

printf "\n=== START TIME: $startDate ===\n"
printf "===   END TIME: `date` ===\n"
```

### Recreate Deployments

This script will only work if the delete deployment and clean-up are not run in the above script.

```bash
#!/bin/sh

startDate=`date`

printf "=== START TIME: $startDate ===\n"

for d in `bosh -e pcf deployments --column=name`
do
   printf "\n--- Recreating deployment $d ---\n"
   bosh -n -e pcf -d $d recreate
done

printf "\n=== START TIME: $startDate ===\n"
printf "===   END TIME: `date` ===\n"
```

### Running NOHUP

You can run scripts in the background as follows:

```bash
nohup sh pcf-shutdown.sh > pcf-shutdown.log 2>&1 &
```

All output is sent to the log file.

