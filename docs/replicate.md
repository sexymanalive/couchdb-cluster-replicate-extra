## Note 

using the command line in order to replicate the database 
```bash

# works fine 
curl -X POST "http://admin:password@localhost:5984/_replicate" \
  -H "Content-Type: application/json" \
  -d '{
    "source": "http://admin:password@172.16.238.11:5984/mydb",
    "target": "http://admin:password@172.16.238.12:5984/new-copy-db",
    "continuous": true, 
    "create_target": true
  }' 

# this will be shown in the replication side bar 
curl -X PUT "http://admin:password@localhost:5984/_replicator/remote_replicate_ne" \
  -H "Content-Type: application/json" \
  -d '{
    "_id": "remote_replicate_ne",
    "source": "http://admin:password@localhost:5984/mydb",
    "target": "http://admin:password@localhost:5985/anotherdb",
    "continuous": true, 
    "create_target": true
  }'

```
172.26.0.2 -> container two 

