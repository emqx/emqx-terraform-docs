# AWS上部署

> 默认安装Region：us-east-1

## 安装terraform
> 请参考 [terraform install doc](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## 配置AWS AccessKey Pair
```bash
export AWS_ACCESS_KEY_ID="anaccesskey"
export AWS_SECRET_ACCESS_KEY="asecretkey"
```

## 部署EMQX单节点
```bash
cd services/emqx
terraform init
terraform plan
terraform apply -auto-approve
```


## 部署EMQX集群
```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve -var="ee_lic=${ee_lic}" -var="region=${region}"
```
**Note:** 如果部署企业版你需要申请一个license


如果apply成功，将输出：
```bash
Outputs:

emqx_cluster_address = "${prefix}.elb.${region}.amazonaws.com"
```

如果你想绑定一个域名，你需要配置CNAME

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

**Note:** 由于节点用的是ubuntu 20.04，所以你需要指定相应操作系统的EMQX安装包
