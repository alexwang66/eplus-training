#Download image from artifactory
ARG REGISTRY=docker.artifactory
ARG art_server=https://demo.jfrogchina.com
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
RUN jf c im eyJ2ZXJzaW9uIjoyLCJ1cmwiOiJodHRwOi8vZGVtby5qZnJvZ2NoaW5hLmNvbS8iLCJhcnRpZmFjdG9yeVVybCI6Imh0dHA6Ly9kZW1vLmpmcm9nY2hpbmEuY29tL2FydGlmYWN0b3J5LyIsImRpc3RyaWJ1dGlvblVybCI6Imh0dHA6Ly9kZW1vLmpmcm9nY2hpbmEuY29tL2Rpc3RyaWJ1dGlvbi8iLCJ4cmF5VXJsIjoiaHR0cDovL2RlbW8uamZyb2djaGluYS5jb20veHJheS8iLCJtaXNzaW9uQ29udHJvbFVybCI6Imh0dHA6Ly9kZW1vLmpmcm9nY2hpbmEuY29tL21jLyIsInBpcGVsaW5lc1VybCI6Imh0dHA6Ly9kZW1vLmpmcm9nY2hpbmEuY29tL3BpcGVsaW5lcy8iLCJ1c2VyIjoiYWRtaW4iLCJwYXNzd29yZCI6IkFQMjR4UGc4UkVLNlZURUFLbmUxcjQ5dmhxZCIsImFjY2Vzc1Rva2VuIjoiZXlKMlpYSWlPaUl5SWl3aWRIbHdJam9pU2xkVUlpd2lZV3huSWpvaVVsTXlOVFlpTENKcmFXUWlPaUpRZUdwNVRsWndaVlUxVTBKcE5rdFBlbHAxTVdaT2FVc3RVazlIWHpJNE4ycHJPRkZXVWpaTFVtcE5JbjAuZXlKemRXSWlPaUpxWm5KMFFEQXhaelozYTNsNmRtUnhOWGgyTVRaaGJuQjBZelV4TW5jMFhDOTFjMlZ5YzF3dllXUnRhVzRpTENKelkzQWlPaUp0WlcxaVpYSXRiMll0WjNKdmRYQnpPaW9pTENKaGRXUWlPaUpxWm5KMFFEQXhaelozYTNsNmRtUnhOWGgyTVRaaGJuQjBZelV4TW5jMElpd2lhWE56SWpvaWFtWnlkRUF3TVdjMmQydDVlblprY1RWNGRqRTJZVzV3ZEdNMU1USjNORnd2ZFhObGNuTmNMMkZrYldsdUlpd2laWGh3SWpveE5qVTVNelF3TlRnMExDSnBZWFFpT2pFMk5Ua3pNelk1T0RRc0ltcDBhU0k2SW1Gak1UazVPR0psTFRRMU5EQXROREkyTWkwNU4yRXhMVEZsTXpWbE5qVmxOekJoWlNKOS5FaDJua0c2Q292aU9LdklCcENyTVJwdkFvMmUzbnp2ZEtMSEFvNjZXRUpxcm9ZXzJXWVZMbEtUT2VHRDc0QUdOOHhaLVdzdno1MG1FUm4zWXdjbDZObjR4ZHBmYmlZMUR2Wk5yb2V4NUVyRXhtN25YYmZROXZkZFBuWHRVLVdGSlJ4bU4wNnRwaktVcVF6dzUzdExGdXJQX2pRTVNKOUNiMHcxZnpfYjVuTTJ6elNRMHdOSnJtUVhEYkdLR3NSdGFsOE5YemljOC1JR0poNFVoalZ6eWtlSTB5NU16WVlpVnpkcTE2V3JZTDhCT1BZN2ZHTU1ra2RwdlpheWI3SXJybDljaFpCOXRPWlRMTjk5UTlqS1ltbk1Bd2VSc2d3Vlc1Ym0yb0JlMl9DVG0xNUJjWU1xdklRcnY0UGhoYVotbzA1Y0JBN0pXN2JxRUY5MkNhd0EtZnciLCJyZWZyZXNoVG9rZW4iOiIwMTQwYmE1My1jOTQ0LTRkMDYtYmMzYS01Zjc3YjNlYjIwZDUiLCJzZXJ2ZXJJZCI6Impmcm9nY2hpbmEifQ==
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

