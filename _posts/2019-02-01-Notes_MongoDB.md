---
class: note
title: Notes MongoDB
updated: 2020-02-01 00:00

---


### MongoBD with Docker

For create a enviroment MongoDB with Docker 

```
$ docker run --name <NAME>_no_auth -v <PATH_DATA>/data/db -p <PORT>:27017 -d mongo

$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS                      NAMES
<container_id>      mongo               "docker-entrypoint.s…"   29 seconds ago      Up 28 seconds               0.0.0.0:<PORT>->27017/tcp  <NAME>

```

Add security to mongo. 

```

$ docker exec -it <container_id> bash
$ mongo
$ use <DB_NAME>
switched to db <DB_NAME>
$ db.createUser({user: "<USER_NAME>",pwd: "<PASSWORD>",roles: [ { role: 'readWrite', db:'<DB_NAME>'} ]})
Successfully added user: {
        "user" : "<USER_NAME>",
        "roles" : [
                {
                        "role" : "readWrite",
                        "db" : "<DB_NAME>"
                }
        ]
}

$ docker stop <container_id>
<container_id>
$ docker run --name <NAME> -v <PATH_DATA>:/data/db -p <PORT>:27017 -d mongo --auth

```


Maybe We need limit o upgrade the resource for the docker conteiner.

```
$ docker update <container_id> --memory=<BYTES> --memory-swap=-1 --cpuset-cpus=<CPU_NUMBER>
```


### How export object from MongoDB

Export to files objets to files

```
$ mongo | tee file.txt
MongoDB shell version: 2.4.2
connecting to: test
> printjson({this: 'is a test'}){ "this" : "is a test" }> printjson({this: 'is another test'}){ "this" : "is another test" }> exit
bye

# delete all except JSON
tail -n +3 file.txt | egrep -v "^>|^bye" > output.json

```


### How import objects

Import from file with a Json Array. 

```
$ mongoimport <CONNECTION_PARAMETERS> --type json --file <FILE_JSON> --jsonArray
```

Import from file Shapefile

```
$ ogr2ogr -f GeoJSON -t_srs EPSG:4326 <file_name>.geojson <file_name>.shp
$ mongoimport <CONNECTION_PARAMETERS> -c <collection> --file "<file_name>.geojson" --jsonArray
```


### Backup and Restore

If We are working with dockers container, just move the <PATH_DATA>. 


Cmd bakcup.
```
# One collection
$ mongodump <CONNECTION_PARAMETERS> --out <PATH_BACKUP> --collection <COLLECTION_NAME> --db <DB_NAME> 
# All the dbs
$ mongodump <CONNECTION_PARAMETERS> --out <PATH_BACKUP>
 
```

Cmd restore
```
# One collection
$ mongorestore -<CONNECTION_PARAMETERS> <PATH_BACKUP> --db <DB_NAME> 
 
```

### Create Index

GEO 
```
db.collection.createIndex( { <location field> : "2dsphere" } )
```
### Queries 

Simple GIS query by circle.
```
db.getCollection('<NAME_COLLECTION>').find({
    "geometry":
     { $near:           {
            $geometry: { type: "Point",  coordinates: [ -3.9667, 40.78 ] },
            $minDistance: 1000,
            $maxDistance: 5000
          }
       }
})
```


Simple GIS query by polygon.

```
db.getCollection('<NAME_COLLECTION>').find({
    "geometry": {
       $geoWithin: {
          $geometry: {
             type : "Polygon" ,
             coordinates: [
               [
                 [ -100, 60 ], [ -100, 0 ], [ -100, -60 ], [ 100, -60 ], [ 100, 60 ], [ -100, 60 ]
               ]
             ],
             crs: {
                type: "name",
                properties: { name: "urn:x-mongodb:crs:strictwinding:EPSG:4326" }
             }
          }
       }
     }
})    
```
