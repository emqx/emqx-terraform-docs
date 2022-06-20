# Deploy on AWS

> Default Region：us-east-1

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
terraform apply -auto-approve -var="ee_lic=${ee_lic}" -var="region=${region}"
```
Note: You have to apply a emqx license if you deploy emqx enterprise.

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

**Note:** Due to ubuntu 20.04 of node installed, you need to config emqx package associate with corresponding os version.



