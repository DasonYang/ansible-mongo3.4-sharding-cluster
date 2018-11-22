# Mongo Sharding Cluster

Deploy mongo replica set sharding cluster

## Environment
### Deploy Host
  - ubuntu 16.04 amd64
  - ansible 2.6+
### Target Host
  - ubuntu 16.04 amd64
  - mongodb 3.4
  - pymongo latest

## Installation
- ### host variables
    
    - mongos_chunk_size: 1
    - mongos_port: 8888
    - mongoc_port: 7777
    - mongodb_datadir_prefix: /data/
    - iface: 'eth1'
    - mongo_admin_pass: 123456
    - mongo_admin_user: admin
    - db_user_pass: <db user password>
    - db_user_name: <db user name>
    - db_name: <db name>

-  ### shards.yml example

	shard_collections:
          - name: account
            key: email
            value: 1

-  ### host.yml example

        all:
            hosts:
              mongo1:
                ansible_ssh_host: 192.168.33.20
              mongo2:
                ansible_ssh_host: 192.168.33.21
              mongo3:
                ansible_ssh_host: 192.168.33.22

            children:
              common-servers:
                hosts: 
                  mongo1:
                  mongo2:
                  mongo3:

              mongod-servers:
                hosts:
                  mongo1:
                    mongod_port: 2700
                  mongo2:
                    mongod_port: 2701
                  mongo3:
                    mongod_port: 2702

              replica-servers:
                hosts:
                  mongo1:
                  mongo2:
                  mongo3:

              mongoc-servers:
                hosts:
                  mongo1:
                  mongo2:
                  mongo3:

              mongos-servers:
                hosts:
                  mongo1:
                  mongo2:
                  mongo3:

- ### commands
    
  ```sh
  ansible-playbook -i host.yml deploy-sharding-cluster.yml -e@vars.yml
  ```

## Test steps

    ### Login to sharding cluster
    ```sh
    mongo localhost:8888/admin -u admin -p 123456
    ```
    ### Show status
    ```sh
    sh.status()
    ```
    ### Login with common user
    ```sh
    mongo localhost:8888/db_name -u db_user -p db_pass
    ```
    ### Insert data
    ```sh
    db.collection_name.insert({"name":"uusseerr"})
    ```
    ### Retrieve data
    ```sh
    db.collection_name.find({})
    ```

## Rewrite From
* [ansible-example] - Ansible github example
* [ansible-mongodb-cluster] - support mongo 3.2 from ansible example

    [ansible-example]: <https://github.com/ansible/ansible-examples>
    [ansible-mongodb-cluster]: <https://github.com/twoyao/ansible-mongodb-cluster>
