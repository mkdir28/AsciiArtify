# POC AsciiArtify

Deploying a GitOps system on the Kubernetes variant **k3d** for local test. 
The team recommended the [**ArgoCD product**](https://argo-cd.readthedocs.io/en/stable/getting_started/).
This part will check whether it is technically possible to implement the product concept for **ascii-art on ArgoCD**

1. 
```bash
➜ ~ k3d cluster create argo
INFO[0000] Prep: Network                                
INFO[0000] Created network 'k3d-argo'                   
INFO[0000] Created image volume k3d-argo-images         
INFO[0000] Starting new tools node...                   
INFO[0000] Starting node 'k3d-argo-tools'               
INFO[0001] Creating node 'k3d-argo-server-0'            
INFO[0002] Creating LoadBalancer 'k3d-argo-serverlb'    
INFO[0004] Using the k3d-tools node to gather environment information 
INFO[0005] Starting new tools node...                   
INFO[0006] Starting node 'k3d-argo-tools'               
INFO[0008] Starting cluster 'argo'                      
INFO[0008] Starting servers...                          
INFO[0008] Starting node 'k3d-argo-server-0'            
INFO[0015] All agents already running.                  
INFO[0015] Starting helpers...                          
INFO[0016] Starting node 'k3d-argo-serverlb'            
INFO[0024] Injecting records for hostAliases (incl. host.k3d.internal) and for 3 network members into CoreDNS configmap... 
INFO[0027] Cluster 'argo' created successfully!         
INFO[0028] You can now use it like this:                
kubectl cluster-info
```

2. 
```bash
➜ ~ kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:54573
CoreDNS is running at https://0.0.0.0:54573/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:54573/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

3. 
```bash
➜  ~ k get all -A
NAMESPACE     NAME                                          READY   STATUS              RESTARTS   AGE
kube-system   pod/coredns-ccb96694c-f7l76                   1/1     Running             0          86s
kube-system   pod/helm-install-traefik-crd-dd78p            0/1     Completed           0          86s
kube-system   pod/helm-install-traefik-k9hpt                0/1     Completed           2          86s
kube-system   pod/local-path-provisioner-5cf85fd84d-5lfl6   1/1     Running             0          86s
kube-system   pod/metrics-server-5985cbc9d7-9qshz           1/1     Running             0          86s
kube-system   pod/svclb-traefik-f282ba4a-7jkzt              2/2     Running             0          13s
kube-system   pod/traefik-5d45fc8cc9-2b758                  0/1     ContainerCreating   0          14s

NAMESPACE     NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
default       service/kubernetes       ClusterIP      10.43.0.1      <none>        443/TCP                      102s
kube-system   service/kube-dns         ClusterIP      10.43.0.10     <none>        53/UDP,53/TCP,9153/TCP       96s
kube-system   service/metrics-server   ClusterIP      10.43.246.47   <none>        443/TCP                      88s
kube-system   service/traefik          LoadBalancer   10.43.48.49    172.21.0.3    80:31916/TCP,443:31057/TCP   14s

NAMESPACE     NAME                                    DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
kube-system   daemonset.apps/svclb-traefik-f282ba4a   1         1         1       1            1           <none>          14s

NAMESPACE     NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/coredns                  1/1     1            1           96s
kube-system   deployment.apps/local-path-provisioner   1/1     1            1           95s
kube-system   deployment.apps/metrics-server           1/1     1            1           88s
kube-system   deployment.apps/traefik                  0/1     1            0           14s

NAMESPACE     NAME                                                DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/coredns-ccb96694c                   1         1         1       86s
kube-system   replicaset.apps/local-path-provisioner-5cf85fd84d   1         1         1       86s
kube-system   replicaset.apps/metrics-server-5985cbc9d7           1         1         1       86s
kube-system   replicaset.apps/traefik-5d45fc8cc9                  1         1         0       14s

NAMESPACE     NAME                                 STATUS     COMPLETIONS   DURATION   AGE
kube-system   job.batch/helm-install-traefik       Complete   1/1           77s        87s
kube-system   job.batch/helm-install-traefik-crd   Complete   1/1           57s        87s
```


4. 
```bash
➜  ~ kubectl create namespace argocd
namespace/argocd created
```


5. 
```bash
➜  ~ k get ns
NAME              STATUS   AGE
argocd            Active   15s
default           Active   9m3s
kube-node-lease   Active   9m3s
kube-public       Active   9m3s
kube-system       Active   9m3s
```

6. 
```bash
~ kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

7. 
```bash
➜  ~ k get all -n argocd
NAME                                                    READY   STATUS              RESTARTS   AGE
pod/argocd-application-controller-0                     0/1     ContainerCreating   0          40s
pod/argocd-applicationset-controller-6fb45499bf-vdp72   0/1     ContainerCreating   0          42s
pod/argocd-dex-server-5f969b9b4d-kwqgm                  0/1     Init:0/1            0          42s
pod/argocd-notifications-controller-5d488dc9dd-68j48    0/1     ContainerCreating   0          41s
pod/argocd-redis-59f4c5f58b-mntlz                       0/1     Init:0/1            0          41s
pod/argocd-repo-server-84788d4897-kdtdq                 0/1     Init:0/1            0          41s
pod/argocd-server-6856fd4959-7jccs                      0/1     ContainerCreating   0          40s

NAME                                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/argocd-applicationset-controller          ClusterIP   10.43.163.213   <none>        7000/TCP,8080/TCP            42s
service/argocd-dex-server                         ClusterIP   10.43.191.10    <none>        5556/TCP,5557/TCP,5558/TCP   42s
service/argocd-metrics                            ClusterIP   10.43.105.208   <none>        8082/TCP                     42s
service/argocd-notifications-controller-metrics   ClusterIP   10.43.232.115   <none>        9001/TCP                     42s
service/argocd-redis                              ClusterIP   10.43.192.76    <none>        6379/TCP                     42s
service/argocd-repo-server                        ClusterIP   10.43.153.67    <none>        8081/TCP,8084/TCP            42s
service/argocd-server                             ClusterIP   10.43.255.238   <none>        80/TCP,443/TCP               42s
service/argocd-server-metrics                     ClusterIP   10.43.157.211   <none>        8083/TCP                     42s

NAME                                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/argocd-applicationset-controller   0/1     1            0           42s
deployment.apps/argocd-dex-server                  0/1     1            0           42s
deployment.apps/argocd-notifications-controller    0/1     1            0           42s
deployment.apps/argocd-redis                       0/1     1            0           41s
deployment.apps/argocd-repo-server                 0/1     1            0           41s
deployment.apps/argocd-server                      0/1     1            0           41s

NAME                                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/argocd-applicationset-controller-6fb45499bf   1         1         0       42s
replicaset.apps/argocd-dex-server-5f969b9b4d                  1         1         0       42s
replicaset.apps/argocd-notifications-controller-5d488dc9dd    1         1         0       42s
replicaset.apps/argocd-redis-59f4c5f58b                       1         1         0       41s
replicaset.apps/argocd-repo-server-84788d4897                 1         1         0       41s
replicaset.apps/argocd-server-6856fd4959                      1         1         0       41s
```


8. 
```bash
  ~ k get po -n argocd -w
NAME                                                READY   STATUS    RESTARTS      AGE
argocd-application-controller-0                     1/1     Running   0             90s
argocd-applicationset-controller-6fb45499bf-vdp72   1/1     Running   0             92s
argocd-dex-server-5f969b9b4d-kwqgm                  1/1     Running   2 (26s ago)   92s
argocd-notifications-controller-5d488dc9dd-68j48    1/1     Running   0             91s
argocd-redis-59f4c5f58b-mntlz                       1/1     Running   0             91s
argocd-repo-server-84788d4897-kdtdq                 1/1     Running   0             91s
argocd-server-6856fd4959-7jccs                      0/1     Running   0             90s
argocd-server-6856fd4959-7jccs                      1/1     Running   0             91s
```


9. 
```bash
➜  ~ kubectl port-forward svc/argocd-server -n argocd 8080:443&
[1] 4764

➜  ~ Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
Handling connection for 8080
```


10. 
```bash
➜  ~ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath"={.data.password}"
OENDQzA4aWxmNmhEN3VzTw==%     
```


11. 
```bash
➜  ~ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath"={.data.password}"| base64 -d;echo
8CCC08ilf6hD7usO
```
