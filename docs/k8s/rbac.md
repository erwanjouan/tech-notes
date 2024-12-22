# Understanding Kubernetes RBAC | Access control basics explained

![type:video](https://www.youtube.com/embed/jvhKOAyD8S8)

- [GitHub Repo for the video](https://github.com/marcel-dempers/docker-development-youtube-series/tree/master/kubernetes/rbac)

## What is ?

- Role based access control
- Roles allows to categorize users based on their needs
  - Dev
    - Readers
  - Marketing
    - Readers
  - Admin
    - Readers
    - Writers

### Concepts

- Users
- Groups
- Roles
  - Defines the permission of users or groups
  - Can be cluster-wide
- RoleBinding
  - Can be cluster-wide


- Explore [kubeconfig file sample](rbac/kubeconfig.yml)
    - components the defines user access

### User Access

- To identify a user or a group, Kubernetes uses a certificate.
  - K8s only trusts certificates that were signed with Kubernetes CA
  - When provisioning K8S Cluster, you provide a CA
  - Cannot login with User Bob, but generate a certificate with CN=Bob
  - Create a Role to allow Bob some permission Set
  - Bind that Role to the certificate

### Kubernetes CA

- CA allows to sign user certificates in order to trust them
- CA lives inside ```/etc/kubernetes/pki``` of control plane node
  - ```ca.crt```
  - ```ca.key```

- Connect to control plane
```shell
vagrant up
vagrant ssh controlplane
```

- Create the client key
```shell
sudo openssl genrsa -out client.key 2048
```

- Create the certificate signing request (CSR) 
```shell
sudo openssl req -new \
  -key client.key \
  -out client.csr \
  -subj "/CN=Client/O=Devops"
```

- Use the CA to generate our certificate by signing our CSR.

```shell
sudo openssl x509 -req \
  -in client.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out client.crt \
  -days 365
```

- Copy ca.crt from control plane to client machine. Generate an entry in kubeconfig
```shell
kubectl config set-cluster fedora.local --server=https://192.168.1.3:6443 \
  --certificate-authority=ca.crt \
  --embed-certs=true
```

- Create client creds
```shell
kubectl config set-credentials client \
  --client-certificate=client.crt \
  --client-key=client.key \
  --embed-certs=true
```

- Create context
```shell
kubectl config set-context fedora.local \
  --cluster=fedora.local \
  --namespace=devops \
  --user=client
kubectl config use-context fedora.local
```


