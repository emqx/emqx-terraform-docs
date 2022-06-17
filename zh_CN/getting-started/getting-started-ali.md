# 阿里云上部署

> 默认安装Region：cn-shenzhen

## 安装terraform
> 请参考 [terraform安装文档](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## 阿里云AccessKey Pair
```bash
export ALICLOUD_ACCESS_KEY="anaccesskey"
export ALICLOUD_SECRET_KEY="asecretkey"
export ALICLOUD_REGION="cn-shenzhen"
```

## 部署EMQ X单节点
```bash
cd services/emqx
terraform init
terraform plan
terraform apply -auto-approve
```

## 销毁EMQ X集群（默认 2 节点）
```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

## 销毁
```bash
terraform destroy -auto-approve
```


**Note:** 由于节点用的是ubuntu 20.04，所以你需要指定相应操作系统的EMQX安装包


