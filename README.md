# docker-virtuoso
Docker for Openlink Virtuoso engine

## INSTALING & RUNNING VIRTUOSO

```bash
docker run --name my-virtuoso -p 8890:8890 -p 1111:1111 -e DBA_PASSWORD=admin -e SPARQL_UPDATE=true -e DEFAULT_GRAPH=http://www.example.com/my-graph -v /home/batman/Desktop/virtuoso/db:/data -d tenforce/virtuoso
```

username = dba \\fb admin configuration

pass = admin


## START DOCKER
```bash
docker start my-virtuoso
docker exec -it my-virtuoso /bin/bash
```

## LOADING DATA
BSBM DATA:
```bash
isql-v -U dba -P admin 

ld_dir('dumps', 'filename.nt', 'http:bsbm_<size>.org');

rdf_loader_run();

select * from DB.DBA.load_list; 
\\ lists all data which has been loaded so far, a status integer 2 means load was successful

SELECT ll_graph, ll_file FROM DB.DBA.LOAD_LIST; \\ optional

checkpoint;

commit WORK;

checkpoint;
```


### DELETING DATA
```bash
DELETE FROM DB.DBA.LOAD_LIST WHERE ll_graph=’http:bsbm_1M.org’;
```
## QUERYING:
### QUERYING USING BSBM TEST DRIVER
To query using the BSBM test generator, first navigate to the bsbm-tools folder from which you created the dataset. Then, use the following command:

```bash
java -cp bin:lib/* benchmark.testdriver.TestDriver http://localhost/sparql
```
OR, if your java doesnot recognize asterisks '*' then;
```bash
java -cp bin:lib/ssj.jar:lib/log4j-1.2.15.jar benchmark.testdriver.TestDriver http://localhost/sparql
```

Also, you can type the following command to run the test driver via its executable provided with the bsbm-tools;
```bash
./testdriver [-ucf usecases/explore/sparql.txt] [-u http://localhost/update] [-udataset dataset_update.nt] [-runs 128] [-w 32] [-t 30000] http://localhost/sparql
```
where;
-t = timeout (msecs); 30s in above exmaple;

-runs = no. of query mixes to run; default=50;

-w = no. of warmup queries; default=10;

-ucf = path to usecase file;

-o = output file containing the aggregated result overview. Default: "benchmark_result.xml";

-dg = Specify a default graph for the queries. Default: null;

-mt = Benchmark with multiple concurrent clients. e.g.: 4;

-rampup = Run test driver in ramp-up/warm-up mode. The test driver will execute randomized queries until it is stopped - ideally when the store reached steady state and is not improving any more.;

-u = <Service endpoint URI for SPARQL Update> If you are running update queries in your tests this option defines where the SPARQL update service endpoint can be found.

-udataset = The file name of the update dataset.

-uqp  = <update query parameter>; The forms parameter name for the SPARQL Update query string. Default: update


source: http://wifo5-03.informatik.uni-mannheim.de/bizer/berlinsparqlbenchmark/spec/BenchmarkRules/index.html#testdriver

