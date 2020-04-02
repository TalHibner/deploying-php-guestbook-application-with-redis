**Part 1** - 

# Deploying PHP Guestbook application with Redis using AWS EKS

Build and deploy a simple, multi-tier web application using Kubernetes and Docker.

This example consists of the following components:

*  A single-instance Redis master to store guestbook entries

*  Multiple replicated Redis instances to serve reads

*  Multiple web frontend instances

The guestbook application uses Redis to store its data.
It writes its data to a Redis master instance and reads data from multiple Redis slave instances.

Although the Redis master is a single pod, you can make it highly available to meet traffic demands by adding replica Redis slaves.

configurong an application that uses K8s to manage a php guest book

Based on:

https://kubernetes.io/docs/tutorials/stateless-application/guestbook/


**Part 2** - 

# Automated deployments to Kubernetes (EKS+ELB) with GitLab

we’ll go through the steps of creating an automated deployment pipeline for Kubernetes using GitLab.

There are many ways to get one, and it does not really matter how you set one up. 
I’m personally happy with the eksctl utility which makes it really easy to set up an AWS EKS Cluster.

Please note that there is some pricing involved with spinning up this cluster. 
You pay $0.20 per hour for the Amazon EKS control plane. 
You’ll also pay $0.0228 per hour for the t3.small worker node that we spin up.

Check out the documentation on eksctl.io to install and configure the tool.
Then you can spin up the Kubernetes cluster with the following command:

eksctl create cluster --name=guestbook-app-with-redis --nodes=1 --node-type t3.small

This will take around 10 to 15 minutes. Once the cluster is created, you can set up your kubeconfig file using the AWS CLI’s update-kubeconfig command as follows:

aws eks update-kubeconfig --name guestbook-app-with-redis

Check to see if your worker node has properly registered with the following command:

kubectl get nodes

Finally we’ll create a gitlab service account that we’ll use to deploy to Kubernetes from GitLab.

Apply these settings with the following command:

kubectl apply -f gitlab-service-account.yaml

This will create a new service account and attach admin permissions to it. 
Keep in mind that in production environments you’ll definitely want to use a role with only the minimum permissions required.

