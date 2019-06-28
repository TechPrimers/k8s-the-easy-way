# K8s - CKAD - The Easy Way
Tips and Tricks on Kubernetes

Tables of Contents
==================
- [Short Names for Resources](#short-names-for-resources)
- [Commands](#commands)
- [Tricks](#tricks)

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

## Commands
### Switch clusters
`kubectl config use-context <cluster_nme>`

### Get resources
`kubectl -n <namespace> get <resource_name>`

## Tricks
```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell`.
```
