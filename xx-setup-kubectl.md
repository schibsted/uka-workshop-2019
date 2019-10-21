How to setup kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl/

Linux
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

MacOS (with homebrew)
brew install kubectl

Windows
Download the latest version from this link https://storage.googleapis.com/kubernetes-release/release/v1.16.0/bin/windows/amd64/kubectl.exe
Add the binary in to your PATH


When you have downloaded and installed kubectl ensure your version is up to date with:

kubectl version
