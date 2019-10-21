# Prerequisites
1. Bring your computer (and if necessary, a charger)
1. If you run into any trouble when installing the tools below, **don't hesitate to let us know** and we'll help you as best we can
1. Install **kubectl for Kubernetes**:
    
    `kubectl` is the Kubernetes command-line tool, that allows you to run commands against Kubernetes clusters.
    - macOS: `brew install kubectl` (install [Homebrew](https://brew.sh) first if needed)
    - Linux:
      ```
      curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
      
      chmod +x ./kubectl
      
      sudo mv ./kubectl /usr/local/bin/kubectl
      ```
    - Windows: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows

    Confirm that it is correctly installed with `kubectl version`

1. Install the **AWS CLI** tool:
    - (Windows only) Install [Python](https://www.python.org/downloads/windows/) if you don't already have it. Go for one of the 3.X.X versions. Mac and Linux users should have this by default (albeit a somewhat old version, probably)
    - Install the aws-cli:
      - macOS: `brew install awscli`
      - Linux/Windows (and macOS if you have trouble with the homebrew version): https://docs.aws.amazon.com/cli/latest/userguide/install-virtualenv.html