## Note for when working cluster setup 
* What we have in the cluster setup 
  * couchdb-one -> 192.168.10.11:5984
  * couchdb-two -> 192.168.10.12:5984
  * couchdb-three -> 192.168.10.13:5984


```bash 
# check membership from the couchdb-one 
http GET  :5984/_membership -a admin:password

echo "Enabling cluster on first node..."
curl -X POST "http://admin:password@192.168.10.11:5984/_cluster_setup" \
    -H "Content-Type: application/json" \
    -d '{"action": "enable_cluster", "bind_address":"0.0.0.0", "username": "admin", "password":"password", "port": 5984, "node_count": "3", "remote_node": "192.168.10.12", "remote_current_user": "admin", "remote_current_password": "password"}'

# Add the second node to the cluster
echo "Adding second node to the cluster..."
curl -X POST "http://admin:password@192.168.10.11:5984/_cluster_setup" \
    -H "Content-Type: application/json" \
    -d '{"action": "add_node", "host":"192.168.10.12", "port": 5984, "username": "admin", "password":"password"}'

# Add the third node to the cluster
echo "Adding third node to the cluster..."
curl -X POST "http://admin:password@192.168.10.11:5984/_cluster_setup" \
    -H "Content-Type: application/json" \
    -d '{"action": "add_node", "host":"192.168.10.13", "port": 5984, "username": "admin", "password":"password"}'

# Finish the cluster setup
echo "Finishing cluster setup..."
curl -X POST "http://admin:password@192.168.10.11:5984/_cluster_setup" \
    -H "Content-Type: application/json" \
    -d '{"action": "finish_cluster"}'

# Verify the cluster setup
echo "Verifying cluster setup..."
curl "http://admin:password@192.168.10.11:5984/_membership"
```