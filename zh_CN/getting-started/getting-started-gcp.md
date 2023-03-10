# GCP 上部署

## 兼容性

|                          | EMQX 4.4.x      |
|--------------------------|-----------------|
| ubuntu 20.04             | ✓               |

> **Note**

- 当前不支持 EMQX 5.x
- 不支持 TLS


## 安装 terraform

> 请参考 [terraform install doc](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## 配置GCP 证书
参考这篇文档
[guide](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/getting_started#adding-credentials)


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
loadbalancer_ip = ${loadbalancer_ip}
```

通过相关的端口访问不同的服务

```bash
Dashboard: ${loadbalancer_ip}:18083
MQTT: ${loadbalancer_ip}:1883
```

## 销毁
```bash
terraform destroy -auto-approve
```

