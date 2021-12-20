# Assignment

## Requirements

Please make sure you have `kubectl` and `helm` installed on your system.  
Also, make sure you fork this repository and clone your fork locally.

In order to be able to connect to the Kubernetes cluster extract the contents of the `kube-assignment.tgz` (sent via e-mail) under your home directory.
```
$ cd ~
$ tar zxvf kube-assignment.tgz
$ ls -l ~/kube-assignment/
$ export KUBECONFIG=~/kube-assignment/kubeconfig.yaml
$ export GOOGLE_APPLICATION_CREDENTIALS=~/kube-assignment/service-account-key.json
```

and verify connectivity to the cluster. For example:
```
$ kubectl get ns
$ kubectl version
```


## TASK 1: Install an Ingress Controller to the K8s cluster

* Install Traefik ingress controller to the cluster using the provided loadbalancer IP: `34.136.78.189`.
* Install it under the `kube-ingress` namespace.

#### Notes

* There is a Reserved Public IP in Google Cloud for this task: `34.136.78.189` 
* There is also a DNS `A` record for `socking.devops.atypon.com` already configured.
* Suggested approach is to install traefik-ingress using the official [Traefik Chart](https://github.com/traefik/traefik-helm-chart/tree/master/traefik).
* If using traefik-ingress Helm chart, the property to set the LoadBalancerIP is `--set service.spec.loadBalancerIP=34.136.78.189`.
* There are no active Kubernetes worker nodes deployed in the cluster. They will be deployed once you install your first workload.

## TASK 2: Install the microservices-demo application

**Please do NOT modify the given `resources` (`limits` & `requests`) in the chart.**  
**They are well defined and they are not related with any errors.**

* The [helm-charts folder](./deploy/kubernetes/helm-chart/) contains a single Helm chart for deploying all the involved services. Please, install the given chart under the `microservices-demo` namespace.
* Once the frond-end is ready, you should be able to access the service from http://socking.devops.atypon.com. (HTTP not HTTPs)

#### Screenshot
![Sock Shop frontend](/assets/sockshop-frontend.png?raw=true "Sock Shop frontend")

## TASK 3: Troubleshooting

* When you are done with your installation you may notice that 2 services (`carts` & `user`) are not marked as `Ready` and Kubernetes keeps restarting the services. This has an impact to the site as it is not working properly.
```
$ kubectl -n microservices-demo get pods  | grep -E '0/1'
carts-bb5cb9ddd-64dwp           0/1     Running   10         80m
user-8547d89b-c2d2k             0/1     Running   15         80m
```

* Please make sure to identify the issues, and try to fix them by redeploying your changes.

#### Notes

`kubectl` subcommands `logs`, `describe` & `events -w` may come handy for this task.  
In addition, reviewing the template files for these `Deployments` objects in Kubernetes may help you to identify the issues.

## TASK 4: Horizontal Pod Autoscaler (Optional task)

* Under [autoscaling folder](./deploy/kubernetes/autoscaling/) there are sample configurations for enabling Horizontal Pod Autoscaler. You may pick up any configuration, for any service and integrate it to the chart or deploy it to the K8s cluster (using `kubectl apply -f`). This is an optional task.

## TASK 5: Pull Request

Once you have identified the issues, please make sure you commit your changes to your forked repository and open a Pull Request to this repository.

## Cleanup

**When done please do *not* remove the services in order to be able to review your progress.**

If, for any reason, you need to cleanup your work (eg in case of a mistake)
```
# delete the microservice
$ helm -n microservices-demo list
$ helm -n microservices-demo delete <your_helm_release_name>
$ kubectl delete ns microservices-demo

# delete the ingress
$ helm -n kube-ingress list
$ helm -n kube-ingress delete <your_helm_release_name>
$ kubectl delete ns kube-ingress
```
