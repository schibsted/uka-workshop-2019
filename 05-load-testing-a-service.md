# Load testing

In order to determine how well a service performs under load it is best to expose it to load in a controlled environment and observe it using monitoring tools. There are a number of load testing tools that exist for generating http traffic and today we will be using [locust](https://locust.io/).

1. Install locust master and workers.

    - `kubectl create -f manifests/locust/locust.yaml`

    This will create a new service in your namespace called `uka-locust-master-svc`.
    This will create two deployments - one for the master and another one for workers.
    There is a single master and there can be a number of workers operating concurrently. By default we have 1 master and 2 workers.

1. Update target host. The host to target is specified as an environment variable in the master deployment. In order to target your service you can edit the deployment resource and update the hostname to match.

    - `kubectl edit deployment uka-locust-master`
    Find the host and update it to match the hostname you specified in a previous step `sirup-<mynamespace>.ingress.uka.k8s.pizza`

1. Connecting to Locust. Once that is done you can try to connect to the master. It has a web interface that can be accessed on your local machine by doing port forwarding to a local port.

    - `kubectl port-forward svc/uka-locust-master-svc 8089:8089`
    Now the locust master is available by pointing your browser to `http://localhost:8089`

    You should verify that the host specified at the top of the page is correct before proceeding.

    There should be two workers available also reported at the top of the locust web interface.

1. Starting a test. Now you can start a new test by starting a new swarm that will target your specified host and endpoint. The worker configmap specifies the tasks that the workers should perform and in our case we are calling a specific endpoint of the `sirup` service (endpoint `/worker/start`). Each worker will perform the tasks that have been specified until the test is ended. It is possible to configure the frequency with which they will perform the task but we will be using the defaults here. The current workers will wait a minimum of 1 second before starting a new task.

It is good to start things off slow and gradually increase the number of concurrent users and the rate with which they are added (hatch rate). For the purpose of this simple exercise numbers between 0-100 users with a hatch rate of 1.

1. Once a test has been started it is possible to observe the pods and their resource utilization inside of the cluster

    - `kubectl top pods -lapp=sirup` (if you have `watch` installed you can prefix this with watch to have it automatically update periodically as the load test progresses)

    ```
    NAME                                 CPU(cores)   MEMORY(bytes)
    sirup-57df898bf4-2hmzf               48m           28Mi
    sirup-57df898bf4-snvbg               75m           28Mi
    ```

As the number of users increases so will the resource usage of the pods. When pods exceed their alotted resource usage they
will start to be throttled and eventually killed. This can lead to degraded performance and failed responses.

More on locust can be found here: https://locust.io/
