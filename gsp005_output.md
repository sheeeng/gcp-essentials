```
Welcome to Cloud Shell! Type "help" to get started.
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud auth list
          Credentialed Accounts
ACTIVE  ACCOUNT
*       google149349_student@qwiklabs.net

To set the active account, run:
    $ gcloud config set account `ACCOUNT`
var http = require('http');

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud config list project
[core]
FROM node:9.2.0
project = qwiklabs-gcp-b6915dadfc82bd20

Your active configuration is: [cloudshell-20361]
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud config set project qwiklabs-gcp-b6915dadfc82bd20
Updated property [core/project].
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud config list project
[core]
project = qwiklabs-gcp-b6915dadfc82bd20

Your active configuration is: [cloudshell-20361]
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ vi server.js
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ node server.js
^C
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ vi Dockerfile
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ cat server.js
var http = require('http');
var handleRequest = function(request, response) {
  response.writeHead(200);
  response.end("Hello, Google Cloud Platform folks!");
}
var www = http.createServer(handleRequest);
www.listen(8080);
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ cat Dockerfile
FROM node:9.2.0
EXPOSE 8080
COPY server.js .
CMD node server.js
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ docker build -t gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1 .
Sending build context to Docker daemon  11.78kB
Step 1/4 : FROM node:9.2.0
9.2.0: Pulling from library/node
85b1f47fba49: Pull complete
ba6bd283713a: Pull complete
817c8cd48a09: Pull complete
47cc0ed96dc3: Pull complete
8888adcbd08b: Pull complete
6f2de60646b9: Pull complete
9dd205971dc0: Pull complete
5859715a4691: Pull complete
Digest: sha256:7c9099e0f68242387d7755eaa54c287e16cedd3cca423444ca773794f5f1e423
Status: Downloaded newer image for node:9.2.0
 ---> c1d02ac1d9b4
Step 2/4 : EXPOSE 8080
 ---> Running in 11b83ee4c8f2
 ---> 2a03f57dd440
Removing intermediate container 11b83ee4c8f2
Step 3/4 : COPY server.js .
 ---> 62570c6322b2
Removing intermediate container a1c13c1e4586
Step 4/4 : CMD node server.js
 ---> Running in 1eb7f22def9a
 ---> fe3eec942e2d
Removing intermediate container 1eb7f22def9a
Successfully built fe3eec942e2d
Successfully tagged gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ docker run -d -p 8080:8080 gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1
c4f27fbcf1364a5e218f8c75dba767774d717a22f5afee7d6b35c9fffffd2559
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ curl http://localhost:8080
Hello, Google Cloud Platform folks!
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ docker ps
CONTAINER ID        IMAGE                                                COMMAND                  CREATED             STATUS              PORTS                    NAMES
c4f27fbcf136        gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1   "/bin/sh -c 'node ..."   32 seconds ago      Up 30 seconds       0.0.0.0:8080->8080/tcp   pedantic_nightingale
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ docker stop pedantic_nightingale
pedantic_nightingale
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud docker -- push gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1
The push refers to a repository [gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node]
af951c42b7ee: Pushed
072b714c90b8: Layer already exists
ea2e97e9408f: Layer already exists
0da372da714b: Layer already exists
bf3841becf9d: Layer already exists
63866df00998: Layer already exists
2f9128310b77: Layer already exists
d9a5f9b8d5c2: Layer already exists
c01c63c6823d: Layer already exists
v1: digest: sha256:be3fae31c1880e5309c79ba2296f4f5013f3996bc16d27bd3d2b50a81fe8f31f size: 2214
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud config list project
[core]
project = qwiklabs-gcp-b6915dadfc82bd20

Your active configuration is: [cloudshell-20177]
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud container clusters create hello-world \
>                 --num-nodes 2 \
>                 --machine-type n1-standard-1 \
>                 --zone europe-west1-b
Creating cluster hello-world...done.
Created [https://container.googleapis.com/v1/projects/qwiklabs-gcp-b6915dadfc82bd20/zones/europe-west1-b/clusters/hello-world].
kubeconfig entry generated for hello-world.
NAME         ZONE            MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION  NUM_NODES  STATUS
hello-world  europe-west1-b  1.7.8-gke.0     35.187.31.114  n1-standard-1  1.7.8-gke.0   2          RUNNING
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl run hello-node \
>     --image=gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1 \
>     --port=8080
deployment "hello-node" created
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get deployments
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   1         1         1            0           14s
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get pods
NAME                          READY     STATUS              RESTARTS   AGE
hello-node-1380548439-p44w1   0/1       ContainerCreating   0          31s
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get pods
NAME                          READY     STATUS    RESTARTS   AGE
hello-node-1380548439-p44w1   1/1       Running   0          44s
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl cluster-info
Kubernetes master is running at https://35.187.31.114
GLBCDefaultBackend is running at https://35.187.31.114/api/v1/namespaces/kube-system/services/default-http-backend/proxy
Heapster is running at https://35.187.31.114/api/v1/namespaces/kube-system/services/heapster/proxy
KubeDNS is running at https://35.187.31.114/api/v1/namespaces/kube-system/services/kube-dns/proxy
kubernetes-dashboard is running at https://35.187.31.114/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: REDACTED
    server: https://35.187.31.114
  name: gke_qwiklabs-gcp-b6915dadfc82bd20_europe-west1-b_hello-world
contexts:
- context:
    cluster: gke_qwiklabs-gcp-b6915dadfc82bd20_europe-west1-b_hello-world
    user: gke_qwiklabs-gcp-b6915dadfc82bd20_europe-west1-b_hello-world
  name: gke_qwiklabs-gcp-b6915dadfc82bd20_europe-west1-b_hello-world
current-context: gke_qwiklabs-gcp-b6915dadfc82bd20_europe-west1-b_hello-world
kind: Config
preferences: {}
users:
- name: gke_qwiklabs-gcp-b6915dadfc82bd20_europe-west1-b_hello-world
  user:
    auth-provider:
      config:
        access-token: ya29.GqMBFAU_AYzeTXZ6V-2MHYfVk02BK7lLk5d0P1afIXB0w9DP54XpjOHznwoIO1BJtTzBs__08RZOuyfNN8zp1cqKIm493YJdgNW757ZljW4dBhYEJKn1jWVTugfG5o78aQTWyP4aS4WZ6_cXiCjwzdFeUXkHfiXaRbYaiAVMnaIUeWaqqH56fZ1gFBVaSN5DD9DGNuRmdIobsGOkve74itsKLRagwg
        cmd-args: config config-helper --format=json
        cmd-path: /google/google-cloud-sdk/bin/gcloud
        expiry: 2017-11-30T20:01:03Z
        expiry-key: '{.credential.token_expiry}'
        token-key: '{.credential.access_token}'
      name: gcp
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get events
LAST SEEN   FIRST SEEN   COUNT     NAME                                                          KIND         SUBOBJECT                     TYPE      REASON                    SOURCE                                                   MESSAGE
3m          3m           1         gke-hello-world-default-pool-68b67db9-29xk.14fbf3322508bdd3   Node                                       Normal    Starting                  kubelet, gke-hello-world-default-pool-68b67db9-29xk      Starting kubelet.
3m          3m           2         gke-hello-world-default-pool-68b67db9-29xk.14fbf3322668e2da   Node                                       Normal    NodeHasSufficientDisk     kubelet, gke-hello-world-default-pool-68b67db9-29xk      Node gke-hello-world-default-pool-68b67db9-29xk status is now: NodeHasSufficientDisk
3m          3m           2         gke-hello-world-default-pool-68b67db9-29xk.14fbf332266bce42   Node                                       Normal    NodeHasSufficientMemory   kubelet, gke-hello-world-default-pool-68b67db9-29xk      Node gke-hello-world-default-pool-68b67db9-29xk status is now: NodeHasSufficientMemory
3m          3m           2         gke-hello-world-default-pool-68b67db9-29xk.14fbf332266e217e   Node                                       Normal    NodeHasNoDiskPressure     kubelet, gke-hello-world-default-pool-68b67db9-29xk      Node gke-hello-world-default-pool-68b67db9-29xk status is now: NodeHasNoDiskPressure
3m          3m           1         gke-hello-world-default-pool-68b67db9-29xk.14fbf332279cd441   Node                                       Normal    NodeAllocatableEnforced   kubelet, gke-hello-world-default-pool-68b67db9-29xk      Updated Node Allocatable limit across pods
3m          3m           1         gke-hello-world-default-pool-68b67db9-29xk.14fbf3340f6090df   Node                                       Normal    Starting                  kube-proxy, gke-hello-world-default-pool-68b67db9-29xk   Starting kube-proxy.
3m          3m           1         gke-hello-world-default-pool-68b67db9-29xk.14fbf3346ccc3ff7   Node                                       Normal    RegisteredNode            controllermanager                                        Node gke-hello-world-default-pool-68b67db9-29xk event: Registered Node gke-hello-world-default-pool-68b67db9-29xk in NodeController
2m          2m           1         gke-hello-world-default-pool-68b67db9-29xk.14fbf3371c036a0b   Node                                       Normal    NodeReady                 kubelet, gke-hello-world-default-pool-68b67db9-29xk      Node gke-hello-world-default-pool-68b67db9-29xk status is now: NodeReady
3m          3m           1         gke-hello-world-default-pool-68b67db9-fkv0.14fbf332178b062c   Node                                       Normal    Starting                  kubelet, gke-hello-world-default-pool-68b67db9-fkv0      Starting kubelet.
3m          3m           2         gke-hello-world-default-pool-68b67db9-fkv0.14fbf332196e7243   Node                                       Normal    NodeHasSufficientDisk     kubelet, gke-hello-world-default-pool-68b67db9-fkv0      Node gke-hello-world-default-pool-68b67db9-fkv0 status is now: NodeHasSufficientDisk
3m          3m           2         gke-hello-world-default-pool-68b67db9-fkv0.14fbf33219712615   Node                                       Normal    NodeHasSufficientMemory   kubelet, gke-hello-world-default-pool-68b67db9-fkv0      Node gke-hello-world-default-pool-68b67db9-fkv0 status is now: NodeHasSufficientMemory
3m          3m           2         gke-hello-world-default-pool-68b67db9-fkv0.14fbf33219739cc8   Node                                       Normal    NodeHasNoDiskPressure     kubelet, gke-hello-world-default-pool-68b67db9-fkv0      Node gke-hello-world-default-pool-68b67db9-fkv0 status is now: NodeHasNoDiskPressure
3m          3m           1         gke-hello-world-default-pool-68b67db9-fkv0.14fbf3321a39dd91   Node                                       Normal    NodeAllocatableEnforced   kubelet, gke-hello-world-default-pool-68b67db9-fkv0      Updated Node Allocatable limit across pods
3m          3m           1         gke-hello-world-default-pool-68b67db9-fkv0.14fbf334425ca544   Node                                       Normal    Starting                  kube-proxy, gke-hello-world-default-pool-68b67db9-fkv0   Starting kube-proxy.
3m          3m           1         gke-hello-world-default-pool-68b67db9-fkv0.14fbf3346ccb864a   Node                                       Normal    RegisteredNode            controllermanager                                        Node gke-hello-world-default-pool-68b67db9-fkv0 event: Registered Node gke-hello-world-default-pool-68b67db9-fkv0 in NodeController
2m          2m           1         gke-hello-world-default-pool-68b67db9-fkv0.14fbf33717a3a86a   Node                                       Normal    NodeReady                 kubelet, gke-hello-world-default-pool-68b67db9-fkv0      Node gke-hello-world-default-pool-68b67db9-fkv0 status is now: NodeReady
1m          1m           1         hello-node-1380548439-p44w1.14fbf347f965be0b                  Pod                                        Normal    Scheduled                 default-scheduler                                        Successfully assigned hello-node-1380548439-p44w1 to gke-hello-world-default-pool-68b67db9-29xk
1m          1m           1         hello-node-1380548439-p44w1.14fbf348023f4000                  Pod                                        Normal    SuccessfulMountVolume     kubelet, gke-hello-world-default-pool-68b67db9-29xk      MountVolume.SetUp succeeded for volume "default-token-lzwhr"
1m          1m           1         hello-node-1380548439-p44w1.14fbf3481c2ebd2a                  Pod          spec.containers{hello-node}   Normal    Pulling                   kubelet, gke-hello-world-default-pool-68b67db9-29xk      pulling image "gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1"
1m          1m           1         hello-node-1380548439-p44w1.14fbf34f7cbe01cd                  Pod          spec.containers{hello-node}   Normal    Pulled                    kubelet, gke-hello-world-default-pool-68b67db9-29xk      Successfully pulled image "gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1"
1m          1m           1         hello-node-1380548439-p44w1.14fbf34f80555ad2                  Pod          spec.containers{hello-node}   Normal    Created                   kubelet, gke-hello-world-default-pool-68b67db9-29xk      Created container
1m          1m           1         hello-node-1380548439-p44w1.14fbf34f87050c39                  Pod          spec.containers{hello-node}   Normal    Started                   kubelet, gke-hello-world-default-pool-68b67db9-29xk      Started container
1m          1m           1         hello-node-1380548439.14fbf347f7d54b98                        ReplicaSet                                 Normal    SuccessfulCreate          replicaset-controller                                    Created pod: hello-node-1380548439-p44w1
1m          1m           1         hello-node.14fbf347f63297ba                                   Deployment                                 Normal    ScalingReplicaSet         deployment-controller                                    Scaled up replica set hello-node-1380548439 to 1
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl describe pod hello-node-1380548439-p44w1
Name:           hello-node-1380548439-p44w1
Namespace:      default
Node:           gke-hello-world-default-pool-68b67db9-29xk/10.132.0.2
Start Time:     Thu, 30 Nov 2017 20:19:50 +0100
Labels:         pod-template-hash=1380548439
                run=hello-node
Annotations:    kubernetes.io/created-by={"kind":"SerializedReference","apiVersion":"v1","reference":{"kind":"ReplicaSet","namespace":"default","name":"hello-node-1380548439","uid":"6f9013da-d603-11e7-bf9a-42010a8401...
                kubernetes.io/limit-ranger=LimitRanger plugin set: cpu request for container hello-node
Status:         Running
IP:             10.44.1.7
Created By:     ReplicaSet/hello-node-1380548439
Controlled By:  ReplicaSet/hello-node-1380548439
Containers:
  hello-node:
    Container ID:   docker://657dec614ae9164fec93e2d08c8eed02a650d38b8250f60f50e723250b699946
    Image:          gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1
    Image ID:       docker-pullable://gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node@sha256:be3fae31c1880e5309c79ba2296f4f5013f3996bc16d27bd3d2b50a81fe8f31f
    Port:           8080/TCP
    State:          Running
      Started:      Thu, 30 Nov 2017 20:20:22 +0100
    Ready:          True
    Restart Count:  0
    Requests:
      cpu:        100m
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-lzwhr (ro)
Conditions:
  Type           Status
  Initialized    True
  Ready          True
  PodScheduled   True
Volumes:
  default-token-lzwhr:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-lzwhr
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.alpha.kubernetes.io/notReady:NoExecute for 300s
                 node.alpha.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason                 Age   From                                                 Message
  ----    ------                 ----  ----                                                 -------
  Normal  Scheduled              3m    default-scheduler                                    Successfully assigned hello-node-1380548439-p44w1 to gke-hello-world-default-pool-68b67db9-29xk
  Normal  SuccessfulMountVolume  3m    kubelet, gke-hello-world-default-pool-68b67db9-29xk  MountVolume.SetUp succeeded for volume "default-token-lzwhr"
  Normal  Pulling                3m    kubelet, gke-hello-world-default-pool-68b67db9-29xk  pulling image "gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1"
  Normal  Pulled                 3m    kubelet, gke-hello-world-default-pool-68b67db9-29xk  Successfully pulled image "gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v1"
  Normal  Created                3m    kubelet, gke-hello-world-default-pool-68b67db9-29xk  Created container
  Normal  Started                3m    kubelet, gke-hello-world-default-pool-68b67db9-29xk  Started container
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl expose deployment hello-node --type="LoadBalancer"
service "hello-node" exposed
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get services

NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-node   LoadBalancer   10.47.248.238   <pending>     8080:31514/TCP   7s
kubernetes   ClusterIP      10.47.240.1     <none>        443/TCP          7m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
hello-node   LoadBalancer   10.47.248.238   35.205.123.175   8080:31514/TCP   1m
kubernetes   ClusterIP      10.47.240.1     <none>           443/TCP          8m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ curl http://35.205.123.175:8080
Hello, Google Cloud Platform folks!google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ dig http://35.205.123.175:8080

; <<>> DiG 9.9.5-9+deb8u14-Debian <<>> http://35.205.123.175:8080
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 3274
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;http://35.205.123.175:8080.    IN      A

;; AUTHORITY SECTION:
.                       86399   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2017113001 1800 900 604800 86400

;; Query time: 26 msec
;; SERVER: 169.254.169.254#53(169.254.169.254)
;; WHEN: Thu Nov 30 20:27:10 CET 2017
;; MSG SIZE  rcvd: 130

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl scale deployment hello-node --replicas=5
deployment "hello-node" scaled
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get deployments
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   5         5         5            4           8m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get pods
NAME                          READY     STATUS              RESTARTS   AGE
hello-node-1380548439-63qcb   1/1       Running             0          18s
hello-node-1380548439-64w2g   1/1       Running             0          18s
hello-node-1380548439-bqm82   0/1       ContainerCreating   0          18s
hello-node-1380548439-mf2sw   1/1       Running             0          18s
hello-node-1380548439-p44w1   1/1       Running             0          8m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get pods
NAME                          READY     STATUS              RESTARTS   AGE
hello-node-1380548439-63qcb   1/1       Running             0          23s
hello-node-1380548439-64w2g   1/1       Running             0          23s
hello-node-1380548439-bqm82   0/1       ContainerCreating   0          23s
hello-node-1380548439-mf2sw   1/1       Running             0          23s
hello-node-1380548439-p44w1   1/1       Running             0          8m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get deployments
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   5         5         5            5           8m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get pods
NAME                          READY     STATUS    RESTARTS   AGE
hello-node-1380548439-63qcb   1/1       Running   0          46s
hello-node-1380548439-64w2g   1/1       Running   0          46s
hello-node-1380548439-bqm82   1/1       Running   0          46s
hello-node-1380548439-mf2sw   1/1       Running   0          46s
hello-node-1380548439-p44w1   1/1       Running   0          8m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl delete pods hello-node-1380548439-bqm82
pod "hello-node-1380548439-bqm82" deleted
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get deployments
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   5         5         5            5           10m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get pods
NAME                          READY     STATUS        RESTARTS   AGE
hello-node-1380548439-63qcb   1/1       Running       0          2m
hello-node-1380548439-64w2g   1/1       Running       0          2m
hello-node-1380548439-bqm82   1/1       Terminating   0          2m
hello-node-1380548439-lqc7w   1/1       Running       0          5s
hello-node-1380548439-mf2sw   1/1       Running       0          2m
hello-node-1380548439-p44w1   1/1       Running       0          10m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$


google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ vi server.js
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ docker build -t gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v2 .
Sending build context to Docker daemon  110.6kB
Step 1/4 : FROM node:9.2.0
 ---> c1d02ac1d9b4
Step 2/4 : EXPOSE 8080
 ---> Using cache
 ---> 2a03f57dd440
Step 3/4 : COPY server.js .
 ---> 68fac4d3d538
Removing intermediate container 356a20b2ee9d
Step 4/4 : CMD node server.js
 ---> Running in ddd96e4e5c61
 ---> 5ce7112d0203
Removing intermediate container ddd96e4e5c61
Successfully built 5ce7112d0203
Successfully tagged gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v2
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud docker -- push gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v2
The push refers to a repository [gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node]
38f10e0e77f0: Pushed
072b714c90b8: Layer already exists
ea2e97e9408f: Layer already exists
0da372da714b: Layer already exists
bf3841becf9d: Layer already exists
63866df00998: Layer already exists
2f9128310b77: Layer already exists
d9a5f9b8d5c2: Layer already exists
c01c63c6823d: Layer already exists
v2: digest: sha256:404338cce1eb322957cb628061db85e6ad1377100d9e5c2a304057f4c65b4be9 size: 2214
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ cat server.js
var http = require('http');
var handleRequest = function(request, response) {
  response.writeHead(200);
  response.end("Hello, Google Cloud Platform folks in the lab!");
}
var www = http.createServer(handleRequest);
www.listen(8080);
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl edit deployment hello-node
deployment "hello-node" edited
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ cat /tmp/kubectl-edit-fplln.yaml
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "2"
  creationTimestamp: 2017-11-30T19:19:50Z
  generation: 3
  labels:
    run: hello-node
  name: hello-node
  namespace: default
  resourceVersion: "2028"
  selfLink: /apis/extensions/v1beta1/namespaces/default/deployments/hello-node
  uid: 6f8eacd3-d603-11e7-bf9a-42010a840156
spec:
  replicas: 5
  selector:
    matchLabels:
      run: hello-node
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: hello-node
    spec:
      containers:
      - image: gcr.io/qwiklabs-gcp-b6915dadfc82bd20/hello-node:v2
        imagePullPolicy: IfNotPresent
        name: hello-node
        ports:
        - containerPort: 8080
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 4
  conditions:
  - lastTransitionTime: 2017-11-30T19:27:58Z
    lastUpdateTime: 2017-11-30T19:27:58Z
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  observedGeneration: 3
  readyReplicas: 4
  replicas: 6
  unavailableReplicas: 2
  updatedReplicas: 3
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl get deployments
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   5         5         5            5           20m
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ curl http://35.205.123.175:8080
Hello, Google Cloud Platform folks in the lab!
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$



google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ gcloud container clusters get-credentials hello-world \
>     --zone europe-west1-b --project qwiklabs-gcp-b6915dadfc82bd20
Fetching cluster endpoint and auth data.
kubeconfig entry generated for hello-world.
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ kubectl proxy --port 8081
Starting to serve on 127.0.0.1:8081
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ curl http://localhost:8081
{
  "paths": [
    "/api",
    "/api/v1",
    "/apis",
    "/apis/",
    "/apis/apiextensions.k8s.io",
    "/apis/apiextensions.k8s.io/v1beta1",
    "/apis/apiregistration.k8s.io",
    "/apis/apiregistration.k8s.io/v1beta1",
    "/apis/apps",
    "/apis/apps/v1beta1",
    "/apis/authentication.k8s.io",
    "/apis/authentication.k8s.io/v1",
    "/apis/authentication.k8s.io/v1beta1",
    "/apis/authorization.k8s.io",
    "/apis/authorization.k8s.io/v1",
    "/apis/authorization.k8s.io/v1beta1",
    "/apis/autoscaling",
    "/apis/autoscaling/v1",
    "/apis/batch",
    "/apis/batch/v1",
    "/apis/certificates.k8s.io",
    "/apis/certificates.k8s.io/v1beta1",
    "/apis/extensions",
    "/apis/extensions/v1beta1",
    "/apis/networking.k8s.io",
    "/apis/networking.k8s.io/v1",
    "/apis/policy",
    "/apis/policy/v1beta1",
    "/apis/rbac.authorization.k8s.io",
    "/apis/rbac.authorization.k8s.io/v1beta1",
    "/apis/storage.k8s.io",
    "/apis/storage.k8s.io/v1",
    "/apis/storage.k8s.io/v1beta1",
    "/healthz",
    "/healthz/SSH Tunnel Check",
    "/healthz/autoregister-completion",
    "/healthz/ping",
    "/healthz/poststarthook/apiservice-registration-controller",
    "/healthz/poststarthook/apiservice-status-available-controller",
    "/healthz/poststarthook/bootstrap-controller",
    "/healthz/poststarthook/ca-registration",
    "/healthz/poststarthook/extensions/third-party-resources",
    "/healthz/poststarthook/generic-apiserver-start-informers",
    "/healthz/poststarthook/kube-apiserver-autoregistration",
    "/healthz/poststarthook/rbac/bootstrap-roles",
    "/healthz/poststarthook/start-apiextensions-controllers",
    "/healthz/poststarthook/start-apiextensions-informers",
    "/healthz/poststarthook/start-kube-aggregator-informers",
    "/healthz/poststarthook/start-kube-apiserver-informers",
    "/logs",
    "/metrics",
    "/swagger-2.0.0.json",
    "/swagger-2.0.0.pb-v1",
    "/swagger-2.0.0.pb-v1.gz",
    "/swagger.json",
    "/swaggerapi",
    "/ui",
    "/ui/",
    "/version"
  ]
}google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$

google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$ curl http://localhost:8081/ui
<a href="/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy">Temporary Redirect</a>.
google149349_student@qwiklabs-gcp-b6915dadfc82bd20:~$
```
