# Hello World

Let's deploy a `hello-world` app to get familiar with `kubectl`.
We have done the very hard work of creating a super complicated web application that will print out the phrase *hello world!*, and Dockerized it for you, so that it is ready to be run in Kubernetes. Your task is to deploy it.

1. Start by checking out this repo to somewhere on your computer. In the example below, we're using `~/dev/uka-workshop-2019`.
    ```
    cd ~/dev
    git clone https://github.com/schibsted/uka-workshop-2019.git
    cd uka-workshop-2019
    ```

1. Look in the `manifests/hello-world` directory you just checked out. Here you'll find a file called `hello-world.yaml`. We encourage you to open it and have a look around. When you're ready, you can deploy the deployment manifest using `kubectl`:
    - `kubectl create -f manifests/hello-world/hello-world.yaml`

    This will create a deployment resource and spawn a single pod. Let's check what the deployment looks like:

    - `kubectl get deployments`
      ```
      NAME                READY     UP-TO-DATE   AVAILABLE   AGE
      hello-world         1/1       1            1           61s
      ```
    There is a single replica running. You can inspect the deployment using describe:
    - `kubectl describe deployment hello-world`
    
    You can similarly inspect the pod using describe:
    - `kubectl describe pod -lapp=hello-world`

    If everything went well, you now have an app running in a Docker container in a Kubernetes cluster in a datacenter in one of Amazons datacenters in Dublin (the `eu-west-1` region).

1. Let's try to see if we can interact with one of the pod endpoints:
    - `kubectl get pods`
      ```
      NAME                                 READY     STATUS    RESTARTS   AGE
      hello-world-56d58679c8-d9l6h         1/1       Running   0          11m
      ```

    Set up port forwarding so that the pod can be accessed via a local port on your computer - in this case port 8000:

    - `kubectl port-forward hello-world-56d58679c8-d9l6h 8000:8000` (replace the pod name with the actual pod name found above)

    Use curl (`curl http://localhost:8000/`) or point your browser to http://localhost:8000/. You should now see a simple `hello world` response.

1. It is possible to attach to a container within a pod and inspect it from the inside. Sometimes this can be useful for debugging.

    - `kubectl exec -it hello-world-56d58679c8-d9l6h ash` (again, replace the pod name)

    This will spawn a shell into the running container within the pod. From here we can try to do the same
    - `/app # curl http://localhost:8000/`
      ```
      hello world!
      ```
    Feel free to have a look around the container some more. To detach from the pod simply exit the shell with `exit`

1. It is possible to delete the deployment and remove the running pod with:
    - `kubectl delete deployment -lapp=hello-world`

    This will remove the deployment object, as well as the running pod.
