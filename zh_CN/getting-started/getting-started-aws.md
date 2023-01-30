# AWS上部署


## 兼容性

|                          | EMQX 4.4.x      |
|--------------------------|-----------------|
| ubuntu 20.04             | ✓               |

> **Note**

- 当前不支持 EMQX 5.x


## 安装 terraform

> 请参考 [terraform install doc](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## 配置 AWS AccessKey Pair

```bash
export AWS_ACCESS_KEY_ID="anaccesskey"
export AWS_SECRET_ACCESS_KEY="asecretkey"
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

> 如果部署企业版你需要申请一个 license, 否则只有10个客户端

如果 apply 成功，将输出：

```bash
Outputs:

emqx_cluster_address = "${prefix}.elb.${region}.amazonaws.com"
```

如果你想绑定一个域名，你需要配置 CNAME

你能访问不同的服务通过不同的端口
```bash
Dashboard: ${prefix}.elb.${region}.amazonaws.com:18083
MQTT: ${prefix}.elb.${region}.amazonaws.com:1883
MQTTS: ${prefix}.elb.${region}.amazonaws.com:8883
WS: ${prefix}.elb.${region}.amazonaws.com:8083
WSS: ${prefix}.elb.${region}.amazonaws.com:8084
```

## 销毁
```bash
terraform destroy -auto-approve
```
