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
Readiness probe is used to identify if the container is ready to accept traffic. In simple terms, it denotes if the Application is completely UP and ready for serving requests.
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
If the liveness probe fails, the Pod will be restarted. This denotes that the health of the container is not good.
```
...
spec:
  containers:
  - image: 'gcr.io/<project_name>/spring-boot-example:v1'
    name: spring-boot-example
    livenessProbe:
      httpGet:
        path: /actuator/keepalive
        port: 8080
      initialDelaySeconds: 15
 ```

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


## Scratch Pad
 1004  k run nginx1 --image=nginx --restart=Never -l app=v1
 1005  k run nginx2 --image=nginx --restart=Never -l app=v1
 1006  k run nginx3 --image=nginx --restart=Never -l app=v1
 1007  k get po
 1008  k get po -o wide
 1009  k get po --labels
 1010  k get po --show-labels
 1011  k get po --label-columns
 1012  k get po --label-column
 1013  k get po --label-columns=app
 1014  k edit label
 1015  k edit -h
 1016  k edit po nginx2
 1017  k get po --show-labels
 1018  k label po -h
 1019  history
 1020  k label po nginx2 app=v2 --override
 1021  k label po nginx2 app=v2 --overwrite
 1024  k get po --label-columns=app
 1025  k get po -L app
 1026  k get po -L app=v2
 1027  k get po -h
 1028  k get po -l app=v2
 1029  k get po -l 'app in (v1, v2)'
 1030  k get po -l 'app in (v2)'
 1031  c
 1032  k label po -h
 1033  k label pods app-
 1034  k label pods app- --all
 1035  k get po
 1036  k get po --show-labels
 1037  kubectl label po -lapp app-
 1038  kubectl label po nginx{1..3} app-
 1039  kubectl label po nginx1 nginx2 nginx3 app-
 1040  c
 1041  k run -h
 1042  k run -h | grep label
 1043  k get no -h
 1044  k get no -h | grep label
 1045  k get no --show-labels
 1046  k run nginx --image=nginx --restart=Never --field-selector accelerator=nvidia-tesla-p100
 1047  k explain po.spec | grep node
 1048  k run nginx --image=nginx --restart=Never --dry-run -o yaml > pod-selector.yml
 1049  vi pod-selector.yml
 1050  k create -f pod-selector.yml
 1051  k get po -w
 1052  k annotate -h
 1053  k annotate pods nginx1 nginx2 nginx3 description='mydescription'
 1054  k describe pods nginx1
 1056  k describe po nginx1 | grep -i 'annotations'
 1057  k describe po nginx1 -o jsonpath='{.metadata.Annotations}'
 1058  k describe po nginx1 -o jsonpath='{.metadata.annotations}'
 1059  k annotate -h
 1060  k annotate po nginx1 nginx2 nginx3 description-
 1061  k describe po nginx1 | grep -i 'annotations'
 1062  k delete po nginx nginx1 nginx2 nginx3
 1063  k run nginx --image=nginx:1.7.8 --replicas=2 --port=80 --dry-run -o yaml
 1064  k run nginx --image=nginx:1.7.8 --replicas=2 --port=80 --dry-run -o yaml > deployment.yml
 1065  k create -f deployment.yml
 1066  k get de -w
 1067  k get do -w
 1068  k get deploy -w
 1069  k get all
 1070  kubectl create deployment nginx  --image=nginx:1.7.8  --dry-run -o yaml
 1071  k describe deploy nginx -o yaml
 1072  k get deploy nginx -o yaml
 1073  k get deploy nginx -o yaml --export
 1074  c
 1075  k get rs nginx
 1076  k get all
 1077  k get rs nginx-587746989d -o yaml
 1078  k get rs nginx-587746989d -o yaml  --export
 1079  k get rs -l run=nginx
 1080  k get pods
 1081  k get po nginx-587746989d-4rbmb -o yaml --expor
 1082  k get po nginx-587746989d-4rbmb -o yaml --export
 1083  c
 1084  k rollout deployment nginx
 1085  k rollout status deployment nginx
 1086  k rollout -h
 1087  k set image -h
 1088  k set image deploy nginx nginx=nginx:1.7.9
 1089  k describe deploy nginx
 1090  k rollout history deploy nginx
 1091  k get rs nginx
 1092  k get rs
 1093  k get po
 1094  k get deploy
 1095  k rollout undo deploy nginx
 1096  k get po -w
 1097  k describe deploy nginx
 1098  k set image deploy nginx nginx=nginx:1.91
 1099  k get all
 1100  k get po -w
 1101  k rollout status deploy nginx
 1102  k history deploy nginx
 1103  k rollout history deploy nginx
 1104  k rollout -h
 1105  k rollout deploy -h
 1106  k rollout undo -h
 1107  k rollout undo --to-revision=2
 1108  k rollout undo deploy nginx --to-revision=2
 1109  k get po -w
 1110  k rollout status deploy nginx
 1111  k describe deploy nginx| grep -i 'Image'
 1112  k rollout -h
 1113  k rollout history deploy nginx -h
 1114  k rollout history deploy nginx --revision=3
 1115  k scale deploy nginx -h
 1116  k scale deploy nginx --replicas=5
 1117  k get po -w
 1118  k scale deploy -h
 1119  k autoscale
 1120  k autoscale -h
 1121  c
 1122  history
 1123  c
 1124  k autoscale -h
 1125  k autoscale deploy nginx --min=5 --max=10 --cpu-percent=80
 1126  k describe deploy nginx
 1127  k describe rs
 1128  k rollout pause deploy nginx
 1129  k set image deploy nginx nginx=nginx:1.9.1
 1130  k rollout status deploy nginx
 1131  k rollout history deploy nginx
 1132  k rollout resume deploy nginx
 1133  k rollout status deploy nginx
 1134  k get po
 1135  k rollout history deploy nginx
 1136  k rollout history deploy nginx --revision=6
 1137  k get all
 1138  k delete deploy nginx
 1139  k getl all
 1140  k get all
 1141  k delete hps -h
 1142  k get hpa
 1143  k delete hpa nginx
 1144  k get all
 1147  k run perl -h
 1148  k run perl --restart=OnFailure -c "perl -Mbignum=bpi -wle 'print bpi(2000)'"
 1149  k run perl --restart=OnFailure --image=perl --command "perl -Mbignum=bpi -wle 'print bpi(2000)'" --dry-run -o yaml
 1150  k run perl --restart=OnFailure --image=perl --command "perl -Mbignum=bpi -wle 'print bpi(2000)'"
 1151  k get all
 1152  k rollout batch perl
 1153  k rollout -h
 1154  k get all
 1155  k logs perl
 1156  k logs perl-62hgp
 1157  k get all
 1158  k delete job perl
 1159  k get all
 1160  k create job perl --image=perl -- "perl -Mbignum=bpi -wle 'print bpi(2000)'"
 1161  k get all
 1162  k get po
 1163  k get po -w
 1164  k delete job perl
 1165  k create job pi --image=perl -- "perl -Mbignum=bpi -wle 'print bpi(2000)'"
 1166  k get po -w
 1167  k get all
 1168  k delete job pi
 1169  k create job pi --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'
 1170  k get po -w
 1171  k logs pi-whrqr
 1172  k get all
 1173  k delete job pi
 1174  k get all
 1175  c
 1176  k create job bb --image=busybox -- echo hello;sleep 30;echo world
 1177  k get po -w
 1178  k logs bb-6v4cj
 1179  k logs bb-6v4cj -f
 1180  k get job
 1181  k get all
 1182  k job
 1183  k get job -o wide
 1184  k describe job bb
 1185  k logs job/bb
 1186  k get jobs
 1187  k delete job bb
 1188  k create job -h
 1189  k explain job.spec
 1190  k create job bb image=busybox --dry-run -o yaml
 1191  k create job bb --image=busybox --dry-run -o yaml
 1192  k create job bb --image=busybox --dry-run -o yaml > job.yaml
 1193  k explain job.spec
 1194  vi job.yaml
 1195  k create -f job.yaml
 1196  k get all
 1197  vi job.yaml
 1198  k apply -f job.yaml
 1199  k get all
 1200  k delete job bb
 1201  k create -f job.yaml
 1202  k get all
 1203  k get job
 1204  k create job -h
 1205  k create cronjob -h
 1206  k create job -h
 1207  k run busybox --restart=OnFailure -h
 1208  k run busybox --restart=OnFailure --schedule='*/1 * * * *' --image=busybox -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
 1209  k get job
 1210  k get all
 1211  k get cronjob
 1212  k logs cronjob/busybox
 1213  k get cronjob
 1214  k run busybox2 --restart=OnFailure --schedule="*/1 * * * *" --image=busybox -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
 1215  k get cronjob
 1216  k logs cronjob/busybox
 1217  k get all
 1218  k logs cronjob/busybox
 1219  k logs cronjob/busybox2
 1220  k logs cronjob/busybox -f
 1221  k get jobs -w
 1222  k logs pod/busybox2-1563334860-5dnl8
 1223  k logs pod/busybox-1563334800-5jts2
 1224  k get po --show-labels
 1225  k delete cj busybox
 1226  k delete cj busybox2
