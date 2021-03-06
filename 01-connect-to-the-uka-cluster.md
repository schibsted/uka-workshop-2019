# Connect to the Kubernetes cluster
We have set up a Kubernetes cluster in advance which you will need to deploy to. The cluster is running in AWS EKS (Amazon Elastic Kubernetes Service), and you’ll get your own namespace in the cluster.

1. Create a `.kube`-directory in your home folder:
    - `mkdir ~/.kube`
1. Copy the config-file `<namespace>.config` (for instance `hovedbygget.config`) from the provided memory stick to the directory you created above. This config file contains AWS credentials and a Kubernetes certificate, both of which are needed to authenticate to the Kubernetes cluster. The value of `<namespace>` is your personal namespace for the rest of this workshop, and in the rest of the guide, you should use this name in place of the `<namespace>` placeholders.
      ### What is a namespace?
      
      Most Kubernetes clusters will have several namespaces, especially if they are shared among many applications or users.   Everything you deploy to Kubernetes must live in a namespace (if you don't specify anything, it will by default go into the namespace `default`). A namespace is a semi-isolated environment, which means that the resources inside one namespace can't directly talk to nodes in other namespaces. In this workshop, each person has got their own namespace to play around with.
1. Export an environment variable that will let `kubectl` find this config file:
    - macOS/Linux: `export KUBECONFIG=$KUBECONFIG:~/.kube/<namespace>.config`
    - Windows: `SET KUBECONFIG=%KUBECONFIG%:~/.kube/<namespace>.config`

    If you open a new shell, you'll need to run this command again.
1. Set the current context for kubectl to actually use the above config file:
    - `kubectl config use-context arn:aws:eks:eu-west-1:404451837094:cluster/ukapizza`

    The strange looking string at the end here is an Amazon Resource Name, but in theory this could be any string. The important part is that it matches the *context name* set in the config file, which in our case is `arn:aws:eks:eu-west-1:404451837094:cluster/ukapizza`.
1. Confirm that you can connect to the cluster by running `kubectl cluster-info`. This should output something like the following:
    ```
    Kubernetes master is running at https://438D4A913F08906FC8ABAD14BEE10E65.sk1.eu-west-1.eks.amazonaws.com
    ```
1. Create a configmap to see if the configuration works:
    - `kubectl create configmap getting-started --from-literal=uka=2019`

    Read the configmap back:
    - `kubectl get cm getting-started -o yaml`
    
    You should see something simalar to the following (actual values will differ). Ensure that you are seeing the literal value `uka: 2019` that you specified when creating the configmap.
    ```
    apiVersion: v1
    data:
      uka: "2019"
    kind: ConfigMap
    metadata:
      creationTimestamp: "2019-10-21T08:55:03Z"
      name: getting-started
      namespace: <namespace>
      resourceVersion: "1006802"
      selfLink: /api/v1/namespaces/finland/configmaps/getting-started
      uid: 789d86ac-f3e0-11e9-96e1-0a978df5813a
    ```

# Reference

[Overview of kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
