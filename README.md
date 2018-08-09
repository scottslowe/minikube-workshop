# Hands-on Minikube Workshop

This workshop is aimed toward folks who are new to Kubernetes and are looking a gentle, hands-on introduction to basic Kubernetes concepts.

## Prerequisites

No prior Kubernetes experience is necessary, but you will need a few tools installed on whatever computer you'd like to use:

1. Minikube **INSERT LINK** (tested with 0.28.2, which uses Kubernetes 1.10.0)
2. A virtualization provider to use with Minikube, like VirtualBox **INSERT LINK**
3. Version 1.10.0 (or newer) of the `kubectl` command-line utility **INSERT LINK**

You'll need Internet access in order to complete this tutorial.

## Part 1: Pods, Deployments, and Services "the Easy Way"

In this first part, you'll use `kubectl` to create Kubernetes pods, deployments, and services without getting into any of the details around these concepts.

1. Use `kubectl run ...` to launch an Nginx pod. This may take a few minutes as it must pull the Nginx container image down from the Internet.

2. Use `kubectl expose ...` to make this Nginx pod accessible outside the cluster.

3. Use `kubectl get pod,deployment,service` to see the objects created by the previous two steps. You should see a single deployment, a single pod, and a single service.

4. Run `kubectl describe deployment <deployment-name>` to get more details on the deployment created by step 1.

5. Run `kubectl describe pod <pod-name>` to get more details on the pod created by step 1.

6. Run `kubectl describe service <service-name>` to get more details on the pod created by step 2.

7. Delete the service and the deployment using `kubectl delete service <service-name>` and `kubectl delete deployment <deployment-name>`, respectively. Use `kubectl get pods` to verify that deleting the deployment also deleted the pod.

## Part 2: Pods, Deployments, and Services "the Hard Way"

In this second part, you'll examine more details on pods, deployments, and services, and work with the YAML files that define these objects in Kubernetes.

1. Examine the `01-pod.yaml` file in this tutorial. If you have any questions about any of the values supplied, use `kubectl explain pod` to get more information.

2. Create the pod by running `kubectl apply -f 01-pod.yaml`. Use `kubectl get pods` to verify that the pod was created.

3. Use `kubectl get deployments,services` to verify that _only_ a pod was created.

4. Examine the `02-deployment.yaml` file in this tutorial. Remember that you can use `kubectl explain deployment` to get more details on any of the values in this file.

5. Create the deployment by running `kubectl apply -f 02-deployment.yaml`.

6. Use `kubectl get pods,deployments,services` to see that a deployment was created, which in turn created a pod.

7. Use `kubectl get deployments,replicasets,pods` to see that a replica set was created automatically when you created the deployment.

8. Use `kubectl describe` to get more details on each of the objects returned by step 7. Note the relationships between the objects (a deployment manages a replica set, which in turn manages a group of pods).

9. Take a look at the `03-service.yaml` file in this tutorial. Use `kubectl explain service` to get more details on anything you don't understand.

10. Create the service using `kubectl apply -f 03-service.yaml`. After it has been created (you can verify with `kubectl get services`), then get more details on the service using `kubectl describe service <service-name>`.

11. Remove all the objects using `kubectl delete`.

That's it!

[link-1]: 
[link-2]: 
[link-3]: 
[link-4]: 
