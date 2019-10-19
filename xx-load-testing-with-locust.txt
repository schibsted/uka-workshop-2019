Install locust master and workers.

kubectl --kubeconfig=ukapizza.config create -f manifests/locust.yaml -n<mynamespace>

This will create a new service in your namespace called `uka-locust-master-svc`.
This will create two deployments - one for the master and another one for workers.
There is a single master and there can be a number of workers operating concurrently. By default we have 1 master and 2 workers.

The host to target is specified as an environment variable in the master deployment. In order to target your service you can edit the deployment resource and update the hostname to match.

kubectl --kubeconfig=ukapizza.config edit deployment uka-locust-master -n<mynamespace>

Once that is done you can try to connect to the master. It has a web interface that can be accessed on your local machine by doing port forwarding to a local port.

kubectl --kubeconfig=ukapizza.config -n<mynamespace> port-forward svc/uka-locust-master-svc 8089:8089

Now the locust master is available by pointing your browser to http://localhost:8089

You should verify that the host specified at the top of the page is correct before proceeding.

There should be two workers available.

Now you can start a new test by starting a new swarm that will target your specified host and endpoint.

It is good to start things off slow and gradually increase the number of concurrent users and the rate with which they are added.

More on locust can be found here: https://locust.io/

It is possible to add more workers by scaling the deployment resource. For doubling the number of workers it is possible to issue the following command.

kubectl --kubeconfig=ukapizza.config scale deployment --replicas 4 uka-locust-worker -n<mynamespace>
