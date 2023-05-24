# Azure上部署

此项目提供了一个 Terraform 脚本，用于在 Microsoft Azure 上部署 EMQX 的开源版或企业版。EMQX 是一个开源的、分布式的 MQTT 消息代理，用于物联网应用程序，设计用于处理大量并发客户端连接。

## 兼容性

| 操作系统/版本   | EMQX Enterprise 4.4.x | EMQX开源版 4.4.x | EMQX开源版 5.0.x |
| --------------- | --------------------- | ---------------- | ---------------- |
| ubuntu 20.04    | ✓                     | ✓                | ✓                |


## 先决条件

- 具有必要权限的Azure帐户
- 在您的机器上安装了Terraform。如果没有，请按照此[指南](https://developer.hashicorp.com/terraform/tutorials/azure-get-started/install-cli)进行操作。
- 安装并配置了 Azure CLI

## 配置

### Azure凭据

按照此[指南](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret)设置 Azure 凭据。设置完毕后，导出 Azure 凭据：

```bash
export ARM_SUBSCRIPTION_ID=${ARM_SUBSCRIPTION_ID}
export ARM_TENANT_ID=${ARM_TENANT_ID}
export ARM_CLIENT_ID=${ARM_CLIENT_ID}
export ARM_CLIENT_SECRET=${ARM_CLIENT_SECRET}
```

### 配置EMQX4
要部署EMQX 4.4.x版本，请在emqx4_package变量中提供软件包URL。将${emqx4_package_url}替换为实际的URL。

```bash
emqx4_package = ${emqx4_package_url}
```

### 配置EMQX5

要部署 EMQX 5.0.x 版本，请在 `emqx5_package` 变量中提供软件包URL。将 `${emqx5_package_url}` 替换为实际的 URL。

```bash
emqx5_package = ${emqx5_package_url}
is_emqx5 = true
emqx5_core_count = 1
emqx_vm_count = 4
```
> **Note**

> `emq5_core_count` 应小于或等于 `emqx_vm_count`。

## 部署

### 部署 EMQX 集群

要部署 EMQX 集群，请运行以下命令：

```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note**
> 如果您要部署 EMQX Enterprise 并且需要超过10个节点，请申请 EMQX 许可证。

运行以下命令应用许可证：

```bash
terraform apply -auto-approve -var="emqx_lic=${your_license_content}"
```

成功部署后，您将看到以下输出：

```bash
Outputs:

loadbalancer_public_ip = ${loadbalancer_public_ip}
```

现在，您可以通过各自的端口访问各种服务：

```bash
Dashboard: ${loadbalancer_public_ip}:18083
MQTT: ${loadbalancer_public_ip}:1883
MQTTS: ${loadbalancer_public_ip}:8883
WS: ${loadbalancer_public_ip}:8083
WSS: ${loadbalancer_public_ip}:8084
```

### 启用SSL/TLS
以下是启用SSL/TLS的一些配置：

```bash
# 默认单向SSL
enable_ssl_two_way = false
# 根证书的通用名称
ca_common_name = "RootCA"
# 证书的通用名称
common_name    = "Server"
# 组织名称
org = "EMQ"
# 证书的有效期（小时）
validity_period_hours = 8760
# 提前续订的时间（小时）
early_renewal_hours = 720
```

运行以下命令将CA、证书和密钥保存到文件中以供客户端连接使用：

```bash
terraform output -raw tls_ca > tls_ca.pem
terraform output -raw tls_cert > tls_cert.pem
terraform output -raw tls_key > tls_key.key
```

如果客户端需要验证服务器的证书链和主机名，您需要配置 hosts 文件：

```bash
${loadbalancer_ip} ${common_name}
```

### 清理

当您完成 EMQX 集群的使用后，可以使用以下命令销毁它：

```bash
terraform destroy -auto-approve
```

这将删除此模块在 Terraform 中创建的所有资源。







