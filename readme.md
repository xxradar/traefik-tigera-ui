## Installing Traefik on docker (provider docker, file and ingress)

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

###  Setting up nginx (docker provide demo)
Create a file ./mylabel.txt
```
traefik.http.routers.whoami.rule=Host(`test.docker.localhost`)
```
```
docker run --label-file ./mylabel.txt -p 80 nginx
```

### Setting a demo k8s deployment and service
In order to point traefik (running in docker for mac) to kubernetes
```
kubectl proxy -p 9080   --address='0.0.0.0' --accept-hosts='^localhost$,^.*$' &
```

```
kubectl apply -f https://raw.githubusercontent.com/xxradar/kuberneteslearning/master/radarhack-deploy.yaml
kubectl apply -f https://raw.githubusercontent.com/xxradar/kuberneteslearning/master/radarhack-expose-clusterip.yaml
kubectl apply -f https://raw.githubusercontent.com/xxradar/kuberneteslearning/master/radarhack-expose-ingress.yaml
```

### Setting up and testing TCP routers

```
openssl s_client -servername sni.radarhack.com -showcerts -connect 127.0.0.1:9090
```
```
openssl req -x509 -newkey rsa:2048 -keyout tls.key -out tls.crt -days 365 -nodes -subj "/CN=test.radarhack.com"
openssl s_client -servername test.radarhack.com -showcerts -connect 127.0.0.1:9090
```
```
openssl req -x509 -newkey rsa:2048 -keyout tls3.key -out tls3.crt -days 365 -nodes -subj "/CN=test3.radarhack.com"
openssl s_client -servername test3.radarhack.com -showcerts -connect 127.0.0.1:9090
```
