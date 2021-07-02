FROM adoptopenjdk:8-jre-hotspot
MAINTAINER LWB

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --force-yes --no-install-recommends \
    wget curl ca-certificates \
    gzip && \
    rm -rf /var/lib/apt/lists/*

ENV MANIFOLDCF_VERSION 2.17
ENV CIFS_VERSION 2.1.2
ENV MANIFOLDCF_REPO http://archive.apache.org/dist/manifoldcf/
ENV ALFRESCO_WEB_CLIENT_VERSION 5.0.a
ENV SQLITE_VERSION 3.36.0.1
ENV JPEG2000_VERSION 1.3.0



RUN wget ${MANIFOLDCF_REPO}/apache-manifoldcf-${MANIFOLDCF_VERSION}/apache-manifoldcf-${MANIFOLDCF_VERSION}-bin.tar.gz && \
    wget https://repo.maven.apache.org/maven2/eu/agno3/jcifs/jcifs-ng/${CIFS_VERSION}/jcifs-ng-${CIFS_VERSION}.jar && \
    wget https://artifacts.alfresco.com/nexus/content/repositories/public/org/alfresco/alfresco-web-service-client/${ALFRESCO_WEB_CLIENT_VERSION}/alfresco-web-service-client-${ALFRESCO_WEB_CLIENT_VERSION}.jar && \
    wget https://repo1.maven.org/maven2/org/xerial/sqlite-jdbc/${SQLITE_VERSION}/sqlite-jdbc-${SQLITE_VERSION}.jar  && \
    wget https://repo1.maven.org/maven2/com/github/jai-imageio/jai-imageio-jpeg2000/${JPEG2000_VERSION}/jai-imageio-jpeg2000-${JPEG2000_VERSION}.jar  && \
    tar -xzvf apache-manifoldcf-${MANIFOLDCF_VERSION}-bin.tar.gz && \
    cp -R apache-manifoldcf-${MANIFOLDCF_VERSION} /usr/share/manifoldcf && \
    cp jcifs-ng-${CIFS_VERSION}.jar /usr/share/manifoldcf/connector-lib-proprietary && \
    cp alfresco-web-service-client-${ALFRESCO_WEB_CLIENT_VERSION}.jar /usr/share/manifoldcf/connector-lib-proprietary && \
    cp sqlite-jdbc-${SQLITE_VERSION}.jar /usr/share/manifoldcf/connector-lib-proprietary && \
    cp jai-imageio-jpeg2000-${JPEG2000_VERSION}.jar /usr/share/manifoldcf/connector-lib-proprietary && \    
    rm *.jar && \
    rm *.tar.gz

RUN sed -i "s/<!--repositoryconnector name=\"Windows shares\" class=\"org.apache.manifoldcf.crawler.connectors.sharedrive.SharedDriveConnector\"\/-->/<repositoryconnector name=\"Windows shares\" class=\"org.apache.manifoldcf.crawler.connectors.sharedrive.SharedDriveConnector\"\/>/g" /usr/share/manifoldcf/connectors.xml && \
    sed -i "s/<property name=\"org.apache.manifoldcf.hsqldbdatabasepath\" value=\".\"\/>/<property name=\"org.apache.manifoldcf.hsqldbdatabasepath\" value=\"\/var\/manifoldcf\/\"\/>/g" /usr/share/manifoldcf/example/properties.xml

EXPOSE 8345

WORKDIR /usr/share/manifoldcf/example
VOLUME /var/manifoldcf

ENTRYPOINT ["java","-XX:+UnlockExperimentalVMOptions","-XX:+UseCGroupMemoryLimitForHeap"]
CMD ["-jar", "start.jar"]