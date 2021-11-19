# Dennis' K8S playbook

This playbook deploys a number of things in a Kubernetes cluster.

Infrastructure: DNS + Certificates
- [External DNS](https://github.com/kubernetes-sigs/external-dns)
- [Cert-Manager](https://github.com/jetstack/cert-manager)

Software
- [Keycloak](https://github.com/codecentric/helm-charts/tree/master/charts/keycloak)
- [Binderhub](https://github.com/jupyterhub/binderhub)
- [Apache Hadoop](https://hadoop.apache.org/) with [Apache Knox](https://knox.apache.org/)
- [Minio](https://min.io/)

## Run the Ansible playbook directly

### Configuration 

Create a YAML file (e.g., `your-settings.yaml`) to select which software to instal (and override default configuration). See `roles/*/defaults/main.yaml` for the individual parameters.

Example:

```yaml
domain: "bla.example.com"

# external dns + cert-manager (ip or hostname)
rfc2136_nameserver: "192.168.1.2"

# external dns
external_dns_enable: True
rfc2136_tsigSecret: "sdfkjasdasdfasdfsdaf...sadfsadfdss=="
rfc2136_tsigKeyname: "{{ domain }}"
rfc2136_tsigAlg: "hmac-sha512"

# cert-manager
cert_manager_enable: True
letsencrypt_email: "xx.yyr@example.com"

# binderhub + jupyterhub
binder_enable: True
```

### Create

```bash
VARS="your-settings.yaml" ; ansible-playbook --extra-vars "@$VARS" create.yaml
```

Run some elements only (e.g., only keycloak): `VARS="your-settings.yaml" ; ansible-playbook --extra-vars "@$VARS" --tags keycloak create.yaml`

### Destroy

```bash
VARS="your-settings.yaml" ; ansible-playbook --extra-vars "@$VARS" destroy.yaml
```

## Requirements

Ansible and Python packages `kubernetes` and `openshift` are required. Run `pip3 install kubernetes openshift`.

## Open Issues / Todos

- Use ClusterIssuer instead of namespaced Issuer
- Destroy does not check whether a component should have been installed in the first place (e.g., it tries to delete nginx ingress despite the fact that is shouldn't have been installed)
- Binder Tokens should be randomly generated
- Add option to configure a github access token in `files/binderhub.yaml`

```
GitHubRepoProvider:
		access_token: dsdfdsafasdfsdaffasdfassadfasdf7sdf
```

## Using TCP/UDP services with Nginx Ingress

Add a UDP/TCP service using the NGINX ingress controller (cf. [Ingress nginx for TCP and UDP services](https://minikube.sigs.k8s.io/docs/tutorials/nginx_tcp_udp_ingress/))

```yaml
spec:
  template:
    spec:
      containers:
      - name: ingress-nginx-controller
        ports:
         - containerPort: 53
           hostPort: 53
```

Run 
```bash
kubectl -n kube-system patch configmap nginx-udp-configmap  --patch '{"data":{"53":"default/my-service:53"}}'

kubectl -n kube-system patch deployment nginx-ingress-controller --patch "$(cat ingress-nginx-controller-patch.yaml)"
```

## Some notes

- Ansible: [Using Variables - Variable precedence: Where should I put a variable?](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)
