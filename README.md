# MongoDB Hive Connector  (Unofficial)

Unofficial Hive MongoDB Connector for Hive 3 and CDP (Cloudera Data Platform) 7.1+

## Purpose

This MongoDB-Hive Connector is a library unofficially modification from https://github.com/mongodb/mongo-hadoop which allows MongoDB (or backup files in its data format, BSON) to be used as an input source, or output destination, for Hive Queries. It is designed to allow greater flexibility and performance and make it easy to integrate data in MongoDB with the Hive 3 and the Hive 3 in CDP (Cloudera Data Platform) 7.1+

This connector will be built by Maven and currently misses unit tests.

## Again, big thanks to the original contributors and hope this one can help someone if they are in Hive 3


## Requirements

#### Version Compatibility

These are the minimum versions tested with the Hadoop connector. Earlier
versions may work, but haven't been tested.

- *Hive*: 3.1+
- *MongoDB*: 4.3+

#### Dependencies

In the pom.xml file, it points to the Cloudera Repository since this is tested in CDP 7.1+ 

## Building

Run `mvn package` to build the jars. 

## Usage

There will be two jars:
connector_core-1.0-SNAPSHOT.jar
connector_hive-1.0-SNAPSHOT.jar

And one more thing, download the `mongo-java-driver-3.12.11.jar` also

Please add these 3 jars into the hive by `add jar` or configure it in the aux path of CDP.

## DDL Example

Create some data in MongoDB
```
use test;
db.test.insert({"name":"Tom", age:18})
db.test.insert({"name":"Jim", age:38})
db.test.insert({"name":"Chris", age:7})
db.test.insert({"name":"Kate", age:21})
show collections;
db.test.find()
```

```sql
CREATE external TABLE test(
    id string,
    name string,
    age integer
)
ROW FORMAT SERDE 
  'com.mongodb.hadoop.hive.BSONSerDe' 
STORED BY 'com.mongodb.hadoop.hive.MongoStorageHandler'
WITH SERDEPROPERTIES('mongo.columns.mapping'='{"id":"_id","name":"name","age":"age"}')
LOCATION
  'hdfs://{external_table_path}'
TBLPROPERTIES('mongo.input.split.create_input_splits'='false', 
'mongo.uri'='mongodb://{mongodb_user_name}:{mongodb_password}@{mongodb_url}:{mongodb_port}/test.test?authSource=admin');
```

Then we can query the data in MongoDB.

## Future Work
Now it missed the unit tests.