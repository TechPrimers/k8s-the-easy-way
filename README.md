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
```

k run nginx1 --image=nginx --restart=Never -l app=v1
k run nginx2 --image=nginx --restart=Never -l app=v1
k run nginx3 --image=nginx --restart=Never -l app=v1
k get po
k get po -o wide
k get po --labels
k get po --show-labels
k get po --label-columns
k get po --label-column
k get po --label-columns=app
k edit label
k edit -h
k edit po nginx2
k get po --show-labels
k label po -h
history
k label po nginx2 app=v2 --override
k label po nginx2 app=v2 --overwrite
k get po --label-columns=app
k get po -L app
k get po -L app=v2
k get po -h
k get po -l app=v2
k get po -l 'app in (v1, v2)'
k get po -l 'app in (v2)'
c
k label po -h
k label pods app-
k label pods app- --all
k get po
k get po --show-labels
kubectl label po -lapp app-
kubectl label po nginx{1..3} app-
kubectl label po nginx1 nginx2 nginx3 app-
c
k run -h
k run -h | grep label
k get no -h
k get no -h | grep label
k get no --show-labels
k run nginx --image=nginx --restart=Never --field-selector accelerator=nvidia-tesla-p100
k explain po.spec | grep node
k run nginx --image=nginx --restart=Never --dry-run -o yaml > pod-selector.yml
vi pod-selector.yml
k create -f pod-selector.yml
k get po -w
k annotate -h
k annotate pods nginx1 nginx2 nginx3 description='mydescription'
k describe pods nginx1
k describe po nginx1 | grep -i 'annotations'
k describe po nginx1 -o jsonpath='{.metadata.Annotations}'
k describe po nginx1 -o jsonpath='{.metadata.annotations}'
k annotate -h
k annotate po nginx1 nginx2 nginx3 description-
k describe po nginx1 | grep -i 'annotations'
k delete po nginx nginx1 nginx2 nginx3
k run nginx --image=nginx:1.7.8 --replicas=2 --port=80 --dry-run -o yaml
k run nginx --image=nginx:1.7.8 --replicas=2 --port=80 --dry-run -o yaml > deployment.yml
k create -f deployment.yml
k get de -w
k get do -w
k get deploy -w
k get all
kubectl create deployment nginx  --image=nginx:1.7.8  --dry-run -o yaml
k describe deploy nginx -o yaml
k get deploy nginx -o yaml
k get deploy nginx -o yaml --export
c
k get rs nginx
k get all
k get rs nginx-587746989d -o yaml
k get rs nginx-587746989d -o yaml  --export
k get rs -l run=nginx
k get pods
k get po nginx-587746989d-4rbmb -o yaml --expor
k get po nginx-587746989d-4rbmb -o yaml --export
c
k rollout deployment nginx
k rollout status deployment nginx
k rollout -h
k set image -h
k set image deploy nginx nginx=nginx:1.7.9
k describe deploy nginx
k rollout history deploy nginx
k get rs nginx
k get rs
k get po
k get deploy
k rollout undo deploy nginx
k get po -w
k describe deploy nginx
k set image deploy nginx nginx=nginx:1.91
k get all
k get po -w
k rollout status deploy nginx
k history deploy nginx
k rollout history deploy nginx
k rollout -h
k rollout deploy -h
k rollout undo -h
k rollout undo --to-revision=2
k rollout undo deploy nginx --to-revision=2
k get po -w
k rollout status deploy nginx
k describe deploy nginx| grep -i 'Image'
k rollout -h
k rollout history deploy nginx -h
k rollout history deploy nginx --revision=3
k scale deploy nginx -h
k scale deploy nginx --replicas=5
k get po -w
k scale deploy -h
k autoscale
k autoscale -h
c
history
c
k autoscale -h
k autoscale deploy nginx --min=5 --max=10 --cpu-percent=80
k describe deploy nginx
k describe rs
k rollout pause deploy nginx
k set image deploy nginx nginx=nginx:1.9.1
k rollout status deploy nginx
k rollout history deploy nginx
k rollout resume deploy nginx
k rollout status deploy nginx
k get po
k rollout history deploy nginx
k rollout history deploy nginx --revision=6
k get all
k delete deploy nginx
k getl all
k get all
k delete hps -h
k get hpa
k delete hpa nginx
k get all
k run perl -h
k run perl --restart=OnFailure -c "perl -Mbignum=bpi -wle 'print bpi(2000)'"
k run perl --restart=OnFailure --image=perl --command "perl -Mbignum=bpi -wle 'print bpi(2000)'" --dry-run -o yaml
k run perl --restart=OnFailure --image=perl --command "perl -Mbignum=bpi -wle 'print bpi(2000)'"
k get all
k rollout batch perl
k rollout -h
k get all
k logs perl
k logs perl-62hgp
k get all
k delete job perl
k get all
k create job perl --image=perl -- "perl -Mbignum=bpi -wle 'print bpi(2000)'"
k get all
k get po
k get po -w
k delete job perl
k create job pi --image=perl -- "perl -Mbignum=bpi -wle 'print bpi(2000)'"
k get po -w
k get all
k delete job pi
k create job pi --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'
k get po -w
k logs pi-whrqr
k get all
k delete job pi
k get all
c
k create job bb --image=busybox -- echo hello;sleep 30;echo world
k get po -w
k logs bb-6v4cj
k logs bb-6v4cj -f
k get job
k get all
k job
k get job -o wide
k describe job bb
k logs job/bb
k get jobs
k delete job bb
k create job -h
k explain job.spec
k create job bb image=busybox --dry-run -o yaml
k create job bb --image=busybox --dry-run -o yaml
k create job bb --image=busybox --dry-run -o yaml > job.yaml
k explain job.spec
vi job.yaml
k create -f job.yaml
k get all
vi job.yaml
k apply -f job.yaml
k get all
k delete job bb
k create -f job.yaml
k get all
k get job
k create job -h
k create cronjob -h
k create job -h
k run busybox --restart=OnFailure -h
k run busybox --restart=OnFailure --schedule='*/1 * * * *' --image=busybox -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
k get job
k get all
k get cronjob
k logs cronjob/busybox
k get cronjob
k run busybox2 --restart=OnFailure --schedule="*/1 * * * *" --image=busybox -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'
k get cronjob
k logs cronjob/busybox
k get all
k logs cronjob/busybox
k logs cronjob/busybox2
k logs cronjob/busybox -f
k get jobs -w
k logs pod/busybox2-1563334860-5dnl8
k logs pod/busybox-1563334800-5jts2
k get po --show-labels
k delete cj busybox
k delete cj busybox2

k create configmap config --from-literal=foo=lala --from-literal=foo2=lolo
k get cm
k get cm -o wide
k get all
k describe cm config
k get cm config
k get cm config -o yaml
k get cm config -o yaml --export
c
k create configmap -h
c
echo -e "foo3=lili\nfoo4=lele" > config.txt
k create configmap config-file --from-file=./config.txt
k describe cm config-file
k get cm config-file -o yaml
k get cm config-file -o yaml --export
c
echo -e "var1=val1\n# this is a comment\n\nvar2=val2\n#anothercomment" > config.env
k create cm -h
c
k create cm config-env-file --from-env-file=./config.env
k get cm config-env-file
k get cm config-env-file -o yaml
k get cm config-env-file -o yaml --export
echo -e "var3=val3\nvar4=val4" > config4.txt
k create cm config4 --from-file ./config4.txt
k get cm config4 -o yaml --export
c
k get cm config4 -o yaml --export
k get cm config4 -o yaml --export > config-map-key.yml
vi config
vi config-map-key.yml
k explain configmap.spec
k explain configmap
k explain configmap.metadata
k explain configmap --recursive | grep key
k explain configmap --recursive | grep generator
k explain configmap --recursive | grep -i generator
c
k create cm config5 --from-file=special=./config4.txt -o yaml --dry-run
k create cm options --from-literal=var5=val5
k describe cm options
k run nginx --image=nginx --restart=Never -h
c
k run nginx --image=nginx --restart=Never --dry-run -o yaml > config-pod.yml
vi config
vi config-pod.yml
k create -f config-pod.yml
vi config-pod.yml
k create -f config-pod.yml
k describe po nginx
k exec nginx -- env
k create configmap env:
c
k create configmap anotherone --from-literal=var6=val6 --from-literal=var7=val7
k run nginx2 --image=nginx --restart=Never --dry-run -o yaml > pod-config2.yml
vi pod-config2.yml
k create -f pod-config2.yml
k describe po nginx2
k exec nginx2 -- env
k create cm cmvolume --from-literal var8=val8 var9=val9
k create cm cmvolume --from-literal var8=val8 --from-literal var9=val9
k run nginx3 --image=nginx --restart=Never --dry-run -o yaml -- /bin/sh 'ls /etc/lala'
k run nginx3 --image=nginx --restart=Never --dry-run -o yaml -- /bin/sh 'ls /etc/lala' > nginx3.yml
vi nginx3.yml
k create -f nginx3.yml
k logs nginx3
k describe nginx3
k describe po nginx3
k logs nginx3
k exec nginx3 -- ls /etc/lala
vi nginx3.yml
k delete po nginx3
k create -f nginx3.yml
vi nginx3.yml
k create -f nginx3.yml
vi nginx3.yml
k create -f nginx3.yml
vi nginx3.yml
k create -f nginx3.yml
vi nginx3.yml
c
k run nginx -h
k run nginx -h | grep UID
k run nginx -h | grep -i UID
k run nginx --restart=Never --image=nginx --dry-run -o yaml
k run nginx --=h
k run nginx --restart=Never --requests cpu=100m,memory=256Mi --limits cpu=200m,memory=512Mi --dry-run -o yaml
k run nginx --restart=Never --requests cpu=100m,memory=256Mi --limits cpu=200m,memory=512Mi --image=nginx --dry-run -o yaml
k run nginx --restart=Never --requests cpu=100m,memory=256Mi --limits cpu=200m,memory=512Mi --image=nginx
k run nginx7 --restart=Never --requests cpu=100m,memory=256Mi --limits cpu=200m,memory=512Mi --image=nginx
k describe nginx7
k describe po nginx7
c
k get all
k delete po nginx nginx2 nginx7
c
k create secret -h
k create secret generic -j
k create secret generic --from-literal password=mypass
k create secret generic --from-literal=password=mypass
k create secret generic --from-literal password=mypass
k create secret generic mysecret --from-literal password=mypass
k get secret
k describe secret mysecret
echo admin > username
k create secret generic mysecret2 -h
k create secret generic mysecret2 --from-file ./username
k describe secret mysecret2
k get secret mysecret2
k get secret mysecret2 -o yaml
k get secret mysecret2 -o yaml --export
k get secret mysecret2 -o yaml --export | cut -f :
cut help
cut --help
k get secret mysecret2 -o yaml --export | grep username| cut -f 2
k get secret mysecret2 -o yaml --export | grep username| cut -f 2 ':'
k get secret mysecret2 -o yaml --export | grep username| cut ':' -f 2
k get secret mysecret2 -o yaml --export | grep username| cut  -f 2 -d ':'
k get secret mysecret2 -o yaml --export | grep username| cut -f 2 -d ':' | cut -d ' '
k get secret mysecret2 -o yaml --export | grep username| cut -f 2 -d ':' | cut -d ' ' -f 2
k get secret mysecret2 -o yaml --export | grep username| cut -f 2 -d ':' | cut -d ' ' -f 2
k get secret mysecret2 -o yaml --export | grep username| cut -f 2 -d ':' | cut -d ' ' -f 2
kubectl get secret mysecret2 -o jsonpath='{.data.username}{"\n"}'
vi pod-secret.yml
k create -f pod-secret.yml
k exec secret-test-pod -it -- /bin/bash
k get po
k delete po secret-test-po
k delete po secret-test-pod
vi pod-secret.yml
k create -f pod-se
k create -f pod-secret.yml
k describe po secret-test-pod
k get po
k exec secret-text-po -it -- /bin.bash
k exec secret-text-pod -it -- /bin/bash
k exec secret-test-pod -it -- /bin/bash
c
k get sa --all-namespaces
k create sa myuser
k describe sa myuser
k run nginx --image=nginx --restart=Never --serviceaccount
k run nginx --image=nginx --restart=Never --serviceaccount=myuser --dry-run -o yaml
k run nginx --image=nginx --restart=Never --serviceaccount=myuser
k describe po nginx | grep service

k cp bb:/etc/passwd ./passwd2
vi passwd
vi pass
k exec bb -- /bin/sh -c 'cat /etc/passwd' > passwd
k exec bb -- /bin/sh -c 'cat /etc/passwd'
k run bb --image=busybox --restart=Never -- /bin/sh -c 'sleep 3600'
k delete po busybox busybox1
k exec busybox1 -it -- /bin/sh
k create -f pvc-pod.yaml
vi pvc-pod.yaml
k exec busybox -it -- /bin/sh
k describe po busybox
vi pvc-pod.y
k run busybox --restart=Never --image=busybox --dry-run -o yaml --command -- /bin/sh -c 'sleep 3600' > pvc-pod.yaml
k delete po busybox
vi pvc-pod.yml
vi pv
k run busybox --restart=Never --image=busybox -o yaml --command -- /bin/sh -c 'sleep 3600' > pvc-pod.yml
k run busybox --restart=Never --image=busybox --command -- /bin/sh -c 'sleep 3600' > pvc-pod.yml
k get pv
k get pvc
k describe pvc mypvc
k create -f pvc.yaml
vi pvc.yaml
k describe pv myvolume
k create -f pv.yaml
vi pv.yaml
k delete po bb
k get po
k exec bb -c bb1 -it -- /bin/sh -c 'cat /etc/foo/passwd'
k exec bb -c bb1 -it -- /bin/sh
k exec bb -c bb2 -it -- /bin/sh
k create -f bb2.yaml
vi bb2.yaml
k run bb --image=busybox --restart=Never --dry-run -o yaml -- /bin/sh -c 'sleep 3600' > bb2.yaml
vi bb
k run bb --image=busybox --restart=Never --dry-run -o yaml > bb2.yaml
k run bb --image=busybox --restart=Never --dry-run -o yaml
k get all
k delete po nginx3
k exec nginx3 -it -- /bin/sh
k exec nginx3 -- ls /etc/lala
k exec nginx -- ls /etc/lala
k describe po nginx3
k create -f config-pod.yaml
k delete pod nginx3
k delete pod nginx2
k delete pod nginx1
k delete pod nginx{1..3}
vi config-pod.yaml
k run nginx3 --restart=Never --image=nginx --dry-run -o yaml> config-pod.yaml
k run nginx3 --restart=Never --image=nginx --dry-run -o yaml
k create cm cmvolume --from-literal var8=val8 --from-literal var9=val9
k create cm config5 --from-file=special=./config4.txt -o yaml --dry-run
k get cm config4 -o yaml
k get cm config4
k get cm confi4
k create -f config4.yml
vi config4.yml
k create cm config4 --from-file=./config4.txt --dry-run -o yaml > config4.yml
ls
pwd
k create cm config4 --from-file=./config4.txt --dry-run -o yaml
echo -e "var3=val3\nvar4=val4" > config4.txt
minikube start
minikube status
export PS1="[\u@\h \W]\$ " ;clear;
exit
minikube stop
k get po -o wide
k delete pod nginx
k run nginx1 --image=nginx --restart=Never --labels=app=v1
k run nginx3 --image=nginx --restart=Never --labels=app=v1
k run nginx2 --image=nginx --restart=Never --labels=app=v1
k run nginx --image=nginx --restart=Never --labels=app=v1
minikube delete
k describe po nginx
k exec nginx -it sh
k run nginx --image=nginx --restart=Never --env="var1=val1"
k run bb1 --restart=Never --image=busybox -it --rm --command -- echo 'Hello World'
k run busybox1 --restart=Never --image=busybox --rm --command -- echo 'Hello World'
k logs busybox
k get po busybox -w
k get po busybox
k run busybox --restart=Never --image=busybox --command -- echo 'Hello World'
k delete pod busybox
k logs pod/nginx -p
k logs pod/nginx
k get po nginx -o yaml --export
k get po nginx -o yaml -h
k get po nginx -o yaml
k get po nginx -o jsonpath='{.status.podIP}'
k get po nginx --jsonpath='.status.PodIP'
k run bb --image=busybox --restart=Never --rm -it -- /bin/sh
k get po -w
kubectl set image pod/nginx nginx=nginx:1.7.1
k set image po nginx nginx=nginx:1.7.1
k apply -f try4.yml
vi try4.yml
k run nginx --image=nginx --restart=Never --port=80 -o yaml --dry-run > try4.yml
k describe po nginx -o yaml
k edit po -h
k edit po nginx --image=nginx:.7.1
k run nginx --image=nginx --restart=Never --port=80
k run nginx --image=nginx --restart=Never --port=80 -o yaml --dry-run
k delete pod nginx -n mynamespace
k get po --all-namespaces
k create quota myrq --dry-run -o yaml --hard=cpu=1,memory=1G,pods=2 > try3.yml
k create quota myrq --dry-run -o yaml --hard=cpu=1,memory=1G,pods=2
k create quota myrq --dry-run -o yaml
k create quota -h
k create resourcequota
k create namespace myns --dry-run -o yaml > try3.yml
k create namespace myns --dry-run -o yaml
k get pod -w
k create -f try2.yml
k run busybox --restart=Never --image=busybox --dry-run -o yaml --command -- env > try2.yml
k run busybox --restart=Never --image=busybox --dry-run -o yaml --command -- env
k run busybox --restart=Never --image=busybox --command -- env -o yaml --dry-run
k run busybox --restart=Never --image=busybox --command='env' -o yaml --dry-run
k run nginx --restart=Never --image=nginx -o yaml --dry-run > try1.yml
k run nginx --restart=Never --image=nginx -o yaml --dry-run
k get all -n mynamespace
k run nginx --restart=Never --image=nginx -n mynamespace
k delete deploy nginx -n mynamespace
k get all -n mynamespace -n mynamespace
k delete deploy nginx
k run nginx --image=nginx --namespace=mynamespace
k create namespace mynamespace
k delete svc nginx
vi ingress-np.yml
k create -f ingress-np.yml
k create np -h
k run nginx --image=nginx --port=80 --expose --replicas=2
k delete deploy foo
k delete svc foo
k get ep foo
k get ep
k get endpoints
wget 10.98.203.247:6262/
k describe svc foo
wget 10.98.203.247:6262
wget -O- 192.168.64.9:6262
wget -O- http://127.0.0.1:6262
wget -O- http://192.168.64.9:6262
wget -O-http://192.168.64.9:6262
wget http://192.168.64.9:6262
minikube ip
k get deploy
k get svc
wget http://10.98.203.247:6262
wget http://10.104.71.160:6262
k expose deployment foo --port=6262 --target-port=8080
wget http://10.104.71.160:6262/
k get svc -w
k expose deployment foo --port=6262
kubectl run foo --image=dgkanatsios/simpleapp --labels=app=foo --port=8080 --replicas=3
k version

```
