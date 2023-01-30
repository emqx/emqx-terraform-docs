# 阿里云上部署

## 兼容性

|                          | EMQX 4.4.x      |
|--------------------------|-----------------|
| ubuntu 20.04             | ✓               |

> **Note**

- 当前不支持 EMQX 5.x
- 不支持 TLS

## 安装 terraform
> 请参考 [terraform安装文档](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## 阿里云 AccessKey Pair

```bash
export ALICLOUD_ACCESS_KEY="anaccesskey"
export ALICLOUD_SECRET_KEY="asecretkey"
export ALICLOUD_REGION="cn-shenzhen"
```

## 部署 EMQ X 单节点
```bash
cd services/emqx
terraform init
terraform plan
terraform apply -auto-approve
```

## 销毁 EMQ X 集群（默认 2 节点）
```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note**

> 如果部署企业版你需要申请一个 license, 否则只有10个客户端

## 销毁
```bash
terraform destroy -auto-approve
```




