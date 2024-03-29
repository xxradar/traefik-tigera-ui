## Installing Traefik on docker node to expose Tigera-UI

### Create a certificate
Change the hostame and make sure it resolves to the Traefik instances. The name should be the hostname the Tigere-UI will be accessblie on.
```
mkdir certs
openssl req -x509 -newkey rsa:2048 -keyout ./certs/tls.key -out ./certs/tls.crt -days 365 -nodes -subj "/CN=test3.radarhack.com"
```

### Adjust the traefik file provider details
1. Change the hostname for the Tigera UI in `./conf/tigera-ui.yaml`
2. Make sure the file specifies the correct certificates in `./conf/tigera-ui.yaml`
3. Make sure  `- url: https://10.101.71.188:30771` in the `./conf/tigera-ui.yaml` is the correct `url: https://Node:NodePort` of the kubernetes service. <br>
   Also verify that this NodePort is reachable from the traefik node.


### Starting traefik
```
docker run -d -p 8080:8080 -p 8000:80 -p 8443:443 -p 9090:9090 \
        -v $PWD/traefik.yaml:/etc/traefik/traefik.yaml \
        -v $PWD/log:/log \
        -v $PWD/conf:/conf \
        -v $PWD/certs:/certspath \
        -v /var/run/docker.sock:/var/run/docker.sock \
        --name traefik traefik:v2.3 
```

This should work ...
```
curl -kv https://test3.radarhack.com:8443/
```

