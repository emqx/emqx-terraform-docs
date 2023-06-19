# AWS上部署

此 Terraform 模块旨在在亚马逊云服务（AWS）上部署 EMQX 或 EMQX Enterprise。EMQX 是一个可扩展的开源 MQTT 代理，用于连接物联网设备。


## 兼容性

| 操作系统/版本   | EMQX Enterprise 4.4.x | EMQX开源版 4.4.x | EMQX开源版 5.0.x |
| --------------- | --------------------- | ---------------- | ---------------- |
| ubuntu 20.04    | ✓                     | ✓                | ✓                |


## 先决条件

### Terraform

此模块需要安装 Terraform。如果尚未安装，请按照官方 Terraform 安装指南进行操作。

### AWS访问密钥
为了在 AWS 上部署 EMQX，您需要提供AWS访问密钥。您可以通过导出 `AWS_ACCESS_KEY_ID` 和 `AWS_SECRET_ACCESS_KEY` 环境变量来完成此操作：

```bash
export AWS_ACCESS_KEY_ID=${access-key}
export AWS_SECRET_ACCESS_KEY=${secret-key}
```

## 部署 EMQX 集群

### 配置 EMQX4
要部署 EMQX 4.4.x版本，请在 emqx4_package 变量中提供软件包 URL。将 `${emqx4_package_url}` 替换为实际的 URL。

```bash
emqx4_package = ${emqx4_package_url}
```

### 配置 EMQX5
要部署 EMQX 5.0.x版本，请在 `emqx5_package` 变量中提供软件包 URL。将 `${emqx5_package_url}` 替换为实际的URL。

```bash
emqx5_package = ${emqx5_package_url}
is_emqx5 = true
emqx5_core_count = 1
emqx_instance_count = 4
```

> **Note**

> `emq5_core_count` 应小于或等于 `emqx_instance_count`。

### 运行Terraform

要应用 Terraform 模块，请转到 services/emqx_cluster 目录，并运行以下命令：

```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note**

> 如果您要部署 EMQX Enterprise 并且需要超过10个配额，请申请 EMQX 许可证，并在 terraform apply 命令期间将其作为变量传递。

应用成功后，它将输出：

```bash
Outputs:

emqx_cluster_address = "${prefix}.elb.${region}.amazonaws.com"
```

如果要将地址与域名关联起来，您需要配置 CNAME 记录。

您可以使用不同的端口访问不同的服务：

```bash
Dashboard：${emqx_cluster_address}:18083
MQTT：${emqx_cluster_address}:1883
MQTTS：${emqx_cluster_address}:8883
WS：${emqx_cluster_address}:8083
WSS：${emqx_cluster_address}:8084
```

### 清理
-----
当您完成 EMQX 集群的使用后，可以使用以下命令销毁它：

```bash
terraform destroy -auto-approve
```

这将删除 Terraform 在该模块中创建的所有资源。


