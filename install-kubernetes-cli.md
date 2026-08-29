```bash
yc vpc network create network-default
yc vpc subnet create --name network-default-1 --description "Network default 1" --network-id ... --zone ru-central1-a --range 192.168.100.0/24
yc iam service-account create --name resource-service-account
yc iam service-account create --name node-service-account
yc resource-manager folder add-access-binding --id ... --role k8s.clusters.agent --subject serviceAccount:...resource-service-account
yc resource-manager folder add-access-binding --id ... --role logging.writer --subject serviceAccount:...resource-service-account
yc resource-manager folder add-access-binding --id ... --role kms.keys.encrypterDecrypter --subject serviceAccount:...resource-service-account
yc resource-manager folder add-access-binding --id ... --role container-registry.images.puller --subject serviceAccount:...node-service-account
yc kms symmetric-key create --name k8s-kms-key --default-algorithm aes-256 --rotation-period 24h --deletion-protection
yc vpc security-group create k8s-security-group --network-name network-default
yc managed-kubernetes cluster create   --name k8s-default   --network-name network-default   --public-ip   --release-channel rapid   --version 1.33   --cluster-ipv4-range 10.96.0.0/16   --service-ipv4-range 10.112.0.0/20   --security-group-ids enpn50290uui309artt5   --service-account-name resource-service-account   --node-service-account-name node-service-account   --master-location zone=ru-central1-a,subnet-name=network-default-1  --daily-maintenance-window start=22:00,duration=10h   --labels environment=production,team=devops

---
```
yc vpc security-group update-rules enpn50290uui309artt5 \
  --add-rule direction=ingress,port=443,protocol=tcp,v4-cidrs=[0.0.0.0/0],description="Kubernetes API" \
  --add-rule direction=ingress,protocol=any,port=any,v4-cidrs=[10.0.0.0/8,192.168.0.0/16],description="Internal traffic" \
  --add-rule direction=ingress,port=22,protocol=tcp,v4-cidrs=[0.0.0.0/0],description="SSH" \
  --add-rule direction=ingress,port=10250,protocol=tcp,v4-cidrs=[0.0.0.0/0],description="Kubelet" \
  --add-rule direction=ingress,from-port=30000,to-port=32767,protocol=tcp,v4-cidrs=[0.0.0.0/0],description="NodePort"
