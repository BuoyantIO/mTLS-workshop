# Step 0: environment setup

# Step 1: install Linkerd
```sh

$ curl https://run.linkerd.io/install | sh -

```

# Step 2: curl & nginx deploy
```
$ less manifests/curl.yaml
$ less manifests/nginx-deploy.yaml

$ kubectl apply -f manifests/curl.yaml
$ kubectl apply -f manifests/nginx-deploy.yaml
```

# Step 3: verifying (no) mTLS!

```sh
$ kubectl exec curl -it -- bin/sh
$ kubectl exec nginx -it -c linkerd-debug -- bin/bash

/$ tshark -i any -d tcp.port==80,http
```

# Step 4: deploy Linkerd + certs

```sh
$ kubectl get secret -n linkerd
```

# Step 5: are TLS'd yet?

```sh
# same steps as 3
```

# Step 6: bonus content

Buoyant Cloud to verify TLS!
