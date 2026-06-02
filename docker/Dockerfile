# See README for information how solr coradefaultcore was created
FROM solr:9.9.0-slim

ADD coradefaultcore /opt/solr/server/solr/configsets/coradefaultcore 
RUN ls -la /opt/solr/server/solr/configsets/coradefaultcore
USER root
RUN chmod +rwx /opt/solr/server/solr/configsets/coradefaultcore -R
USER solr