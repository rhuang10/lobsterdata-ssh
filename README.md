## IMPORTANT: PLEASE DO NOT RUN MULTIPLE BATCH TASKS IN ONE ACCOUNT. THE PROGRAM MIGHT CRASH!. This is for protecting the resources for other LOBSTER users. If you really need to accelerate the construction for a good reason, please contact me. 

# Construct LOB on lobsterdata server
This manual instroduce the steps of constucting LOBSTER limit order book data, using the `lobBatch` tool on LOBSTER server.

## Pre-requisites
* Your institute has a legal subscription to LOBSTER data.
* Your have a user account in LOBSTER user server. This must be required separately by emailing LOBSTER support team.
* A ssh client on you local system.
    - For linux system, a ssh client is typically installed.
    - For Windows system, I would suggest,
        - [MobaXterm](https://mobaxterm.mobatek.net/), 
        - [PuTTY](https://www.putty.org/), or
        - Simply [git-bash](https://git-scm.com/downloads) 

## Steps
1. Log into your account in LOBSTER server, using the port `2222`
   ```bash
   ssh your-name@lobsterdata.com -p 2222
   ``` 
   For the ssh-client with GUI, please consult its documentation.
2. Create the construction batch file. There is two example files under `/home/lobster/`.
   ```bash
   cp /home/lobster/example.batch .
   ```
   or  
   ```bash
   cp /home/lobster/example_snapshot.batch .
   ```
   If you'd like to contract order book interval snapshots.
   
   Modify the file by `nano` or `vim`
   ```bash
   nano example_snapshot.batch
   ``` 
   The fields are `symbol`, `start date`, `end date`, `start time`, `end time`, `book level`, `output format` (please always use 1), `with MPID`, `snapshot_interval_in_seconds`
4. As the constructing could take long time, I would suggest you use `screen`, so that you can disconnect from the server without stopping the job.
   ```bash
   screen -S constructing
   ```
5. Start constructing in `screen` terminal
   ```bash
   mkdir output
   lobBatch -c /home/lobster/.lobsterrc -b example_snapshot.batch -o /home/your-name/output
   ```
6. Once you see the constructing job starts, you can close your connection window.
7. You could check the progress by logging back, either through `screen`
   ```bash
   screen -ls # see the active screen terminal
   screen -Rd constructing # reconnect to the terminal
   ```
   or the file `lobBatch.log` under you home directory
   ```bash
   nano lobBatch.log
   ```
8. Once the constructing job is completed, you can download the data through `sftp` client.
   ```bash
   sftp -P 2222 your-name@lobsterdata.com
   ```
   The output files are in `output/0/0` under your home directory.
