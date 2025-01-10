# This guide will help you to deploy a 3 node K3S cluster, with ArgoCD and Cert-Manager on the top of it, using Let's encrypt certificates if you have a valid domain managed by Cloudflare

This guide will deploy the below architecture, an HA 3 node k3s cluster with embeded db, using etcd. You will require a front load-balancer to monitor the node's healt, internal traffic will be load-balanced between different pods by the built-in traefik load-balancer in k3s:

![image](https://github.com/user-attachments/assets/b9411741-d036-4d24-a2e4-a4b940d0aefe)

