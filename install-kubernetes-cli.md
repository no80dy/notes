```bash
yc vpc network create network-default
yc vpc subnet create --name network-default-1 --description "Network default 1" --network-id ... --zone ru-central1-a --range 192.168.100.0/24
yc iam service-account create --name resource-service-account
yc iam service-account create --name node-service-account
yc resource-manager folder add-access-binding --id ... --role k8s.clusters.agent --subject serviceAccount:...resource-service-account
yc resource-manager folder add-access-binding --id ... --role logging.writer --subject serviceAccount:...resource-service-account
yc resource-manager folder add-access-binding --id ... --role kms.keys.encrypterDecrypter --subject serviceAccount:...resource-service-account
yc resource-manager folder add-access-binding --id ... --role container-registry.images.puller --subject serviceAccount:...node-service-account
```
