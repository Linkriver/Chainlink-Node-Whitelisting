# Chainlink node requester whitelisting

## Why?
Node operators can register their nodes and jobs on https://market.link to make it easier for developers to find the right oracle on any chain that transmits data according to their projects' needs. Nodes can therefore become targets of DoS attacks or requests with functions requiring huge amount of gas units resulting in transactional costs exceeding the set oracle payment. Therefore, node operators can manually limit the permission for direct request job runs by adding entitled requesters to the node’s database.

## How?

### 1 - Install PostgreSQL on your host

First of all you need to install the PostgreSQL client on your host in order to connect to your PostgreSQL server and database.

Install PostreSQL on Ubuntu:
```bash
sudo apt-get install postgresql-client
```

PostgreSQL for other Operating Systems: https://www.postgresql.org/download/

### 2 - Connect to your PostgreSQL databse
Your can connect to the database with the same USER that the chainlink node uses. You are able to connect and edit the database meanwhile the node is running, because the User just lock the database for "writing"

```bash
psql "host=$YOURIP sslmode=disable dbname=$DBNAME user=$USERNAME password=$PASSWORD"
```
If your PostgreSQL server runs in a private subnet, you are able to connect with this command if you install the postgres client on an Bastion-Host, in order to communicate with private IP's
### 3 - Navigate to the "initiators" table

In order to add or remove requesters you need to access the "initators" table:

```bash
SELECT * FROM initiators;
```

### 4 - Whitelist requesters

All of your node's jobs are displayed in separate rows and you can whitelist contract addresses by adding them to the jobs' "requesters" cells.

Edit whitelist for a single job
```bash
UPDATE initiators SET requesters='$ADDRESS' WHERE job_spec_Id='$JOBSPECID';
```

Edit whitelist for every RunLog job:
```bash
UPDATE initiators SET requesters='$ADDRESS1,$ADDRESS2,$ADDRESS3' WHERE type ='runlog';
```

Here is an example how to list 2 diffrent addresses for onw specific JobSpec:
```bash
UPDATE initiators SET requesters='0x840374766feC302C453E37b35D21029d3ea00333,0xFfD5E84D50EfEfb19fe927347aB2e366D63a8543' WHERE job_spec_Id='7c4b9450-28f7-4b2e-abd7-d428f03ba45c';
```
