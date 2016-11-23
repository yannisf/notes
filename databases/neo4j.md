# neo4j

* Relationships always have direction
* Relationships always have a type
* Relationships can have properties

## docker
[Run neo4j using docker](http://neo4j.com/developer/docker/)

### interactive with terminal
```
docker run --interactive --rm --publish 7474:7474 --volume $HOME/neo4j/data:/data neo4j
```

### as daemon running in the background
```
docker run --detach --name neo4j --publish 7474:7474 --volume $HOME/neo4j/data:/data neo4j
```

## Cypher

### Create node
```
CREATE (ee:Person { name: "Emil", from: "Sweden" }) RETURN ee;
```

### Match node
```
MATCH (ee:Person) WHERE ee.name = "Emil" RETURN ee;
```

### Create nodes and relationships
```
CREATE (ee:Person { name: "Emil", from: "Sweden", klout: 99 }),
(js:Person { name: "Johan", from: "Sweden", learn: "surfing" }),
(ir:Person { name: "Ian", from: "England", title: "author" }),
(rvb:Person { name: "Rik", from: "Belgium", pet: "Orval" }),
(ally:Person { name: "Allison", from: "California", hobby: "surfing" }),
(ee)-[:KNOWS {since: 2001}]->(js),(ee)-[:KNOWS {rating: 5}]->(ir),
(js)-[:KNOWS]->(ir),(js)-[:KNOWS]->(rvb),
(ir)-[:KNOWS]->(js),(ir)-[:KNOWS]->(ally),
(rvb)-[:KNOWS]->(ally);
```

### Pattern matching
MATCH (ee:Person)-[:KNOWS]->(friends:Person) 
WHERE ee.name = "Emil" RETURN friends;

MATCH <pattern> WHERE <conditions> RETURN <expressions>

## REST API
http://localhost:7474/db/data/node/1

http://localhost:7474/db/data/relationship/1
