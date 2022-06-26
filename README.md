# My k8s & spring boot experiments
# Steps
* create a namespace(*kubectl create namespace workshop*)
* create correct Dockerfile
* build docker image
* check if it starts correctly
* create deployment manifest
* create a secret


    kubectl create secret generic --from-literal=spring.security.user.name=username --from-literal=spring.security.user.password=password --from-literal=spring.security.user.roles=ADMIN

* create volume from secrets
    
      spec:
      ...
         volumes:
            - name: workshop-secret
              secret:
                secretName: workshop-credentials-secret
      ...

* mount volume to container


    containers:
    ...
       volumeMounts:
          - mountPath: /secret/app
            name: workshop-secret
    ...

* import values from secret volume to spring boot app with 

      ...
      spring.config.import=optional:configtree:/secret/app/
      ...

* create a configmap and plug it into deployment


    containers:
    ...
      envFrom:
        configMapRef:
        name: workshop-configmap
    ...
* declare resources for container 


    containers:
    ...
      resources:
        requests:
          cpu: 100m
          memory: 128Mi
        limits:
          cpu: 600m
          memory: 512Mi
    ...
* create priority class to add more flexibility for limit range
* create limit range manifest to control your container resources
* create quota to control resources for your namespace
* create service for your pods