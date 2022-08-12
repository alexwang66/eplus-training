# SwampUp 2022 Pipelines

kubectl create secret docker-registry regcred \
  --docker-server=${art_server} \
  --docker-username=admin \
  --docker-password=${apikey} \
  --docker-email=wq237wq@gmail.com