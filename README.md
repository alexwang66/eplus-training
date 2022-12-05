# SwampUp 2022 Pipelines

# Artifactory Developer:


Rest API

查看文件
curl -i -uadmin:${jfrogchina_apikey} https://server:443/artifactory/app1-maven-release-local/org/jfrog/test/multi3/4.7/multi3-4.7.war

更新制品属性
curl -H "X-JFrog-Art-Api:${jfrogchina_apikey}" -X PUT "https://server:443/artifactory/api/storage/app1-maven-release-local/org/jfrog/test/multi3/4.7/multi3-4.7.war?properties=os=win,linux;qa=done&recursive=1"

Build Tools
CLI Mvn

cd jfrog-pilelines/java-backend-service

## Maven
   # Configure the project with Artifactory servers and repositories 
   -  jf mvnc --server-id-resolve=jfrogchina --repo-resolve-releases=eplus-libs-release --repo-resolve-snapshots=eplus-libs-release --server-id-deploy=jfrogchina --repo-deploy-releases=eplus-libs-release --repo-deploy-snapshots=eplus-libs-release
   # Build the project, while deploying artifacts to Artifactory
   -jf mvn install --build-name eplus-libs-release --build-number 1
 
 # set properties
jf rt sp "eplus-libs-release-local/com/jfrog/backend/1.0.0/backend-1.0.0.jar" "build.name=eplus-libs-release;build.number=1"


   # Publish build info
   - jfrog rt bp eplus-libs-release 1

   # Promote build
   -  jfrog rt bpr  eplus-libs-release 1 demo-mvn-prod-local --copy=true --props="released=true"


CLI Build NPM
jf npm install --build-name=my-build-name --build-number=1
jf npm publish --build-name=my-build-name --build-number=1


CLI Build Docker image
Cd swampup2020/SUV-007-CI-CD-Deep-Dive:JFrog-Pipelines
docker build -t docker-app .
docker run -d -p 8088:8088 --name docker-app consolidate-app



docker login demo.jfrogchina.com
➜  /etc docker tag docker-app server/docker-local/docker-app
➜  /etc docker push server/docker-local/docker-app

jf docker push server/docker-local/docker-app --build-name=docker-app --build-number=1
jf rt bp docker-app 1

Maven plugin:
<build>
    <plugins>
        ...
        <plugin>
            <groupId>org.jfrog.buildinfo</groupId>
            <artifactId>artifactory-maven-plugin</artifactId>
            <version>3.2.3</version>
            <inherited>false</inherited>
            <executions>
                <execution>
                    <id>build-info</id>
                    <goals>
                        <goal>publish</goal>
                    </goals>
                    <configuration>
                        <deployProperties>
                            <gradle>awesome</gradle>
                            <review.team>qa</review.team>
                        </deployProperties>
                        <publisher>
                            <contextUrl>https://server</contextUrl>
                            <username>admin</username>
                            <password>{DESede}...</password>
                            <repoKey>eplus-libs-release</repoKey>
                            <snapshotRepoKey>libs-snapshot-local</snapshotRepoKey>
                        </publisher>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

Build Helm chart
helm install  docker-app docker-app-chart
kc get svc                         
NAME                          TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)          AGE
docker-app-docker-app-chart   LoadBalancer   192.168.34.120   39.106.131.127   8088:31420/TCP   52s
kubernetes                    ClusterIP      192.168.0.1      <none>           443/TCP          37d

Visit: http://server:8088/index.html#/bootstrap

AQL
Vi findByBuildName.aql 
{
"files": [{
    "aql": {
        "items.find": {
            "@build.name": {"$eq":"eplus-libs-release"}
        }
    }
}]}

jf rt dl --spec=findByBuildName.aql --dry-run
# Artifactory Admin
Replication
Federated repo
Access Federation
Access token
User plugin
	
# Xray

# Pipeline
