# Step 0: environment setup

# Step 1: install Linkerd
```sh

$ curl https://run.linkerd.io/install | sh -

```

# Step 2: curl & nginx deploy
```
$ less manifests/curl.yaml
$ less manifests/nginx-deploy.yaml

$ kubectl deploy -f manifests/curl.yaml
$ kubectl deploy -f manifests/nginx-deploy.yaml
```

# Step 3: verifying (no) mTLS!

```sh
$ kubectl exec curl -it -- bin/sh
$ kubectl exec nginx -it -c linkerd-debug -- bin/bash

/$ tshark -i any -d tcp.port==80,http # will decode packets to tcp port 80 as http
/$ tshark -s0 -i eth0 -w testcap.pcap 
    # start capture and save to file 
    # -s0 snaplen 0 => full packet is captured
/$ tshark -r testcap.pcap # show raw packet
/$ tshark -r testcap.pcap -V # show packet details
/$ tshark -r testcap.pcap -Px -Y http
   # -P: print summary of packet
   # -x: see ASCII dump (i.e contents of packet)
   # -Y http: filter only http packets
   # man tshark for more

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
