## Sample2: Kubernetes Hello World with enableLiveness and hostname mapping

- This sample runs simple ballerina hello world service in kubernetes cluster with enableLiveness probe and  hostname
 mapping for ingress. 
- Following files will be generated from this sample.
    ``` 
    $> docker image
    hello-world-k8s:latest
    
    $> tree
    ├── README.md
    ├── hello_world_k8s.bal
    ├── hello_world_k8s.balx
    └── kubernetes
        ├── docker
        │   └── Dockerfile
        └── hello_world_k8s.yaml
    ```
### How to run:

1. Compile the  hello_world_k8s.bal file. Command to run kubernetes artifacts will be printed on success:
```bash
$> ballerina build hello_world_k8s.bal
@kubernetes:Docker 			 - complete 3/3 
@kubernetes:Deployment 		 - complete 1/1
@kubernetes:Service 		 - complete 1/1
@kubernetes:Ingress 		 - complete 1/1
Run following command to deploy kubernetes artifacts: 
kubectl apply -f /Users/lakmal/ballerina/kubernetes/samples/sample2/kubernetes/

```

2. hello_world_k8s.balx, Dockerfile, docker image and kubernetes artifacts will be generated: 
```bash
$> tree
├── README.md
├── hello_world_k8s.bal
├── hello_world_k8s.balx
└── kubernetes
    ├── docker
    │   └── Dockerfile
    └── hello_world_k8s.yaml
```

3. Verify the docker image is created:
```bash
$> docker images
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
hello_world_k8s      latest              df83ae43f69b        2 minutes ago        102MB

```

4. Run kubectl command to deploy artifacts (Use the command printed on screen in step 1):
```bash
$> kubectl apply -f ./kubernetes/
service "helloep-svc" created
deployment "hello-world-k8s-deployment" created
ingress "helloworld-ingress" created
```

5. Verify kubernetes deployment,service and ingress is running:
```bash
$> kubectl get pods
NAME                                                READY     STATUS    RESTARTS   AGE
hello-world-k8s-deployment-54768647ff-m64v9   1/1       Running   0          4s


$> kubectl get svc
NAME                                              TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
helloep-svc                                       ClusterIP      10.110.199.222   <none>        9090/TCP                     3m

$> kubectl get ingress
NAME                 HOSTS     ADDRESS   PORTS     AGE
helloworld-ingress   abc.com             80, 443   4m
```

6. Access the hello world service with curl command:

- **Using ingress**

Add /etc/hosts entry to match hostname.
_(127.0.0.1 is only applicable to docker for mac users. Other users should map the hostname with correct ip address 
from `kubectl get ingress` command.)_
 ```
 127.0.0.1 abc.com
 ```
Use curl command with hostname to access the service.
```bash
$> curl http://abc.com/HelloWorld/sayHello
Hello, World from service helloWorld !
```
7. Undeploy sample:
```bash
$> kubectl delete -f /Users/lakmal/ballerina/kubernetes/samples/sample2/kubernetes/
```