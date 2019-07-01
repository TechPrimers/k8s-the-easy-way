# K8s - CKAD - The Easy Way
Tips and Tricks on Kubernetes

Table of Contents
=================

   * [K8s - CKAD - The Easy Way](#k8s---ckad---the-easy-way)
   * [Table of Contents](#table-of-contents)
      * [Short Names for Resources](#short-names-for-resources)
      * [Which apiVersion should I use?](#which-apiversion-should-i-use)
         * [What does each apiVersion mean?](#what-does-each-apiversion-mean)
            * [alpha](#alpha)
            * [beta](#beta)
            * [stable](#stable)
            * [v1](#v1)
            * [apps/v1](#appsv1)
            * [autoscaling/v1](#autoscalingv1)
            * [batch/v1](#batchv1)
            * [batch/v1beta1](#batchv1beta1)
            * [certificates.k8s.io/v1beta1](#certificatesk8siov1beta1)
            * [extensions/v1beta1](#extensionsv1beta1)
            * [policy/v1beta1](#policyv1beta1)
            * [rbac.authorization.k8s.io/v1](#rbacauthorizationk8siov1)
      * [Commands](#commands)
         * [Get list of contexts](#get-list-of-contexts)
         * [Switch clusters](#switch-clusters)
         * [Set Namespace for all commands](#set-namespace-for-all-commands)
         * [Get resources](#get-resources)
      * [Understanding YAML](#understanding-yaml)
         * [Selectors](#selectors)
            * [Equality Based Selectors (Replication Controller) [Old way]](#equality-based-selectors-replication-controller-old-way)
            * [Set Based Selectors (ReplicatSet) [New way]](#set-based-selectors-replicatset-new-way)
            * [MatchLabels](#matchlabels)
      * [Tricks](#tricks)
         * [AutoCompletion in BASH](#autocompletion-in-bash)
         * [Set Aliases](#set-aliases)
      * [References](#references)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

## Short Names for Resources
Short name	| Full name
-----       | -----
csr      	  | certificatesigningrequests
cs	        | componentstatuses
cm	        | configmaps
ds	        | daemonsets
deploy	    | deployments
ep	        | endpoints
ev	        | events
hpa      	  | horizontalpodautoscalers
ing	        | ingresses
limits	    | limitranges
ns	        | namespaces
no	        | nodes
pvc	        | persistentvolumeclaims
pv	        | persistentvolumes
po	        | pods
pdb	        | poddisruptionbudgets
psp	        | podsecuritypolicies
rs	        | replicasets
rc	        | replicationcontrollers
quota	      | resourcequotas
sa	        | serviceaccounts
svc	        | services

## Which apiVersion should I use?
Kind	                          | apiVersion
----                            | ----
CertificateSigningRequest	      | certificates.k8s.io/v1beta1
ClusterRoleBinding	            | rbac.authorization.k8s.io/v1
ClusterRole	                    | rbac.authorization.k8s.io/v1
ComponentStatus	                | v1
ConfigMap	                      | v1
ControllerRevision	            | apps/v1
CronJob	                        | batch/v1beta1
DaemonSet	                      | extensions/v1beta1
Deployment	                    | extensions/v1beta1
Endpoints	                      | v1
Event	                          | v1
HorizontalPodAutoscaler	        | autoscaling/v1
Ingress	                        | extensions/v1beta1
Job	                            | batch/v1
LimitRange	                    | v1
Namespace	                      | v1
NetworkPolicy	                  | extensions/v1beta1
Node	                          | v1
PersistentVolumeClaim	          | v1
PersistentVolume	              | v1
PodDisruptionBudget	            | policy/v1beta1
Pod	                            | v1
PodSecurityPolicy	              | extensions/v1beta1
PodTemplate	                    | v1
ReplicaSet	                    | extensions/v1beta1
ReplicationController	          | v1
ResourceQuota	                  | v1
RoleBinding	                    | rbac.authorization.k8s.io/v1
Role	                          | rbac.authorization.k8s.io/v1
Secret	                        | v1
ServiceAccount	                | v1
Service	                        | v1
StatefulSet	                    | apps/v1

### What does each apiVersion mean?
#### alpha
API versions with ‘alpha’ in their name are early candidates for new functionality coming into Kubernetes. These may contain bugs and are not guaranteed to work in the future.

#### beta
‘beta’ in the API version name means that testing has progressed past alpha level, and that the feature will eventually be included in Kubernetes. Although the way it works might change, and the way objects are defined may change completely, the feature itself is highly likely to make it into Kubernetes in some form.

#### stable
These do not contain ‘alpha’ or ‘beta’ in their name. They are safe to use.

#### v1
This was the first stable release of the Kubernetes API. It contains many core objects.

#### apps/v1
apps is the most common API group in Kubernetes, with many core objects being drawn from it and v1. It includes functionality related to running applications on Kubernetes, like Deployments, RollingUpdates, and ReplicaSets.

#### autoscaling/v1
This API version allows pods to be autoscaled based on different resource usage metrics. This stable version includes support for only CPU scaling, but future alpha and beta versions will allow you to scale based on memory usage and custom metrics.

#### batch/v1
The batch API group contains objects related to batch processing and job-like tasks (rather than application-like tasks like running a webserver indefinitely). This apiVersion is the first stable release of these API objects.

#### batch/v1beta1
A beta release of new functionality for batch objects in Kubernetes, notably including CronJobs that let you run Jobs at a specific time or periodicity.

#### certificates.k8s.io/v1beta1
This API release adds functionality to validate network certificates for secure communication in your cluster. You can read more on the official docs.

#### extensions/v1beta1
This version of the API includes many new, commonly used features of Kubernetes. Deployments, DaemonSets, ReplicaSets, and Ingresses all received significant changes in this release.

Note that in Kubernetes 1.6, some of these objects were relocated from extensions to specific API groups (e.g. apps). When these objects move out of beta, expect them to be in a specific API group like apps/v1. Using extensions/v1beta1 is becoming deprecated—try to use the specific API group where possible, depending on your Kubernetes cluster version.

#### policy/v1beta1
This apiVersion adds the ability to set a pod disruption budget and new rules around pod security.

#### rbac.authorization.k8s.io/v1
This apiVersion includes extra functionality for Kubernetes role-based access control. This helps you to secure your cluster. Check out the official blog post.

## Commands
### Get list of contexts
`kubectl config get-contexts                          # display list of contexts`

### Switch clusters
`kubectl config use-context <cluster_name>`

### Set Namespace for all commands
```
# permanently save the namespace for all subsequent kubectl commands in that context.
kubectl config set-context --current --namespace=<namespace_name>`
```

### Get resources
`kubectl -n <namespace> get <resource_name>`

### Login to pod
```
> kubectl exec <pod_name> --stdin --tty -c <container_name> /bin/sh
Eg:
> kubectl exec nginx-59577d796b-2fsv5 --stdin --tty -c nginx /bin/sh
# ps -ef | grep nginx
root         1     0  0 17:39 ?        00:00:00 nginx: master process nginx -g daemon off;
nginx        6     1  0 17:39 ?        00:00:00 nginx: worker process
root        15     7  0 17:55 pts/0    00:00:00 grep nginx
# 
```

### Access Pod - PortForward
- Used for testing purpose in exposing a port in Pod.
- For Production, use services.
- 443 is the port running in the Container
`kubectl port-forward <pod_name> 10443:443`


### Create Secret & ConfigMap
```
kubectl create secret generic tls-certs --from-file=<folder_name>
kubectl create configmap nginx-proxy-config --from-file=<folder_name>/proxy.conf
```

### Create Volume Mounts
```
...
spec:
  template:
    spec:
      containers:
      - name: nginx
        image: nginx:1.10
        ports:
        - containerPort: 80
        volumeMounts:
        - name: "nginx-proxy-conf"
          mountPath: "/etc/nginx/conf.d"
        - name: "tls-certs"
          mountPath: "/etc/tls"
```

### Create Lifecyle
#### PreStop
```
...
spec:
  template:
    spec:
      containers:
      - name: nginx
        image: nginx:1.10
        ports:
        - containerPort: 80
        lifecycle:
          preStop: 
            exec:
              command: ["/usr/sbin/nginx/", "-s", "quit"]
        volumeMounts:
        - name: "nginx-proxy-conf"
          mountPath: "/etc/nginx/conf.d"
        - name: "tls-certs"
          mountPath: "/etc/tls"
```

### Cluster info
```
> kubectl cluster-info
Kubernetes master is running at https://192.168.64.6:8443
KubeDNS is running at https://192.168.64.6:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

## Understanding YAML
### Selectors
#### Equality Based Selectors (Replication Controller) [Old way]
- Works on `=`, `==`, and `!=`
- Easy to understand but less powerful
Command: `kubectl get pods -l environment=production`
```
...
selector:
  environment: production
  tier: frontend
```

#### Set Based Selectors (ReplicatSet) [New way]
- Works on `in`, `notin` and `exists`
- Complex to understand but very powerful
Command: `kubectl get pods -l 'environment in (production)'
```
...
selector:
  matchExpressions:
    - {key: environment, operator: In, values: [prod, qa]}
    - {key: tier, operator: NotIn, values: [frontend, backend]}
```

#### MatchLabels
- `ReplicationControllers` and `Services` doesnt support `matchLabels` under selectors [Old resources]
- `Deployments`, `ReplicaSet`, `Jobs` and `DaemonSet` supports `matchLabels` under selectors [New resources]

### Health Checks
#### Readiness Probe
```
...
readinessProbe:
  httpGet:
    path: /readiness
    port: 81
    scheme: HTTP
    initialDelaySeconds: 5
    timeoutSeconds: 1
```

#### Liveness Probe

## Tricks
### AutoCompletion in BASH
```
# setup autocomplete in bash into the current shell, bash-completion package should be installed first.
source <(kubectl completion bash) 
# add autocomplete permanently to your bash shell`.
echo "source <(kubectl completion bash)" >> ~/.bashrc 
```

### Set Aliases 
```
alias k=kubectl
k get pod
```

## References
- [kubectl cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

