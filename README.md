# Environment setup for the workshop

## Requirements 

. A linux system (Physical or Virtual) (Ubuntu 22.04 LTS) with 8 GB RAM and 32 GB free diskspace

## Installation steps

Please note that all the following instructions must be executed as normal user who has sudo access. DO NOT run the following commands as root ( or in #(hash) prompt). ALWAYS RUN them as normal unix user). Some of the commands given below need to executed using sudo. When prompted ,please give your password in those cases. 

### Docker installation 

1. Open a shell and run the following commands. Please make sure you do not have docker installed already in your system
```sh
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
# Install 
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# Press Y when prompoted while running the above command
# Enable the service 
sudo systemctl enable docker
sudo systemctl start docker

```
2. Give access of docker to the current user

```sh
sudo usermod -aG docker `whoami`
```
3. Logout and login again
4. Verify the installation . Open an terminal and run 
```sh
docker ps
# Sample output
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```


### SDKMan installation 

1. Install sdkman ( This tool will be used to install java, maven etc)
```sh

curl -s "https://get.sdkman.io" | bash 

```
2. Logout  & Login 
3. Open a shell and verify the installation 

```sh
sdk version

#Sample output
SDKMAN!
script: 5.18.2
native: 0.4.3

```
### Install Java using SDKMan 

1. Open a shell and run the following command 
```
sdk install java 21.0.1-open
```
2. Verify the installaion 

```sh
java -version
# Sample output 
openjdk version "21.0.1" 2023-10-17
OpenJDK Runtime Environment (build 21.0.1+12-29)
OpenJDK 64-Bit Server VM (build 21.0.1+12-29, mixed mode, sharing)

```
3. Logout and login again


### Install Maven 

1. Open a shell and run the following command

```sh
sdk install maven 4.0.0-alpha-10
```

2. Verify the installation 
```sh

mvn --version 

# Sample output
Apache Maven 4.0.0-alpha-10 (89d3c0321dda868c432edf504f1884e6fd706f00)
Maven home: /home/suddutt1/.sdkman/candidates/maven/current
Java version: 21.0.1, vendor: Oracle Corporation, runtime: /home/suddutt1/.sdkman/candidates/java/21.0.1-open
Default locale: en_IN, platform encoding: UTF-8
OS name: "linux", version: "6.2.0-39-generic", arch: "amd64", family: "unix"

```
### Install Quarkus
1. Open a shell and run the following commands
```sh
# Sample output 
Installing: quarkus 3.6.4
Done installing!

Setting quarkus 3.6.4 as default.

```
2. Verify installation
```sh
quarkus --version

#Sample output
3.6.4

```

### Install VSCODE 
1. Open a shell and run the following commands
```sh
sudo apt-get install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt install apt-transport-https
sudo apt update
sudo apt install code
```
2. Verify installation 
```sh
code 
# Should open the VS code environment
```

### Install kubectl
1. Open a shell and run the following commands

```sh
# Download the binary 
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Install .  Provide the password if required
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl


```

2. Verify the installation 
```sh
kubectl version

# Sample output 
Client Version: v1.29.0
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3

```

### Install kind

1. Open a shell and run the following commands

```sh
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

2. Verify the installation by creating a cluster

```sh
kind create cluster --name test-cluster
# Above command might take some time to execute for the first time
# Sample output of the above command 
Creating cluster "test-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.27.3) 
 ✓ Preparing nodes 
 ✓ Writing configuration 
 ✓ Starting control-plane  
 ✓ Installing CNI 
 ✓ Installing StorageClass
Set kubectl context to "kind-test-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-test-cluster

Have a nice day! 

```
3. Delete the test cluster ( DO NOT MISS THIS STEP)
```sh
kind delete cluster --name test-cluster
```

