# Kubernetes for AWS

Introduction to key concepts

https://www.eksworkshop.com/010_introduction/


Kudenetes - container orcestration / group together servers into a pool of resources for running applications on
- Open source container management platform 
- Helps you run containers at scale
- provides objects and apis for building modern applications

Takes and given end state and is capable of figuring out by itself how to get their

EKS - Elastic Kubenetes Service (from Amazon)

Orchestration - managing dozens of applications can become unweildy. Orchestrations allows for:
- better packaging (Docker)
- easier installation
- easier movement of applicantions (efficency gains)

Kubernetes is formed in a cluster of nodes or (EC2) instances (physical or virtual). There are two types:
-  A Control-plane-node type, which makes up the Control Plane, acts as the “brains” of the cluster.
- A Worker-node type, which makes up the Data Plane, runs the actual container images (via pods).

Kubenetes objects are a 'record of intent' that deffine the clusters desired state. This state includes:
- Pod - a wrapper around one or more containers
- Daemon set - implements a single instance of a pod on a worker node
- Deployment - how to roll out version of your applications
- Replica set - ensures a deffined number of pods are running
- Job - ensures a pod runs to completion
- Service - maps an ip address to a group of pods (how back end talks to front end)

App --> Container --> Pod --> Docker --> Worker --> Control plane --> Cluster

Control plane and workers communicate via an API service to the 'kublet'

### EKS Cluster creation workflow

1. Create EKS cluster (single line of code)
- Create HA Control Plane
- IAM Integration
- Certification Managemnt
- Setup LB
2. Provision worker nodes
3. Launch add-ons
4. launch workloads
 
# Launch EKSCTL

https://www.eksworkshop.com/030_eksctl/

Alot of kubenetes can be set up on Cloud 9 that can be set up and accessed through AWS.

1. Download and install Eksctl - https://eksctl.io/

'''
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv -v /tmp/eksctl /usr/local/bin

eksctl version

eksctl completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
'''

2. Create an EKS cluster

Run '''aws sts get-caller-identity''' and validate that your Arn contains '''eksworkshop-adminand''' an Instance Id....

{
    "Account": "123456789012",
    "UserId": "AROA1SAMPLEAWSIAMROLE:i-01234567890abcdef",
    "Arn": "arn:aws:sts::123456789012:assumed-role/eksworkshop-admin/i-01234567890abcdef"
}

Create an eksctl deployment file (eksworkshop.yaml) use in creating your cluster using the following syntax:

'''
cat << EOF > eksworkshop.yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: eksworkshop-eksctl
  region: ${AWS_REGION}
  version: "1.17"

availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]

managedNodeGroups:
- name: nodegroup
  desiredCapacity: 3
  instanceType: t3.small
  ssh:
    allow: true
    publicKeyName: eksworkshop

# To enable all of the control plane logs, uncomment below:
# cloudWatch:
#  clusterLogging:
#    enableTypes: ["*"]

secretsEncryption:
  keyARN: ${MASTER_ARN}
EOF
'''

Next, use the file you created as the input for the eksctl cluster creation (this can take up to 15mins).

'''eksctl create cluster -f eksworkshop.yaml'''

3. Test the cluster

Confirm you nodes
'''
kubectl get nodes # if we see our 3 nodes, we know we have authenticated correctly
'''

Export worker names
'''
STACK_NAME=$(eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName')
ROLE_NAME=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME | jq -r '.StackResources[] | select(.ResourceType=="AWS::IAM::Role") | .PhysicalResourceId')
echo "export ROLE_NAME=${ROLE_NAME}" | tee -a ~/.bash_profile
'''

Try deploying the Kubernetes dashboard...

'''
export DASHBOARD_VERSION="v2.0.0"

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/${DASHBOARD_VERSION}/aio/deploy/recommended.yaml
'''

you'll need to start a proxy by running, '''kubectl proxy --port=8080 --address=0.0.0.0 --disable-filter=true &'''

In your Cloud9 environment, click Tools / Preview / Preview Running Application
Scroll to the end of the URL and append: '''/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/'''

Open a New Terminal Tab and enter 
'''
aws eks get-token --cluster-name eksworkshop-eksctl | jq -r '.status.token' 
'''

Copy the output of this command and then click the radio button next to Token then in the text field below paste the output from the last command. Sign in. 

...

After you are done, clean up with ...

'''
# kill proxy
pkill -f 'kubectl proxy --port=8080'

# delete dashboard
kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/${DASHBOARD_VERSION}/aio/deploy/recommended.yaml

unset DASHBOARD_VERSION
'''