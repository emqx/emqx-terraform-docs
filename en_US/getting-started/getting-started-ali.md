# Deploy on Ali Cloud

## Compatability

|                          | EMQX 4.4.x      |
|--------------------------|-----------------|
| ubuntu 20.04             | ✓               |

> **Note**

- Not support EMQX 5.x currently
- Not support TLS

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

> **Note**

> You should apply for an emqx license if you want more than 10 quotas when deploying emqx enterprise.


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


