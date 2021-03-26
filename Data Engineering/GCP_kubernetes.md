#Kubernetes for GCP

Log on to the Google Cloud Platform - http://console.cloud.google.com/

In this lab, you learn how to:
- Provision a Kubernetes cluster using Google Kubernetes Engine.
- Deploy and manage Docker containers using kubectl.
- Split an application into microservices using Kubernetes' Deployments and Services.

At the highest level, Kubernetes it is a set of APIs that you can use to deploy containers on a set of nodes. It's purpose is to automate all the manual set up and deployment processes associated with running large numbers of containerised scripts. You use Kubernetes Engine and its Kubernetes API to deploy, manage, and upgrade applications.You use an example application called "app" to complete the labs.

App is hosted on GitHub. It's a 12-Factor application with the following Docker images:

- Monolith: includes auth and hello services.
- Auth microservice: generates JWT tokens for authenticated users.
- Hello microservice: greets authenticated users.
- nginx: frontend to the auth and hello services.

## Activate Google Cloud Shell

gcloud is the command-line tool for Google Cloud Platform. It comes pre-installed on Cloud Shell and supports tab-completion.

You can list the active account name with this command: '''gcloud auth list'''

You can list the project ID with this command: '''gcloud config list project'''

Get the sample code from the Git repository: '''git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git'''

## A quick demo of Kubernetes

### Start a Kubernetes cluster

Define your zone as a project default zone: '''gcloud config set compute/zone us-central1-a'''

In Cloud Shell, run the following command to start a Kubernetes cluster called bootcamp that runs 5 nodes: '''gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"'''

After the cluster is created, check your installed version of Kubernetes using the kubectl version command: '''kubectl version'''

Use kubectl cluster-info to find out more about the cluster: '''kubectl cluster-info'''

### Run and deploy a container

Use kubectl run to launch a single instance of the nginx container: '''kubectl run nginx --image=nginx:1.10.0'''

Use the kubectl expose command to expose the nginx container outside Kubernetes: '''kubectl expose deployment nginx --port 80 --type LoadBalancer'''

Use the kubectl get pods command to view the pod running the nginx container: '''kubectl get pods'''

Use the kubectl get command to view the new service: '''kubectl get services'''

Use the kubectl scale command to scale up the number of backend applications (pods) running on your service using: '''kubectl scale deployment nginx --replicas 3'''

Clean up nginx by running the following commands: '''kubectl delete deployment nginx'''

Clean up nginx by running the following commands: '''kubectl delete service nginx'''

### Creating Pods

In this section The pod is made up of one container (called monolith). You pass a few arguments to the container when it starts up and open port 80 for HTTP traffic.

Explore the [monolith] pod's configuration file: '''cat pods/monolith.yaml'''

Create the monolith pod using kubectl create: '''kubectl create -f pods/monolith.yaml'''

When the pod is running, use the kubectl describe command to get more information about the monolith pod: '''kubectl describe pods monolith'''

### Create a deployment

Examine the deployment configuration file: '''cat deployments/auth.yaml'''

Create the deployment object using kubectl create: '''kubectl create -f deployments/auth.yaml'''

Verify that it was created: '''kubectl get deployments'''

Kubernetes creates a ReplicaSet for the deployment: '''kubectl get replicasets'''

With your pod running, it's time to put it behind a service. Use the kubectl create command to create the auth service: '''kubectl create -f services/auth.yaml'''

You can update the replicas field most easily using the kubectl scale command: '''kubectl scale deployment hello --replicas=5'''

## Rolling updates

Deployments update images to new versions through rolling updates. When a deployment is updated with a new version, it creates a new ReplicaSet and slowly increases the number of replicas in the new ReplicaSet as it decreases the replicas in the old ReplicaSet.

View the new entry in the rollout history: '''kubectl rollout history deployment/hello'''

Verify the current state of the rollout: '''kubectl rollout status deployment/hello'''

Pause the update: '''kubectl rollout pause deployment/hello'''

Use the resume command to continue the rollout: '''kubectl rollout resume deployment/hello'''

Use the rollout undo command to roll back to the previous version, then fix any bugs: '''kubectl rollout undo deployment/hello'''

Verify this with the pods: '''kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}' '''

## Canary Deployments

Run a canary deployment to test a new deployment in production with a subset of users. This mitigates risk with new releases.

### Create a Canary Deployment

A canary deployment consists of a separate deployment from your stable deployment and a service that targets them both at the same time.

Examine the file that creates a canary deployment for your new version: '''cat deployments/hello-canary.yaml'''

## Blue-Green Deployments

You can use blue-green deployments if it's more beneficial to modify load balancers to point to a new, fully-tested deployment all at once. You use two nearly-identical service files (hello-blue and hello-green) to switch between versions. The only difference between these files is their version selector. You could edit the service while it's running and change the version selector, but switching files is easier for labs.

First, update the service to use the blue deployment: '''kubectl apply -f services/hello-blue.yaml'''

Create the green deployment: '''kubectl create -f deployments/hello-green.yaml'''

Verify the blue deployment (1.0.0) is still being used: '''curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version'''

Run the following command to update the service to use the green deployment: '''kubectl apply -f services/hello-green.yaml'''

### Rollback a Blue-Green Deployment

While the green deployment is still running, simply update the service to the old (blue) deployment: '''kubectl apply -f services/hello-blue.yaml'''

## Monitoring and Health Checks

Kubernetes supports monitoring applications in the form of readiness and liveness probes. Health checks can be performed on each container in a pod. Readiness probes indicate when a pod is "ready" to serve traffic. Liveness probes indicate whether a container is "alive." If a liveness probe fails multiple times, the container is restarted. Liveness probes that continue to fail cause a pod to enter a crash loop. If a readiness check fails, the container is marked as not ready and is removed from any load balancers.

In this lab, you deploy a new pod named healthy-monolith, which is largely based on the monolith pod with the addition of readiness and liveness probes.

Explore the healthy-monolith pod configuration file: '''cat pods/healthy-monolith.yaml'''

Create the healthy-monolith pod using kubectl: '''kubectl create -f pods/healthy-monolith.yaml'''

See how Kubernetes responds to failed readiness probes. The monolith container supports the ability to force failures of its readiness and liveness probes. This enables you to simulate failures for the healthy-monolith pod.

Building on what you learned in the previous tutorial, use the kubectl port-forward and curl commands to force the monolith container liveness probe to fail. Observe how Kubernetes responds to failing liveness probes.

