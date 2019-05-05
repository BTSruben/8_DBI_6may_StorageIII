# 8_DBI_6may_StorageIII

MongoDB in AWS Setup


1. Create an instance EC2 in AWS with Amazon Linux Image

2. Connect to public EC2 instance using the command: ssh -i <keypair> <hostname>

3. Add MongoDB repo inside the instance:

    sudo vim /etc/yum.repos.d/mongodb-org-4.0.repo


    [mongodb-org-4.0]
    name=MongoDB Repository
    baseurl=https://repo.mongodb.org/yum/amazon/2013.03/mongodb-org/4.0/x86_64/
    gpgcheck=1
    enabled=1
    gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc

4. Install MongoDB ( EC2 Instance )

    sudo yum install -y mongodb-org

5. Start MongoDB Service ( EC2 Instance )

    sudo service mongod start

6. You can verify that the mongod process has started successfully by checking the contents of the log file at /var/log/mongodb/mongod.log for a line reading ( EC2 Instance )

    [initandlisten] waiting for connections on port <port>


    grep "waiting for connection" /var/log/mongodb/mongod.log

7. From our local machine send the file restaurants.json to EC2 instance ( Local machine )

    scp -i <keypair> <file> <hostname>:<path>

8. Import our documents to MongoDB ( EC2 Instance )

    mongoimport --db test --collection restaurants --file restaurants.json




**Remote access**

1. Change MongoDB config ( EC2 Instance )

    sudo vim /etc/mongod.conf

    bindIp: 0.0.0.0

2. Restart MongoDB Service ( EC2 Instance )

    sudo service mongod restart

3. Add in Security Group of AWS EC2 ( AWS PANEL )

    Outbound open tcp 27017 

4. Connect from local machine to remote MongoDB:

    mongo <public-url>:<port>/<database>


**Exercices**

1. Write a MongoDB query to display all the documents in the collection restaurants.

db.restaurants.find();

2. Write a MongoDB query to display the fields restaurant_id, name, borough and cuisine for all the documents in the collection restaurant.

db.restaurants.find({},{"restaurant_id" : 1,"name":1,"borough":1,"cuisine" :1});

3. Write a MongoDB query to display the fields restaurant_id, name, borough and cuisine, but exclude the field _id for all the documents in the collection restaurant.

db.restaurants.find({},{"restaurant_id" : 1,"name":1,"borough":1,"cuisine" :1,"_id":0});

4. Write a MongoDB query to display all the restaurant which is in the borough Bronx.

db.restaurants.find({"borough": "Bronx"});

5. Write a MongoDB query to display the first 5 restaurant which is in the borough Bronx.

db.restaurants.find({"borough": "Bronx"}).limit(5);

6. Write a MongoDB query to display the next 5 restaurants after skipping first 5 which are in the borough Bronx.

db.restaurants.find({"borough": "Bronx"}).skip(5).limit(5);

7. Write a MongoDB query to find the restaurants who achieved a score more than 90.

db.restaurants.find({grades : { $elemMatch:{"score":{$gt : 90}}}});

8. Write a MongoDB query to find the restaurants that achieved a score is more than 80 but less than 100.

db.restaurants.find({grades : { $elemMatch:{"score":{$gt : 80 , $lt :100}}}});


