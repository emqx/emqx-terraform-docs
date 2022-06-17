# Deploy on Ali Cloud

> Default Region：cn-shenzhen

## Install terraform
> Please refer to [terraform install doc](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## Alicloud AccessKey Pair
```bash
export ALICLOUD_ACCESS_KEY="anaccesskey"
export ALICLOUD_SECRET_KEY="asecretkey"
export ALICLOUD_REGION="cn-shenzhen"
```

## Deploy EMQ X single node
```bash
cd services/emqx
terraform init
terraform plan
terraform apply -auto-approve
```


## Deploy EMQ X cluster(default 2 node)
```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

## Destroy
```bash
terraform destroy -auto-approve
```

**Note:** Due to ubuntu 20.04 of node installed, you need to config emqx package associate with corresponding os version.


