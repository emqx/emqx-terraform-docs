# 阿里云上部署

这个 Terraform 模块旨在在阿里云上部署 EMQX 或 EMQX Enterprise。EMQX 是一个可扩展且开源的 MQTT 代理，用于连接物联网设备。

## 兼容性

| 操作系统/版本   | EMQX Enterprise 4.4.x | EMQX开源版 4.4.x | EMQX开源版 5.0.x |
| --------------- | --------------------- | ---------------- | ---------------- |
| ubuntu 20.04    | ✓                     | ✓                | ✓                |

## 先决条件

### Terraform

在使用此模块之前，需要先安装 Terraform。如果尚未安装，请参考[官方Terraform安装指南](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/install-and-configure-terraform-on-your-computer)进行安装。

### 阿里云访问密钥

为了在阿里云上部署 EMQX，您需要提供阿里云访问密钥。您可以通过设置环境变量 `ALICLOUD_ACCESS_KEY` 和 `ALICLOUD_SECRET_KEY` 来实现：

```bash
export ALICLOUD_ACCESS_KEY=${access-key}
export ALICLOUD_SECRET_KEY=${secret-key}
```



## 部署 EMQX 集群

### 配置 EMQX4

要部署 EMQX 版本 4.4.x，请在 `emqx_package` 变量中提供软件包URL。将 `${emqx4_package_url}` 替换为实际的 URL。

```bash
emqx_package = ${emqx4_package_url}

```


### 配置 EMQX5

要部署 EMQX 版本 5.0.x，请在 `emqx_package` 变量中提供软件包URL。将 `${emqx5_package_url}` 替换为实际的URL。


```bash
emqx_package = ${emqx5_package_url}
is_emqx5 = true
emqx5_core_count = 1
emqx_instance_count = 4
```


> **Note**

> `emq5_core_count` 应小于或等于 `emqx_instance_count`。

### 运行 Terraform

要应用 Terraform 模块，请进入 `services/emqx_cluster` 目录，并执行以下命令：

```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```


> **Note**

> 如果您正在部署 EMQX Enterprise 并且需要超过10个配额，请申请 EMQX 许可证，并在 `terraform apply` 命令中作为变量传递。

成功应用后，将输出：

```bash
Outputs:

emqx_cluster_address = ${emqx-cluster-ip}
```


您可以使用不同的端口访问不同的服务：

- Dashboard：`${emqx_cluster_address}:18083`
- MQTT：`${emqx_cluster_address}:1883`
- MQTTS：`${emqx_cluster_address}:8883`
- WS：`${emqx_cluster_address}:8083`
- WSS：`${emqx_cluster_address}:8084`

### 清理

当您完成EMQX集群的使用后，可以使用以下命令销毁它：

```bash
terraform destroy -auto-approve
```

这将删除Terraform在该模块中创建的所有资源。


