# Deployment Configurations

Sub directories contain scripts and configuration for deployment in target platforms. 

# Assignment

## Requirements

Make sure you have `kubectl` and `helm` installed to your system.

Please, make sure you fork this repository and clone your fork locally.


## Install an Ingress Controller to the cluster

* Install an ingress controller to the cluster using the given loadbalancer IP `34.136.78.189`.
* Install it under the `kube-ingress` namespace.

### Notes

There is a Reserved Public IP in Google Cloud for this task: `34.136.78.189` and a DNS name configured for `socking.devops.atypon.com`.
Suggested approach is to install nginx-ingress using the official [Helm Chart](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/). If using nginx-ingress, the property to set the LoadBalancerIP is `--set controller.service.loadbalancerIP=34.136.78.189`.

## Install the microservices-demo application

**Please do NOT modify the given `resources:`. They are well defined and they are not related with any errors.**

* Install the given chart under the `microservices-demo` namespace.
* You should be able to access the service from http://socking.devops.atypon.com (If you have issues, resolving the service, then please make sure IPv6 is disabled on your system or add a static entry in your etc hosts file for `34.136.78.189 socking.devops.atypon.com`).

## Troubleshooting

* When you are done with the deployment you may notice that 2 deployments (`carts` & `user`) are not marked as `Ready` and Kubernetes is restarting the services. 
* Please make sure to identify the issues, and try to fix them.
* Make sure you commit your fixes.

## Optional task

* Under autoscaling configuration there are example configurations for enabling Horizontal Pod Autoscaler. You may pick up any configuration you want, for any service and integrate it to the chart or deploy it to the cluster (using kubectl). This is an optional task.

## Cleanup

When done please do *not* remove your work in order to be able to review your work.
Also please make sure you commit your work to your forked repository and open a Pull Request to my repository.

If however you need to cleanup your work (eg you made a mistake)
```
$ helm -n kube-ingress list
$ helm -n kube-ingress delete <your_helm_release_name>
$ kubectl delete ns microservices-demo
$ helm -n microservices-demo list
$ helm -n microservices-demo delete <your_helm_release_name>
$ kubectl delete ns kube-ingress
```
