---
class: note
title: Notes Pgrouting 
updated: 2020-06-08 00:00
---

### Settings. 



Install

Install pgrouting with docker

```
docker pull pgrouting/pgrouting
docker run --name <NAME_CONTAINER> -p <PORT>:5432 -e POSTGRES_PASSWORD=admin pgrouting/pgrouting
```

Install tools in my computer
```
sudo apt install postgis 
```


Downloads shapefile

```
wget http://download.geofabrik.de/europe/spain-latest-free.shp.zip
unzip spain-latest-free.shp.zip
```

How to convert to postgres table. 
```
shp2pgsql gis_osm_roads_free_1.shp > spain_roads.sql
```

Install the gis extension in the database;
CREATE EXTENSION postgis;
CREATE EXTENSION pgrouting;

psql -d <DATABASE> -h localhost -U postgres -p5432 -f spain_roads.sql


shp2pgsql -s 4326  -I C:\ruta\ocean.shp public.ocean > ocean.sql

