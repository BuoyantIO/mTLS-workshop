## Minikube

* `minikube start`

A service bound to all networks on the host, as you configured Vault, is
addressable by pods in Minikube's cluster by sending requests to the gateway
address of the Kubernetes cluster.

```sh
$ minikube ssh
ping host.minikube.internal
PING host.minikube.internal (192.168.49.1) 56(84) bytes of data.
# our IP is 192.168.49.1
```

Install cert-manager

## Install Vault

1. Follow instructions from [vault's official docs][vault-install-docs] on how
   to install `Vault` on your target platform.

2. Once `Vault` is installed, we can run the server in the background or in a separate shell session:

```sh
$ vault server -dev -dev-root-token-id root -dev-listen-address 0.0.0.0:8200

#
# Either export VAULT_ADDR=http://0.0.0.0:8200
# or type it before the commands, like in the example
#

# Login as root
$ VAULT_ADDR=http://0.0.0.0:8200 vault login root

# Enable vault pki engine
$ VAULT_ADDR=http://0.0.0.0:8200 vault secrets enable pki

# Test vault connectivity
$ minikube ssh
$ curl -s http://192.168.49.1:8200/v1/sys/seal-status`
# where 192.168.49.1:8200 is the IP address we previously identified
```

3. In order for `cert-manager` to use `Vault`, we will need to create a set of
   roles and a token that `cert-manager` will use to authenticate the requests.


```sh
#
# Create Vault policies
#

# First, enable the secrets engine if you have not done so already
$ vault secrets enable pki

# Tune certs to satisfy 90 days request
$ vault secrets tune -max-lease-ttl=8760h pki

# Generate a root CA keypair
$ vault write pki/root/generate/internal common_name=<name> ttl=8760h

# Configure vault endpoints, we can use the address of our vault server (127.0.0.1:8200)
$ vault write pki/config/urls \
   issuing_certificates="http://<vault_server>:8200/v1/pki/ca" \
   crl_distribution_points="http://<vault_server>:8200/v1/pki/crl"

# Create a pki role to be used by cert-manager to sign certificates
$ vault write pki/roles/cert-manager \
   allow_subdomains=true \
   allow_any_name=true \
   allow_localhost=true \
   enforce_hostnames=false \
   max_ttl=8760h \
   key_type=any \
   key_usage= \
   ext_key_usage=

# Create a vault policy
$ echo 'path "pki*" {  capabilities = ["create", "read", "update", "delete", "list", "sudo"]}' \
   |vault policy write pki_policy -

# Create a token that uses the policy we just created; this will output a token
# save the token for later
$ vault write /auth/token/create policies="pki_policy" \
   no_parent=true no_default_policy=true renewable=true \
   ttl=767h num_uses=0

# Copy token, encode it and create a k8s secret in the cert-manager namespace.
# See `token.yaml` for an example
# you can add the token value and apply the file
$ kubectl apply -f token.yaml
```




[vault-install-docs]: https://learn.hashicorp.com/tutorials/vault/getting-started-install?in=vault/getting-started
