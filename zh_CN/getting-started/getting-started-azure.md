# Azure上部署

## 兼容性

|                          | EMQX 4.4.x      |
|--------------------------|-----------------|
| ubuntu 20.04             | ✓               |

> **Note**

> 当前不支持 EMQX 5.x
不支持 TLS


## 安装 terraform
Please refer to [terraform install doc](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## 配置 azure 证书
You could follow this [guide](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret)
```bash
export ARM_SUBSCRIPTION_ID=${ARM_SUBSCRIPTION_ID}
export ARM_TENANT_ID=${ARM_TENANT_ID}
export ARM_CLIENT_ID=${ARM_CLIENT_ID}
export ARM_CLIENT_SECRET=${ARM_CLIENT_SECRET}
```

## 部署 EMQX 单节点
```bash
cd services/emqx
terraform init
terraform plan
terraform apply -auto-approve
```


## 部署 EMQX 集群
```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note**

> 如果部署企业版你需要申请一个license, 否则只有10个客户端


如果apply成功，将输出：

```bash
Outputs:
loadbalancer_public_ip = ${loadbalancer_public_ip}
```

通过相关的端口访问不同的服务

```bash
Dashboard: ${loadbalancer_public_ip}:18083
MQTT: ${loadbalancer_public_ip}:1883
```

## 销毁
```bash
terraform destroy -auto-approve
```

