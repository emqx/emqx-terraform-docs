# GCP 上部署

这个 Terraform 模块用于在 Google Cloud Platform (GCP) 上部署 EMQX 或 EMQX Enterprise。EMQX 是一个可扩展的开源 MQTT 代理，用于连接物联网设备。



## 兼容性

| 操作系统/版本   | EMQX Enterprise 4.4.x | EMQX开源版 4.4.x | EMQX开源版 5.0.x |
| --------------- | --------------------- | ---------------- | ---------------- |
| ubuntu 20.04    | ✓                     | ✓                | ✓                |


## 先决条件

### Terraform
 
此模块需要安装 Terraform。如果尚未安装，请按照官方 Terraform 安装指南进行安装。

## GCP 凭证
为了与 Google Cloud API 进行交互，您需要进行身份验证。请按照此指南开始使用 GCP 身份验证。


## 部署 EMQX 集群

### 配置 EMQX4

要部署 EMQX 4.4.x 版本，请在 `emqx4_package` 变量中提供软件包的URL。将 `${emqx4_package_url}` 替换为您的实际URL。

```bash
emqx4_package = ${emqx4_package_url}
```

### 配置 EMQX5

要部署 EMQX 5.0.x 版本，请在 `emqx5_package` 变量中提供软件包的 URL。将 `${emqx5_package_url} 替换为您的实际 URL。

```bash
emqx5_package = ${emqx5_package_url}
is_emqx5 = true
emqx5_core_count = 1
emqx_instance_count = 4
```

> **Note** 

> `emqx5_core_count` 应小于或等于 `emqx_instance_count`。

### 运行 Terraform

要应用 Terraform 模块，请导航到 services/emqx_cluster 目录并运行以下命令：

```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note** 

> 如果您正在部署 EMQX Enterprise 并且需要超过10个配额，请申请 EMQX 许可证，并在 terraform apply 命令中作为变量传递。

成功执行后，Terraform 将输出负载均衡器的 IP 以及诸如 tls_ca、tls_cert 和 tls_key 之类的敏感信息。

```bash
Outputs:

loadbalancer_ip = ${loadbalancer_ip}
tls_ca = <sensitive>
tls_cert = <sensitive>
tls_key = <sensitive>
```

使用提供的端口访问各种服务，例如：

```bash
Dashboard: ${loadbalancer_ip}:18083
MQTT: ${loadbalancer_ip}:1883
MQTTS: ${loadbalancer_ip}:8883
WS: ${loadbalancer_public_ip}:8083
WSS: ${loadbalancer_public_ip}:8084
```

WSS: ${loadbalancer_public_ip}:8084

### 启用 SSL/TLS

以下是启用 SSL/TLS 的配置：

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
# 到期前提前续订的时间（小时）
early_renewal_hours = 720
```

要将 CA、证书和密钥保存到用于客户端连接的文件中，请运行以下命令：

```bash
terraform output -raw tls_ca > tls_ca.pem
terraform output -raw tls_cert > tls_cert.pem
terraform output -raw tls_key > tls_key.key
```

如果客户端需要验证服务器的证书链和主机名，您需要配置主机文件（hosts file）：

```bash
${loadbalancer_ip} ${common_name}
```

### 清理

完成 EMQX 集群的使用后，您可以使用以下命令销毁它：

```bash
terraform destroy -auto-approve
```

这将删除 Terraform 在此模块中创建的所有资源。

