# Deploy on Ali Cloud

This Terraform module is designed to deploy either EMQX or EMQX Enterprise on Alibaba Cloud. EMQX is a scalable and open-source MQTT broker that connects IoT devices.

## Compatability

|   OS/Version | EMQX Enterprise 4.4.x | EMQX Open Source 4.4.x | EMQX Open Source 5.0.x |
|--------------|-----------------------|------------------------|------------------------|
| ubuntu 20.04 | ✓                     | ✓                      | ✓                      |


## Prerequisites

### Terraform 

This module requires Terraform to be installed. If you haven't installed it already, you can follow the [official Terraform installation guide](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/install-and-configure-terraform-on-your-computer)

### Alibaba Cloud Access Keys

In order to deploy EMQX on Alibaba Cloud, you need to provide your Alibaba Cloud access keys. You can do this by exporting the `ALICLOUD_ACCESS_KEY` and `ALICLOUD_SECRET_KEY` environment variables:

``` bash
export ALICLOUD_ACCESS_KEY=${access-key}
export ALICLOUD_SECRET_KEY=${secret-key}
```


## Deploying EMQX Cluster

### Configuring EMQX4

To deploy EMQX version 4.4.x, provide the package URL in the `emqx_package` variable. Replace `${emqx4_package_url}` with your actual URL.

```bash
emqx_package = ${emqx4_package_url}
```

### Configuring EMQX5

To deploy EMQX version 5.0.x, provide the package URL in the `emqx_package` variable. Replace `${emqx5_package_url}` with your actual URL.

```bash
emqx_package = ${emqx5_package_url}
is_emqx5 = true
emqx5_core_count = 1
emqx_instance_count = 4
```

> **Note**

> The `emq5_core_count` should be less than or equal to `emqx_instance_count`. 


### Running terraform

To apply the Terraform module, navigate to the services/emqx_cluster directory and run the following commands:

```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note**

> If you're deploying EMQX Enterprise and need more than 10 quotas, apply for an EMQX license and pass it in as a variable during the terraform apply command.


After apply successfully, it will output:
```bash
Outputs:

emqx_cluster_address = ${emqx-cluster-ip}
```

You can access different services with different ports:
```bash
Dashboard: ${emqx_cluster_address}:18083
MQTT: ${emqx_cluster_address}:1883
MQTTS: ${emqx_cluster_address}:8883
WS: ${emqx_cluster_address}:8083
WSS: ${emqx_cluster_address}:8084
```

### Cleanup

After you've finished with the EMQX cluster, you can destroy it using the following command:


```bash
terraform destroy -auto-approve
```

This will delete all resources created by Terraform in this module.

