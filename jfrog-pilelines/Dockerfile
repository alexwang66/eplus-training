#Download image from artifactory
ARG REGISTRY=http://demo.jfrogchina.com/artifactory

ENV jfrogchina_token ${jfrogchina_token}
FROM openjdk:11-jdk
# FROM $REGISTRY/openjdk:11-jdk

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y nodejs \
    npm                       # note this one

WORKDIR /app

#Define ARG Again -ARG variables declared before the first FROM need to be declered again
ARG REGISTRY=http://demo.jfrogchina.com/artifactory
MAINTAINER Alex Wang

# Download artifacts from Artifactory
#RUN curl $REGISTRY/alex-maven-local/com/jfrog/backend/1.0.0/backend-1.0.0.jar --output server1.jar
#RUN curl $REGISTRY/alex-npm-local/frontend/-/frontend-3.0.0.tgz --output client1.tgz


#Extract vue app
RUN curl -fL https://install-cli.jfrog.io | sh
RUN jf c im ${jfrogchina_token}
RUN jf rt dl alex-maven-local/com/jfrog/backend/1.0.0/backend-1.0.0.jar ./server1.jar
RUN jf rt dl alex-npm-local/frontend/-/frontend-3.0.0.tgz ./client1.tgz
RUN mv frontend/-/client1.tgz ./client1.tgz
RUN tar -xzf client1.tgz && rm client1.tgz
RUN mv com/jfrog/backend/1.0.0/server1.jar /app/server1.jar

WORKDIR "package"

RUN npm install

RUN npm run build

# Set JAVA OPTS + Static file location
ENV STATIC_FILE_LOCATION="/app/package/target/dist/"
ENV JAVA_OPTS=""

# Fire up our Spring Boot app
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Dspring.profiles.active=remote -Djava.security.egd=file:/dev/./urandom -jar /app/server1.jar" ]

