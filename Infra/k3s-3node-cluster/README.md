##K3S cluster installation on 3 nodes

# To crete the first server on the first node
```
curl -sfL https://get.k3s.io | K3S_TOKEN=V3rryS3cr3t sh -s - server \
--cluster-init \
--tls-san=k3s-vm01.bokors.net \
--tls-san=k3s-vm01 \
--tls-san=k3s.bokors.net \
--tls-san=192.168.10.200 \
--tls-san=k3s
```

# To verify the k3s node, use the below command:
`kubectl get nodes

# To join the first node to the previously created cluster
```
curl -sfL https://get.k3s.io | K3S_TOKEN=V3rryS3cr3t sh -s - server \
--server https://192.168.10.201:6443 \
--tls-san=k3s-vm02.bokors.net \
--tls-san=k3s-vm02 \
--tls-san=192.168.10.200

```

# To join the second node to the previously created cluster
```

curl -sfL https://get.k3s.io | K3S_TOKEN=V3rryS3cr3t sh -s - server \
--server https://192.168.10.201:6443 \
--tls-san=k3s-vm03.bokors.net \
--tls-san=k3s-vm03 \
--tls-san=192.168.10.200

```
# To add this K3S cluster to an already exising Portainer (running on Docker on a different node)

First enable the docker agent on the server node with below command:
`kubectl apply -f https://downloads.portainer.io/ce2-21/portainer-agent-k8s-lb.yaml

Then simply use the wizard in portainer under the Administration -> Environment-related -> Environments -> Add environment