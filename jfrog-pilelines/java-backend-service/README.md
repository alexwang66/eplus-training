
# CLI:
## Maven
# Configure the project with Artifactory servers and repositories
-  jf mvnc --repo-resolve-releases=alex-maven --server-id-resolve=jfrogchina --server-id-deploy=jfrogchina --repo-resolve-snapshots=alex-maven --repo-deploy-releases=alex-maven --repo-deploy-snapshots=alex-maven
# Build the project, while deploying artifacts to Artifactory
-jf mvn clean install --build-name alex-maven --build-number 15
# upload file
- jf rt u target/backend-1.0.0.jar alex-maven-local --build-name=alex-maven --build-number=15
# Collect environment variables
- jf rt bce alex-maven  15
# Collect git details and tracked project issues
- jf rt bag alex-maven ../../
# Publish build info
- jfrog rt bp alex-maven 15
# Trigger Xray Scan on the published build
- jfrog bs alex-maven 15
# Promote build
- jfrog rt bpr promotion-repo
