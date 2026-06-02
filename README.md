# How to create coradefaultcore

*ideas from: https://github.com/docker-solr/docker-solr-examples/tree/master/schema-api*

##created by:
docker run --rm -d --name solr-initial -p 8983:8983 solr:8.11-slim solr-precreate coradefaultcore

###added field to schema:
curl -X POST -H 'Content-type:application/json' --data-binary '{
  "add-field":{
    "name":"ids",
    "type":"string",
    "multiValued":true,
    "indexed":true,
    "required":true,
    "stored":true
    }
}' http://localhost:8983/solr/coradefaultcore/schema

docker cp solr-initial:/var/solr/data/coradefaultcore /mnt/depot/cora/eclipse202112forcora1/workspace/cora-docker-solr/
docker kill solr-initial

changed *_t to be multivalued in managed-schema.xml 
<dynamicField name="*_t" type="text_general" multiValued="true" indexed="true" stored="true"/>
changed *_s to be multivalued in managed-schema.xml 
<dynamicField name="*_s" type="string" multiValued="true" indexed="true" stored="true"/>
remove file core.properties 

added the following to requestHandler / defaults
<str name="defType">edismax</str>
<str name="q.op">AND</str>
<str name="mm">100%</str>
      

##build with:
maven build

##start on development with:
`docker run --net=cora -p 8984:8983 --name solr  -d  -P solr solr-create -c coracore`

##start on build server with:
`docker run --net=cora-test  --name solr-test -d  solr -P solr solr-create -c coracore`

##start on external with:
`docker run --net=cora  --name solr  -d  solr -P solr solr-create -c coracore`

##reomove unused volumes with:
`docker volume rm $(docker volume ls -q)`