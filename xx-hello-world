Lets deploy a hello-world app.

Deploy the deployment manifest using kubectl

kubectl create -f manifests/hello-world.yaml

This will create a deployment resource and spawn a single pod

Lets check what the deployments look like

kubectl get deployments -n<mynamespace>

NAME                READY     UP-TO-DATE   AVAILABLE   AGE
hello-world         1/1       1            1           61s

There is a single replica running. 

You can inspect the deployment using describe:

kubectl describe deployment hello-world -n<mynamespace>

You can similarly inspect the pod using describe:

kubectl describe pod -lapp=hello-world

Let's try to see if we can interact with one of the pod endpoints

kubectl get pods -n<mynamespace>

NAME                                 READY     STATUS    RESTARTS   AGE
hello-world-56d58679c8-d9l6h         1/1       Running   0          11m

Set up port forwarding so that the pod can be accessed via a local port - in this case port 8000.

kubectl -n<mynamespace> port-forward hello-world-56d58679c8-d9l6h 8000:8000

Use curl `curl http://localhost:8000/' or point your browser to http://localhost:8000/

You should now see a simple `hello world` response.

It is possible to attach to a container within a pod and inspect it from the inside. Sometimes this can be useful for debugging.

kubectl exec -it hello-world-56d58679c8-d9l6h -n<mynamespace> ash

This will spawn a shell into the running container within the pod. From here we can try to do the same

/app # curl http://localhost:8000/
hello world!

Feel free to have a look around the container some more.

To detach from the pod simply exit the shell with `exit`

It is possible to delete the deployment and remove the running pod with: 

kubectl delete deployment -lapp=hello-world -n<mynamespace>

This will remove the running pod.
