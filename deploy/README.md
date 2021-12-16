# Deployment Configurations

Sub directories contain scripts and configuration for deployment in target platforms. 

# Assignment

## Requirements

Please make sure you have `kubectl` and `helm` installed to your system.

Also, make sure you fork this repository and clone your fork locally.


## Install an Ingress Controller to the cluster

* Install an ingress controller to the cluster using the given loadbalancer IP: `34.136.78.189`.
* Install it under the `kube-ingress` namespace.

### Notes

There is a Reserved Public IP in Google Cloud for this task: `34.136.78.189` and a DNS name configured for `socking.devops.atypon.com`.
Suggested approach is to install nginx-ingress using the official [Helm Chart](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/). If using nginx-ingress, the property to set the LoadBalancerIP is `--set controller.service.loadbalancerIP=34.136.78.189`.

## Install the microservices-demo application

**Please do NOT modify the given `resources:`. They are well defined and they are not related with any errors.**

* Install the given chart under the `microservices-demo` namespace.
* You should be able to access the service from http://socking.devops.atypon.com. The DNS is already prepared for you.
* If you have issues, accessing the service, then please make sure IPv6 is disabled on your system for the assignment. Alternatively, add a static entry in your `/etc/hosts` file for `34.136.78.189 socking.devops.atypon.com`.

## Troubleshooting

* When you are done with your installation you may notice that 2 services (`carts` & `user`) are not marked as `Ready` and Kubernetes is restarting the services. This has an impact to the site as it is not working properly.
```
$ kubectl -n microservices-demo get pods  | grep -E '0/1'
carts-bb5cb9ddd-64dwp           0/1     Running   10         80m
user-8547d89b-c2d2k             0/1     Running   15         80m
```

* Please make sure to identify the issues, and try to fix them. `kubectl` `logs` & `describe` may come handy. Also, reviewing the template files for these `Deployments` will help you to identify the error.

## Optional task

* Under autoscaling configuration there are sample configurations for enabling Horizontal Pod Autoscaler. You may pick up any configuration you want, for any service you like and integrate it to the chart or deploy it to the cluster (using kubectl apply). This is an optional task.

## Pull Request

Please make sure you commit your changes to your forked repository and open a Pull Request to my repository.


## Cleanup

When done please do *not* remove your work in order to be able to review your work.

If, for any reason, you need to cleanup your work (eg you made a mistake)
```
$ helm -n kube-ingress list
$ helm -n kube-ingress delete <your_helm_release_name>
$ kubectl delete ns microservices-demo
$ helm -n microservices-demo list
$ helm -n microservices-demo delete <your_helm_release_name>
$ kubectl delete ns kube-ingress
```
