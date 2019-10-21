# Hello Internet!

For the hello-world app to be available to the internet, we first need to define a [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/). This service resource will specify what ports and over which protocol(s) we want to expose our app.

```
kubectl create -f manifests/hello-world/hello-world-service.yaml
```

Again, we encourage you to open the manifest and have a look. This manifest creates a service resource that exposes port 80 but uses port 8000 as the target port. The service uses a label selector to specify which pods it uses.

Inside of the cluster there is a ingress controller that is exposed to the internet. By creating ingress resources it is possible
to have traffic from an external load balancer reach the pod via the service. In order to do so the ingress manifest will need to be
added. Make sure to update and specify a unique hostname in the `manifests/hello-world/hello-world-ingress.yaml` manifest before proceeding.
Once you have specified your unique hostname such as `helloworld-<namespace>.ingress.uka.k8s.pizza` (by editing the file) you can apply it in your namespace.

```
kubectl create -f manifests/hello-world/hello-world-ingress.yaml
```

Now the service can be accessed through the external loadbalancer directly from the internet.

You can use curl or use your browser to go to your selected hostname `https://helloworld-<namespace>.ingress.uka.k8s.pizza/`
