# Neo4j
Download Neo4j from based on your device <a href='https://neo4j.com/download-center/' target="_blank">Neo4j Download Center</a>

Extract the downloaded file.

Place the extracted files in a permanent home on your server, for example D:\neo4j\.

Set path on environment variable.

Run the command <code>D:\neo4j>neo4j install-service</code> to install the service

Start neo4j by running command <code>neo4j start</code>

Open neo4j browser using commands below:
  1) console             Start server in console.
  2) start               Start server as a daemon.
  3) stop                Stop the server daemon.
  4) restart             Restart the server daemon.
  5) status              Get the status of the server.
  6) install-service     Install the Windows service.
  
Visit <a href:'http://localhost:7474' target="_blank">http://localhost:7474</a> in your web browser.

Add the json files of product in import folder

For Relation Between Attributes, Services, Brands, Manufacturer Copy the below and run.

<code>
CALL apoc.load.json('file:/product.json') YIELD value
CREATE (p:Product {prod_name: value.prod_title, product: value._id})
SET p.product = value._id
WITH p, value
MERGE (m:Manufacture {manufacturer: value.schema.manufacturer})
MERGE (p)-[:Manufactured_by]->(m)
MERGE (b:Branded {brand: value.schema.brand.name})
MERGE (p)-[:Branded_by]->(b)
CREATE (a:Attributes {
    Prod_Title: value.prod_title,
    Image: value.schema.image,
    Application: value.specifications.Application,
    Capacity: value.specifications.Capacity
    })
MERGE (p)-[:Attributes]->(a)
MERGE (f:Features {
    feature: value['product-features']})
MERGE (p)-[:Features]->(f)
return p,m,b,a,f limit 25;

</code>

<h3>Output</h3>

![overview_of_relation](https://user-images.githubusercontent.com/51017576/204612407-7d001cd7-88b4-4b6e-9f4b-474dfcba6b4d.png)

![relation_of_product_ _attributes](https://user-images.githubusercontent.com/51017576/204611901-7ca54666-9c63-4e08-bf9c-283ee3907ae2.png)
