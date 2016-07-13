# docker-virtuoso
Docker for Openlink Virtuoso engine

== INSTALING & RUNNING VIRTUOSO ==

docker run --name my-virtuoso -p 8890:8890 -p 1111:1111 -e DBA_PASSWORD=admin -e SPARQL_UPDATE=true -e DEFAULT_GRAPH=http://www.example.com/my-graph -v /home/batman/Desktop/virtuoso/db:/data -d tenforce/virtuoso

username = dba
pass = admin

=== LOAD DATA ===
*BSBM DATA *

docker exec -it my-virtuoso bash
=======
== BSBM DATA ==
docker start my-virtuoso
docker exec -it my-virtuoso /bin/bash
isql-v -U dba -P admin
ld_dir('dumps', 'scale2785_1M.nt', 'http:bsbm_1M.org');
rdf_loader_run();
select * from DB.DBA.load_list;
SELECT ll_graph, ll_file FROM DB.DBA.LOAD_LIST; 
checkpoint;
commit WORK;
checkpoint;

== DELETING DATA ==
DELETE FROM DB.DBA.LOAD_LIST WHERE ll_graph=’http:bsbm_1M.org’;
