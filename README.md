# K8s - CKAD - The Easy Way
Tips and Tricks on Kubernetes

Tables of Contents
==================

   * [K8s - CKAD - The Easy Way](#k8s---ckad---the-easy-way)
   * [Tables of Contents](#tables-of-contents)
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
         * [Switch clusters](#switch-clusters)
         * [Set Namespace for all commands](#set-namespace-for-all-commands)
         * [Get resources](#get-resources)
      * [Tricks](#tricks)
         * [AutoCompletion in BASH](#autocompletion-in-bash)

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
kubectl config get-contexts                          # display list of contexts

### Switch clusters
`kubectl config use-context <cluster_name>`

### Set Namespace for all commands
`kubectl config set-context --current --namespace=ggckad-s2 # permanently save the namespace for all subsequent kubectl commands in that context.`

### Get resources
`kubectl -n <namespace> get <resource_name>`

## Tricks
### AutoCompletion in BASH
```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell`.
```

