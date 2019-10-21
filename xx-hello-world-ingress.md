We can expose the hello-world app as follows

First we need to create a service resource that will specify how we expose our pods

kubectl create -f manifests/hello-world-service.yaml -n<mynamespace>

This creates a service resource that exposes port 80 but uses port 8000 as the target port. The service uses a label selector
specify which pods it uses.

Inside of the cluster there is a ingress controller that is exposed to the internet. By creating ingress resources it is possible
to have traffic from an external load balancer reach the pod via the service. In order to do so the ingress manifest will need to be
added. Make sure to update and specify a unique hostname in the `manifests/hello-world-ingress.yaml` manifest before proceeding.
Once you have specified your unique hostname such as `helloworld.ingress.uka.k8s.pizza` (by editing the file) you can apply it in your namespace.

kubectl create -f manifests/hello-world-ingress.yaml -n<mynamespace>

Now the service can be accessed through the external loadbalancer directly from the internet.

You can use curl or use your browser to go to your selected hostname https://helloworld.ingress.uka.k8s.pizza/
