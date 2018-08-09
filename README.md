# Hands-on Minikube Workshop

This workshop is aimed toward folks who are new to Kubernetes and are looking a gentle, hands-on introduction to basic Kubernetes concepts.

## Prerequisites

No prior Kubernetes experience is necessary, but you will need a few tools installed on whatever computer you'd like to use:

1. Minikube, available from [this GitHub repository][link-1] (tested with version 0.28.2, which uses Kubernetes 1.10.0)
2. A virtualization provider to use with Minikube, like [VirtualBox][link-2]
3. Version 1.10.0 (or newer) of the `kubectl` command-line utility (please see [this page][link-3] for instructions).

You'll need Internet access in order to complete this tutorial.

## Part 1: Set up a local Kubernetes cluster

Run `minikube start` to have Minikube download an ISO image and boot a VM (using your installed virtualization provider) where it will run a single-node Kubernetes cluster.

Minikube will automatically create a configuration file that `kubectl` can use to connect to the Kubernetes cluster.

## Part 2: Pods, Deployments, and Services "the Easy Way"

In this part, you'll use `kubectl` to create Kubernetes pods, deployments, and services without getting into any of the details around these concepts.

1. Use `kubectl run nginx-web --image=nginx --replicas=1` to launch an Nginx pod. This may take a few minutes as it must pull the Nginx container image down from the Internet.

2. Run `kubectl get pod,deployment` to see that the previous step created a deployment object as well as a single pod (managed by the deployment).

3. Run `kubectl describe deployment nginx-web` to get more details on the deployment created by step 1. Note that this deployment has a label "run=nginx-web".

4. Run `kubectl describe pod <pod-name>` to get more details on the pod created by step 1. The name of the pod is available in the output of the command in step 2 (feel free to re-run that command if the output has scrolled out of sight). Note that the pod also has the label "run=nginx-web".

5. Run `kubectl get deployment,rs,pod`. Note the similarities in names between the deployment, the replica set, and the final pod (each name is derived from the previous object).

6. Use `kubectl expose deployment nginx-web --name nginx-service --port-=8888` to make this Nginx pod accessible outside the cluster.

7. Use `kubectl get pod,deployment,service` to see the objects created by the previous two steps. You should see a single deployment, a single pod, and a single service.

8. Run `kubectl describe service nginx-service` to get more details on the pod created by step 2. Note the "Selector" line, which shows that this service is using the "run=nginx-web" label to note the pods that will be part of this service.

9. Delete the service and the deployment using `kubectl delete service nginx-service` and `kubectl delete deployment nginx-web`, respectively. Use `kubectl get pods` to verify that deleting the deployment also deleted the pod.

## Part 3: Pods, Deployments, and Services "the Hard Way"

In this final part, you'll examine more details on pods, deployments, and services, and work with the YAML files that define these objects in Kubernetes.

1. Examine the `01-pod.yaml` file in this tutorial. If you have any questions about any of the values supplied, use `kubectl explain pod` to get more information.

2. Create the pod by running `kubectl apply -f 01-pod.yaml`. Use `kubectl get pods` to verify that the pod was created.

3. Use `kubectl get pods,deployments,services` to verify that _only_ a pod was created.

4. Delete the pod using `kubectl delete pod nginx-pod`.

5. Examine the `02-deployment.yaml` file in this tutorial. Remember that you can use `kubectl explain deployment` to get more details on any of the values in this file.

6. Create the deployment by running `kubectl apply -f 02-deployment.yaml`.

7. Use `kubectl get pods,deployments,services` to see that a deployment was created, which in turn created a pod. Note there is no service yet.

8. Use `kubectl get deployments,replicasets,pods` to see that a replica set was created automatically when you created the deployment.

9. Use `kubectl describe` to get more details on each of the objects returned by step 7. Note the relationships between the objects (a deployment manages a replica set, which in turn manages a group of pods), and note the use of labels across the various objects.

10. Take a look at the `03-service.yaml` file in this tutorial. Use `kubectl explain service` to get more details on anything you don't understand.

11. Create the service using `kubectl apply -f 03-service.yaml`. After it has been created (you can verify with `kubectl get services`), then get more details on the service using `kubectl describe service nginx-service`.

12. Remove all the objects with the following commands:

        kubectl delete service nginx-service
        kubectl delete deployment nginx-deployment
    
    Verify the objects are gone with `kubectl get deployment,rs,service,pod`.

That's it!

[link-1]: https://github.com/kubernetes/minikube/
[link-2]: https://virtualbox.org/
[link-3]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-via-curl
