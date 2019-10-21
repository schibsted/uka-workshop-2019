# Scaling a service

Describe sirup appen

1. Create a deployment
As before you can deploy the deployment manifest using `kubectl`:
    - `kubectl create -f manifests/sirup/sirup.yaml`

    This will create a deployment resource and spawn a single pod. Let's check what the deployment looks like:

    - `kubectl get deployments`
      ```
      NAME                READY     UP-TO-DATE   AVAILABLE   AGE
      sirup               1/1       1            1           61s
      ```
    There is now a single replica running. You can inspect the deployment using describe:
    - `kubectl describe deployment sirup`
    
    You can similarly inspect the pod using describe:
    - `kubectl describe pod -lapp=sirup`

1. Create a service
Create the service manifest using `kubectl`:
    - `kubectl create -f manifests/sirup/sirup-service.yaml`

1. Create ingress
Before proceeding edit the `manifests/sirup/sirup-ingress.yaml` file and change the hostname to match your assigned namespace.  (change sirup-CHANGEME.ingress.uka.k8s.pizza to sirup-<mynamespace>.ingress.uka.k8s.pizza without the <>).
Now create the ingress manifest using `kubectl`:
    - `kubectl create -f manifests/sirup/sirup-ingress.yaml`
    Now the service is accessible from the internet via `https://sirup-<mynamespace>.ingress.uka.k8s.pizza`.

1. Getting running pods
    - `kubectl get pod -lapp=sirup`

1. Killing one of the pods will spawn a new one
    - `kubectl delete pod -lapp=sirup`

1. In order to be receive a number of requests simultaneously it is possible to add more replicas
    - `kubectl scale deployment --replicas 2 sirup`
    - `kubectl scale get deployment sirup`
    ```
    NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    hello-world   2         1         0            1           1d
    ```
    - `kubectl scale get deployment sirup`
    ```
    NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    hello-world   2         2         0            2           1d
    ```
    - `kubectl scale get deployment sirup -w` (it is possible to `watch` the deployment change (abort with ctrl-c))

    Now there are two replicas in place to handle all traffic coming in through the `sirup` service.
