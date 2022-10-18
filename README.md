# Chainlink node requester whitelisting

## Why?
Node operators might want to promote their nodes and jobs to make it easier for developers to find a suitable oracle on any blockchain that transmits data according to their projects' needs. Nodes can therefore become targets of DoS attacks or requests requiring huge amounts of gas units resulting in transaction costs exceeding the set oracle payment. Therefore, node operators are encourged to manually restrict the permissions for direct request job runs on mainnets by adding authorized requesters to the node’s database.

## How?

### 1 - Install PostgreSQL on your host

First of all you need to install the PostgreSQL client on your host in order to connect to your PostgreSQL server and database.

Install PostreSQL on Ubuntu:
```bash
sudo apt-get install postgresql-client
```

PostgreSQL for other Operating Systems: https://www.postgresql.org/download/

### 2 - Connect to your PostgreSQL database
You have to connect to the database with the same credentials the Chainlink node uses. You are able to connect and edit the database while the node is running. 
```bash
psql "host=$YOURIP sslmode=disable dbname=$DBNAME user=$USERNAME password=$PASSWORD"
```
If your PostgreSQL server runs in a private subnet, you can connect to it using the command above after installing the PostgreSQL client on a bastion host in order to communicate with private IPs.
### 3 - Navigate to the "direct_request_specs" table

In order to add or remove authorized requesters you need to access the "direct_request_specs" table:

```bash
SELECT * FROM direct_request_specs;
```

### 4 - Whitelist requesters

Your node's jobs are displayed in separate rows and you can whitelist contract addresses by adding them to the jobs' "requesters" cells.

Whitelisting for a single job
```bash
UPDATE direct_request_specs SET requesters='$ADDRESS' WHERE job_spec_Id='$JOBSPECID';
```

Whitelisting for every RunLog job:
```bash
UPDATE direct_request_specs SET requesters='$ADDRESS1,$ADDRESS2,$ADDRESS3' WHERE type ='runlog';
```

Example command for whitelisting two diffrent addresses for one specific job:
```bash
UPDATE direct_request_specs SET requesters='0x840374766feC302C453E37b35D21029d3ea00333,0xFfD5E84D50EfEfb19fe927347aB2e366D63a8543' WHERE job_spec_Id='7c4b9450-28f7-4b2e-abd7-d428f03ba45c';
```
