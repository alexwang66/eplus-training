# Docker build

## Set env in Linux system
vi ~/.bash_profile

export jfrogchina_token=your-token

source ~/.bash_profile

## Docker build
docker build -t demo.jfrogchina.com/docker-local/docker-app .  --build-arg jfrogchina_token=${jfrogchina_token}

# SwampUp 2022 Pipelines

kubectl create secret docker-registry regcred \
  --docker-server=${art_server} \
  --docker-username=admin \
  --docker-password=${apikey} \
  --docker-email=wq237wq@gmail.com