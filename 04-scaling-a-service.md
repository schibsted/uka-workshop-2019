# Scaling a service

In real life Enterprise Applications™ you can't just run one instance of your application. We need the app to scale, because you have millions of customers that want to use your awesome services. And you probably want to have downtime-free deployments, graceful failures, and so on.

We have developed another great app for this workshop, called Sirup™. Some (bad) people would maybe call Sirup™ a rather useless and badly written app, but don't listen to them. Sirup™ is great. The Norwegian word *sirup* also means *syrup* in English, and as we all know, liquid sugar is awesome.

What it does? It eats CPU cycles and starts to throw error messages at you if it gets to much to do.

### How do I deploy this great app, you might ask? Just follow these easy steps:

1. Create a deployment
    
    As before you can deploy the deployment manifest using `kubectl`:
    - `kubectl create -f manifests/sirup/sirup.yml`

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
    - `kubectl create -f manifests/sirup/sirup-service.yml`

1. Create ingress
    
    Before proceeding edit the `manifests/sirup/sirup-ingress.yml` file and change the hostname to match your assigned namespace.  (change `sirup-CHANGEME.ingress.uka.k8s.pizza` to `sirup-<my-namespace>.ingress.uka.k8s.pizza` without the <>).
    Now create the ingress manifest using `kubectl`:
    - `kubectl create -f manifests/sirup/sirup-ingress.yml`
    
    Now the service is accessible from the internet via `https://sirup-<mynamespace>.ingress.uka.k8s.pizza`.

    - The app has a simple API:
      - Spawn a new job: `POST /worker/start`
      - Health check (if the app is running): `GET /actuator/health`
      - Readyness check (if the app will accept more work): `GET /worker/healthy`
    - Sirup™ has a few flaws. It is very CPU intensitive, and if the load gets high enough, performance will start to suffer.

1. Getting running pods
    - `kubectl get pod -lapp=sirup`

1. Killing one of the pods will spawn a new one
    - `kubectl delete pod -lapp=sirup`
    ```
    NAME                     READY     STATUS    RESTARTS   AGE
    sirup-67bd8f765f-2jdt9   1/1       Running   0          72s
    sirup-67bd8f765f-zfbcz   1/1       Running   0          72s
    ```
    - `kubectl get pod -lapp=sirup -w` (Use `watch` flag to see continuous updates of pods (abort with ctrl-c))
    ```
    NAME                     READY     STATUS              RESTARTS   AGE
    sirup-67bd8f765f-2jdt9   1/1       Terminating         0          1m
    sirup-67bd8f765f-bnpvr   0/1       Running             0          4s
    sirup-67bd8f765f-cxqjk   0/1       Running             0          12s
    sirup-67bd8f765f-kfdmd   0/1       Terminating         0          34s
    sirup-67bd8f765f-qkpm8   0/1       ContainerCreating   0          4s
    sirup-67bd8f765f-tkdjs   0/1       Running             0          12s
    sirup-67bd8f765f-xwqgb   0/1       Running             0          12s
    sirup-67bd8f765f-zfbcz   1/1       Terminating         0          1m
    sirup-67bd8f765f-kfdmd   0/1       Terminating   0         34s
    sirup-67bd8f765f-kfdmd   0/1       Terminating   0         34s
    sirup-67bd8f765f-qkpm8   0/1       Running   0         4s
    sirup-67bd8f765f-2jdt9   0/1       Terminating   0         1m
    sirup-67bd8f765f-2jdt9   0/1       Terminating   0         1m
    sirup-67bd8f765f-w6sc2   1/1       Running   0         20s
    sirup-67bd8f765f-hmkxx   1/1       Running   0         20s
    sirup-67bd8f765f-xwqgb   1/1       Running   0         20s
    sirup-67bd8f765f-tkdjs   1/1       Running   0         22s
    sirup-67bd8f765f-cxqjk   1/1       Running   0         25s
    ```

1. In order to be able to distribute the load when receiving a number of requests simultaneously it is possible to add more replicas
    - `kubectl scale deployment --replicas 4 sirup`
    - `kubectl scale get deployment sirup`
    ```
    NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    hello-world   4         1         0            1           1d
    ```
    - `kubectl scale get deployment sirup`
    ```
    NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    hello-world   4         4         0            4           1d
    ```
    - `kubectl scale get deployment sirup -w` (it is possible to `watch` the deployment change (abort with ctrl-c))

    Now there are three more replicas in place to handle all traffic coming in through the `sirup` service.

    Should we want to avoid processing any requests for the given service it is possible to scale the deployment all the way down to 0 which will kill all pods. 

    We did this scaling manually for now but essentially want the scaling to happen automatically as traffic to the service increases.
