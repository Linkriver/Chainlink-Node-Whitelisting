# Chainlink node whitelisting

## Why?
Node operators can register their nodes and jobs on https://market.link to make it easier for developers to find the right oracle on any chain that transmits data according to their needs. Depending on the network and its gas fees, publicly listed jobs could be abused if the callback function used a huge amount of gas units resulting in transactional costs exceeding the set oracle payment or just by. Therefore, node operators can manually limit the permission by adding entitled requesters’ smart contract addresses to the node’s database.

## How?

### 1 - install PostgreSQL on your host

First of all you need to install postgres on your host in order to connect to your PostgreSQL server and database

here is the guide to install Postres on Linux systems (Linux,Ubuntu):
```bash
sudo apt-get install postgresql-client
```

For other OS have a look here: https://www.compose.com/articles/postgresql-tips-installing-the-postgresql-client/

### 2 - connect to your PostgreSQL databse
Your are need to connect to the database with the same USER that the chainlink node uses. You are able to connect and edit the database meanwhile the node is running, because the User just lock the database for "writing"

```bash
psql "host=$YOURIP sslmode=disable dbname=$DBNAME user=$USERNAME password=$PASSWORD"
```
If you have your PostgreSQL server inside of a private subnet, you are able to connect with this command if you install the postgres client on an Bastion-Host, in order to communicate with private IP's
### 3 - direct to the Initiators table

In order to change or set requester you need to open the initators table

```bash
SELECT * FROM initiators;
```

### 4 - Whitelist requesters

You see now the full list of all your job specs and are able to whitlist addresses there that appear than in the requesters row

Whitlist every runlog job:
```bash
UPDATE initiators SET requesters='$ADDRESS1,$ADDRESS2,$ADDRESS3' WHERE type ='runlog';
```

Whitelist seperate job Specs-ID's
```bash
UPDATE initiators SET requesters='$ADDRESS' WHERE job_spec_Id='$JOBSPECID';
```

Here is an example how to list 2 diffrent addresses for onw specific JobSpec:
```bash
UPDATE initiators SET requesters='0x840374766feC302C453E37b35D21029d3ea00333,0xFfD5E84D50EfEfb19fe927347aB2e366D63a8543' WHERE job_spec_Id='7c4b9450-28f7-4b2e-abd7-d428f03ba45c';
```
