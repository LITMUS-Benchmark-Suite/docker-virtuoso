# docker-virtuoso
Docker for Openlink Virtuoso engine

## INSTALING & RUNNING VIRTUOSO

docker run --name my-virtuoso -p 8890:8890 -p 1111:1111 -e DBA_PASSWORD=admin -e SPARQL_UPDATE=true -e DEFAULT_GRAPH=http://www.example.com/my-graph -v /home/batman/Desktop/virtuoso/db:/data -d tenforce/virtuoso

username = dba \\fb admin configuration
pass = admin

## START DOCKER
docker start my-virtuoso
docker exec -it my-virtuoso /bin/bash

## LOADING DATA ===
BSBM DATA:

isql-v -U dba -P admin
ld_dir('dumps', 'filename.nt', 'http:bsbm_<size>.org');
rdf_loader_run();
select * from DB.DBA.load_list; \\ lists all data which has been loaded so far, a status integer 2 means load was usccessful
SELECT ll_graph, ll_file FROM DB.DBA.LOAD_LIST; \\ optional
checkpoint;
commit WORK;
checkpoint;

### DELETING DATA
DELETE FROM DB.DBA.LOAD_LIST WHERE ll_graph=’http:bsbm_1M.org’;
