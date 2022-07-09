# K8S

Utilities and examples for kubernetes

## Production deployments stragies 

### Canary Releases

Canary Release is a production deployment strategy to ensure new releases are robust and stable.

Canary Release redirects a percentage of the traffic from one pod to another so that possible bugs in the new version have a very limited impact.

The traffic will increase until reaching 100% and the new version will replace the current one.

During the increment of the canary, errors will be checked, if they exist, and the % of the canary will be increased accordingly.

![Canary](./resources/canary.png)

Create sample app1:

````
$ kubectl apply -f ./canary/app1.yaml
````

Create sample app2:
````
$ kubectl apply -f ./canary/app2.yaml
````

Create ingress from app1:
````
$ kubectl apply -f ./canary/app1-ingress.yaml
````

Editing ./canary/app2-ingress.yaml, and modify 'nginx.ingress.kubernetes.io/canary-weight' to update canary percentage
````
metadata:
  annotations:
    nginx.ingress.kubernetes.io/canary: "true"
    nginx.ingress.kubernetes.io/canary-weight: "25"
    kubernetes.io/ingress.class: public
  name: nginx-ingress-canary
  namespace: default
````

Create/update ingress from app2:
````
$ kubectl apply -f ./canary/app2-ingress.yaml
````

You can apply this file n times to update canary to your percentage.
