# Deploy on AWS

## Compatability

|                          | EMQX 4.4.x      |
|--------------------------|-----------------|
| ubuntu 20.04             | ✓               |

> **Note**

- Not support EMQX 5.x currently

## Install terraform
> Please refer to [terraform install doc](https://learn.hashicorp.com/tutorials/terraform/install-cli)


## AWS AccessKey Pair
```bash
export AWS_ACCESS_KEY_ID="anaccesskey"
export AWS_SECRET_ACCESS_KEY="asecretkey"
```

## Deploy EMQX single node
```bash
cd services/emqx
terraform init
terraform plan
terraform apply -auto-approve
```


## Deploy EMQX cluster
```bash
cd services/emqx_cluster
terraform init
terraform plan
terraform apply -auto-approve
```

> **Note**

> You should apply for an emqx license if you want more than 10 quotas when deploying emqx enterprise.

After apply successfully, it will output:
```bash
Outputs:

emqx_cluster_address = "${prefix}.elb.${region}.amazonaws.com"
```

If you want to associate address with domain name, you need to config CNAME.

You can access different services with different ports
```bash
Dashboard: ${prefix}.elb.${region}.amazonaws.com:18083
MQTT: ${prefix}.elb.${region}.amazonaws.com:1883
MQTTS: ${prefix}.elb.${region}.amazonaws.com:8883
WS: ${prefix}.elb.${region}.amazonaws.com:8083
WSS: ${prefix}.elb.${region}.amazonaws.com:8084
```

## Destroy
```bash
terraform destroy -auto-approve
```



