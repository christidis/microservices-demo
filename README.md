# Assignment

## Requirements

Please make sure you have `kubectl` and `helm` installed to your system.  
Also, make sure you fork this repository and clone your fork locally.


## TASK 1: Install an Ingress Controller to the K8s cluster

* Install an ingress controller to the cluster using the given loadbalancer IP: `34.136.78.189`.
* Install it under the `kube-ingress` namespace.

#### Notes

* There is a Reserved Public IP in Google Cloud for this task: `34.136.78.189` 
* The DNS name is already configured for `socking.devops.atypon.com`.
* Suggested approach is to install nginx-ingress using the official [Helm Chart](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/).
* If using nginx-ingress Helm chart, the property to set the LoadBalancerIP is `--set controller.service.loadbalancerIP=34.136.78.189`.

## TASK 2: Install the microservices-demo application

**Please do NOT modify the given `resources:`. They are well defined and they are not related with any errors.**

* The [helm-charts folder](./deploy/kubernetes/helm-chart/) contains a single Helm chart for deploying all the involved services. Please, install the given chart under the `microservices-demo` namespace.
* You should be able to access the service from http://socking.devops.atypon.com. The DNS is already prepared for you.
* If you have issues, accessing the service, then please make sure that IPv6 is disabled on your system. Alternatively, add a static entry in your `/etc/hosts` file for `34.136.78.189 socking.devops.atypon.com`.

#### Screenshot
![Sock Shop frontend](https://github.com/microservices-demo/microservices-demo.github.io/raw/master/assets/sockshop-frontend.png)

## TASK 3: Troubleshooting

* When you are done with your installation you may notice that 2 services (`carts` & `user`) are not marked as `Ready` and Kubernetes keeps restarting the services. This has an impact to the site as it is not working properly.
```
$ kubectl -n microservices-demo get pods  | grep -E '0/1'
carts-bb5cb9ddd-64dwp           0/1     Running   10         80m
user-8547d89b-c2d2k             0/1     Running   15         80m
```

* Please make sure to identify the issues, and try to fix them by redeploying your changes.

#### Notes

`kubectl` subcommands `logs` & `describe` may come handy for this task. In addition, reviewing the template files for these `Deployments` will help you to identify the error.

## TASK 4: Horizontal Pod Autoscaler (Optional task)

* Under [autoscaling folder](./deploy/kubernetes/autoscaling/) there are sample configurations for enabling Horizontal Pod Autoscaler. You may pick up any configuration you want, for any service you like and integrate it to the chart or deploy it to the cluster (using kubectl apply). This is an optional task.

## TASK 5: Pull Request

Please make sure you commit your changes to your forked repository and open a Pull Request to this repository.


## Cleanup

When done please do *not* remove your work in order to be able to review your work.

If, for any reason, you need to cleanup your work (eg you made a mistake)
```
# delete the ingress
$ helm -n kube-ingress list
$ helm -n kube-ingress delete <your_helm_release_name>
$ kubectl delete ns kube-ingress

# delete the microservice
$ helm -n microservices-demo list
$ helm -n microservices-demo delete <your_helm_release_name>
$ kubectl delete ns microservices-demo
```
