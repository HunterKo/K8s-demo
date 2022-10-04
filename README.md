### pack spring boot & build image, then push image to docker hub  
```
$ cd C:\k8s-sample-code\springio-api
$ docker build -t springio-demo .

# this image tag and "image" (in k8s-app/deployment.yaml) should be identical
#
$ docker login
### docker push hunterko/k8s-app-demo:tagname
$ docker tag springio-demo hunterko/springio-demo:1.0.0
$ docker push hunterko/springio-demo:1.0.0
```

### check k8s cluster status  
```
$ kubectl cluster-info
$ kubectl get nodes
$ kubectl get services
```

### apply all service (app & mysql)  
```
$ cd ..
$ kubectl apply -f base
$ kubectl apply -f mysql
$ kubectl apply -f springio-api/k8s-app
```

### check service,pod status  
```
$ kubectl get svc,pods -n springio
```

### or using kubernetes proxy
```
# open browser: http://localhost:8080/swagger-ui.html
#
$ kubectl port-forward -n springio svc/springio-demo 8080:8080
```

========================== reset ===============================
### delete all in namespace "springio"  
```
$ kubectl delete namespaces springio
```

========================== debug ===============================
### debug pod & check pod logs  
```
$ kubectl get pods -A
$ kubectl describe pods ${POD_NAME} -n springio 
$ kubectl logs ${POD_NAME} -n springio   
```

### look inside mysql  
```
$ kubectl -n springio exec -it ${MYSQL_POD_NAME} -- mysql -u root -p
```

### if pods stuck in terminating status  
```
$ kubectl delete pod <PODNAME> --grace-period=0 --force -n springio  
```