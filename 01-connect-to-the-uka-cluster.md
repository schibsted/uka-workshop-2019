# Prerequisites
1. Bring your computer (and if necessary, a charger)
1. Install **kubectl for Kubernetes**:
    
    `kubectl` is the Kubernetes command-line tool, that allows you to run commands against Kubernetes clusters.
    - macOS: `brew install kubectl` (install [Homebrew](https://brew.sh) first if needed)
    - Other OS: https://kubernetes.io/docs/tasks/tools/install-kubectl

    Confirm that it is installed with `kubectl version`
1. Install **aws-iam-authenticator**:
    - macOS: `brew install aws-iam-authenticator`
    - Other OS: https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

    Confirm that it is installed with `aws-iam-authenticator help`

# Connect to the Kubernetes cluster
We have set up a Kubernetes cluster in advance which you will need to deploy to. The cluster is running in AWS EKS (Amazon Elastic Kubernetes Service), and you’ll get your own namespace in the cluster.

1. Create a `.kube`-directory in your home folder:
    - `mkdir ~/.kube`
1. Copy the config-file `ukapizza.config` from the provided memory stick to the directory you created above
1. Export an environment variable that will let `kubectl` find this config file:
    - macOS/Linux: `export KUBECONFIG=$KUBECONFIG:~/.kube/ukapizza.config`
    - Windows: `SET KUBECONFIG=%KUBECONFIG%:~/.kube/ukapizza.config`
1. Set the current context for kubectl to actually use the above config file:
    - `kubectl config use-context arn:aws:eks:eu-west-1:404451837094:cluster/ukapizza`

    The strange looking string at the end here is an Amazon Resource Name, but in theory this could be any string. The important part is that it matches the *context name* set in the config file, which in our case is `arn:aws:eks:eu-west-1:404451837094:cluster/ukapizza`.
1. Most Kubernetes clusters will have several namespaces, especially if they are shared among many applications or users. Everything you deploy to Kubernetes must live in a namespace (if you don't specify anything, it will by default go into the namespace `default`). A namespace is a semi-isolated environment, which means that the resources inside one namespace can't directly talk to nodes in other namespaces. In this workshop, each person has got their own namespace to play around with. To avoid having to specify the namespace in every command you run, you can use the following command to make it default:
    - `kubectl config set-context --current --namespace=<namespace>`
1. Confirm that you can connect to the cluster by running `kubectl cluster-info`. This should output something like the following:
    ```
    Kubernetes master is running at https://438D4A913F08906FC8ABAD14BEE10E65.sk1.eu-west-1.eks.amazonaws.com
    ```
1. Create a configmap to see if the configuration works:
    - `kubectl create configmap getting-started --from-literal=uka=2019`

    Read the configmap back:
    - `kubectl get cm getting-started -oyaml`
    
    You should see something simalar to the following (actual values will differ). Ensure that you are seeing the literal value `uka: 2019` that you specified when creating the configmap.
