# FRP on AWS and Kubernetes

## Slides

```shell
podman run -p 8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx
```

## Content

### Introduction

We have built a SaaS platform for customers to visualize their data on processing and manufacturing pipelines. We have built this 
platform on top of AWS and Kubernetes. Customer data is fairly sensitive, so customers are relucatant to open it to the internet
so our SaaS platform can access it.

### Problem

We need a networking component to get access from a VPC on AWS to a datacenter from the customer. The customer can not open incoming
ports in their network.

### Solution (1)

Pick your favorite VPN solution and install it on AWS and the customer datacenter. This seems like the easiest off the shelf solution.
The downside is that IP ranges are hard to manage and are more error prone for customers to manage.

### Solution (2)

[FRP](https://github.com/fatedier/frp) is a reverse proxy that can expose local services behind a NAT or firewall to the internet. By
using a solution like this, we can route traffic from AWS to the customer datacenter without the customer having to open any ports.
All traffic is encrypted in transit and we remain in control of certificates and autentication.

### Managing the server

[OperatorSDK](https://sdk.operatorframework.io/) is a toolkit that allows you to manage Kubernetes operators. We have built an operator
to manage the server using Kubernetes Custom Resources. By using this approach, we can manage the bulk of the configuration using tools
like ArgoCD and the operator will handle the configuration of the FRP server.