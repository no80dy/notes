```bash
yc vpc network create network-default
yc vpc subnet create --name subnet-default --network-id $(yc vpc network get network-default --format json | jq .id -r) --zone ru-central1-a --range 192.168.1.0/24

yc iam service-account create --name k8s-res-sa-$(yc config get folder-id)
yc resource-manager folder add-access-binding \
--id $(yc config get folder-id) --role editor \
--subject serviceAccount:$(yc iam service-account get --name k8s-res-sa-$(yc config get folder-id) --format json | jq .id -r)

yc iam service-account create --name k8s-node-sa-$(yc config get folder-id)
yc resource-manager folder add-access-binding \
--id $(yc config get folder-id) \
--role container-registry.images.puller \
--subject serviceAccount:$(yc iam service-account get --name k8s-node-sa-$(yc config get folder-id) --format json | jq .id -r)

yc vpc security-group create \
--name security-group-default \
--network-id $(yc vpc network get network-default --format json | jq .id -r) \
--rule "direction=ingress,from-port=0,to-port=65535,protocol=any,v4-cidrs=[0.0.0.0/0]" \
--rule "direction=egress,from-port=0,to-port=65535,protocol=any,v4-cidrs=[0.0.0.0/0]"

yc managed-kubernetes cluster create \
--name k8s-cluster \
--network-id $(yc vpc network get network-default --format json | jq .id -r) \
--service-account-id $(yc iam service-account get --name k8s-res-sa-$(yc config get folder-id) --format json | jq .id -r) \
--node-service-account-id $(yc iam service-account get --name k8s-node-sa-$(yc config get folder-id) --format json | jq .id -r) \
--cluster-ipv4-range 10.96.0.0/16 \
--service-ipv4-range 10.112.0.0/16 \
--public-ip \
--security-group-ids $(yc vpc security-group get security-group-default --format json | jq .id -r) \
--zone ru-central1-a

yc managed-kubernetes node-group create \
--name k8s-default \
--cluster-name k8s-cluster \
--platform standard-v3 \
--cores 2 \
--memory 4 \
--core-fraction 50 \
--disk-type network-ssd \
--fixed-size 2 \
--network-interface subnets=subnet-default,ipv4-address=nat,security-group-ids=[$(yc vpc security-group get security-group-default --format json | jq .id -r)]
```
