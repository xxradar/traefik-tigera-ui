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

This should work ...
```
curl -kv https://test3.radarhack.com:8443/
```

https://test3.radarhack.com:8443/ should point to your traefik ...

