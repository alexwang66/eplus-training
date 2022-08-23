
# CLI:
## Maven
# Configure the project with Artifactory servers and repositories
-  jfrog mvnc --server-id-resolve=artifactory-server --repo-resolve-releases=alex-maven-virtual --repo-resolve-snapshots=alex-maven-snapshot --server-id-deploy=artifactory-server --repo-deploy-releases=alex-maven-local --repo-deploy-snapshots=alex-maven-snapshot
# Build the project, while deploying artifacts to Artifactory
-jf mvn clean install --build-name alex-maven --build-number 10
# Collect environment variables
- jf rt bce alex-maven  10
# Collect git details and tracked project issues
- jf rt bag alex-maven ./
# Publish build info
- jfrog rt bp alex-maven 10
# Trigger Xray Scan on the published build
- jfrog rt bs alex-maven 1 alex-maven  10
# Promote build
- jfrog rt bpr promotion-repo
