# Kubernetes Notes

### What is Kubernetes (K8s) ?

It's a container orchestration platform that manages containers for you at scale.
It automates deployment, scaling, healing, networking and management of containerized applications.

# Docker vs Kubernetes

| Feature | Docker | Kubernetes |
|---------|--------|------------|
| **Purpose** | Creates, packages, and runs containers. | Orchestrates and manages multiple containers across machines. |
| **Primary Role** | Containerization platform. | Container orchestration platform. |
| **Deployment** | Runs containers on a single host. | Deploys containers across a cluster of machines. |
| **Scaling** | Manual scaling (or with Docker Compose/Swarm). | Automatic scaling based on CPU, memory, or custom metrics. |
| **Load Balancing** | Limited; requires additional configuration. | Built-in service discovery and load balancing. |
| **Self-Healing** | No automatic recovery if a container crashes. | Automatically restarts, replaces, and reschedules failed containers. |
| **High Availability** | Not designed for high availability by itself. | Designed for high availability and fault tolerance. |
| **Networking** | Basic container networking. | Advanced networking with Services, Ingress, and Network Policies. |
| **Storage** | Supports Docker Volumes for persistent data. | Supports Persistent Volumes (PV) and Persistent Volume Claims (PVC). |
| **Rolling Updates** | Manual or limited support. | Built-in rolling updates and rollbacks with zero downtime. |
| **Configuration Management** | Environment variables and mounted files. | ConfigMaps and Secrets for centralized configuration. |
| **Multi-Host Support** | Not natively (requires Docker Swarm or other tools). | Native support for managing clusters of many nodes. |
| **Learning Curve** | Easier to learn and use. | More complex with many concepts (Pods, Deployments, Services, etc.). |
| **Best Use Case** | Developing, testing, and running individual containers. | Managing containerized applications in production at scale. |

### Pod

A Pod is the smallest deployable unit in Kubernetes.

```
Application
    ↓
Container (Docker)
    ↓
Pod (Kubernetes)

```

Kubernetes does not deploy containers directly. It deploys Pods, and each Pod contains **one or more** containers.

**Why not deploy containers directly?**

Kubernetes needs something that can:

- Give containers an IP address.
- Attach storage.
- Define restart policies.
- Manage networking.
- Keep related containers together.

That "wrapper" is called a Pod.

A Pod can contain multiple containers

Sometimes two containers must always run together.

Example:
```
Pod
┌───────────────────────────────┐
│ Node.js App                   │
│                               │
│ Logging Agent                 │
└───────────────────────────────┘
```

Both containers:

- Share the same IP address.
- Share the same storage volumes.
- Can communicate using localhost.

If the Pod is deleted, **both containers are deleted together.**

### Kubernetes Architecture

Kubernetes Cluster = Control Plane (brain) + Worker Nodes (does the job)

A Kubernetes cluster is made up of two main parts:

- Control Plane (the brain)
- Worker Nodes (the workers)


#### Node:

- Node = one Virtual Machine
- Node has CPU, Memory, Network, Runtime (container runtime)
- Pods never run directly on cluster, they always run on a node
- If node dies, pods on that node also die.

#### Cluster:

- Cluster = control plane + one or more nodes
- Its a group of machines
- Cluster cab span 1 node or 1000 nodes
- You deploy apps to the cluster, not to nodes directly

```
                 Kubernetes Cluster
        ┌───────────────────────────────┐
        │        Control Plane          │
        │   (Manages the cluster)       │
        └──────────────┬────────────────┘
                       │
         ┌─────────────┼─────────────┐
         │             │             │
    Worker Node 1  Worker Node 2  Worker Node 3
         │             │             │
      Pods          Pods          Pods
         │             │             │
    Containers    Containers    Containers
```

#### 1. Control Plane

| Component              | Purpose                                                                                              |
| ---------------------- | ---------------------------------------------------------------------------------------------------- |
| **API Server**         | Receives all Kubernetes requests (from `kubectl` or other clients).                                  |
| **Scheduler**          | Decides which worker node should run each Pod.                                                       |
| **Controller Manager** | Continuously checks that the actual cluster matches the desired state (e.g., recreates failed Pods). |
| **etcd**                | Stores the cluster's configuration and current state (a distributed key-value database).             |


#### 2. Worker Node

| Component             | Purpose                                                                |
| --------------------- | ---------------------------------------------------------------------- |
| **Kubelet**           | Talks to the Control Plane and ensures Pods are running as instructed. |
| **Container Runtime** | Runs the containers (e.g., containerd).                                |
| **Kube Proxy**        | Handles networking and routes traffic to Pods.                         |


#### What happens when we run `kubectl apply -f app.yml` ?


```
        ┌───────────────────────────────┐
        │          API Server           │
        │   Receives the request        │
        └──────────────┬────────────────┘
                       │
                       ▼
        ┌───────────────────────────────┐
        │             etcd              │
        │  Stores the desired state     │
        └──────────────┬────────────────┘
                       │
                       ▼
        ┌───────────────────────────────┐
        │          Scheduler            │
        │ Chooses the Worker Node       │
        └──────────────┬────────────────┘
                       │
                       ▼
        ┌───────────────────────────────┐
        │           Kubelet             │
        │ Creates the Pod on the        │
        │ selected Worker Node          │
        └──────────────┬────────────────┘
                       │
                       ▼
        ┌───────────────────────────────┐
        │      Container Runtime        │
        │ Starts the Container(s)       │
        └──────────────┬────────────────┘
                       │
                       ▼
        ┌───────────────────────────────┐
        │      Controller Manager       │
        │ Watches the cluster and       │
        │ fixes any differences         │
        └───────────────────────────────┘
```

#### Deployment

A Deployment is a Kubernetes object that tells Kubernetes how many Pods you want and how they should be managed.

Instead of creating Pods manually, you create a Deployment.

#### Setting up K8s locally

There are many ways to setup kubernestes locally:
- Docker Desktop
- Minikube
- Kind

We will use docker desktop. Open Docker Desktop, go to Settings and enable Kubernetes.

Commands:

- `kubectl get nodes`: lists the available worker nodes

- `kubectl get pods`: display running pods

- `kubectl create deployment web --image=nginx`: 

  Creates deployment called "web" that runs the image nginx. Actual step: kubernetes creates deployment (web) -> deployment creates pod -> pod creates container -> container runs the image


- `kubectl expose derployment web --type=NodePort --port=80`: Expose the deployment using a service

- `kubectl get service`: lists all the service

- `kubectl get service web`: shows only service web

- `kubectl delete pod web`: deletes a pod

  When we run this command then this deletes a pod, but kubernetes will recreate the pod agaibn. It is because when we created the deployemtn then by default it was taken as 1 pod will always exist. So whern this pod gets deleted then another gets created immediately.

- `kubectl get pods -w`: It dispays the pods in watch mode. It shows live updates like pods creating, termination, running.

- `kubectl get rs`: Command to display the replica sets
- `kubectl describe rs_name`: Complete detail of a replica set


    

#### Service in K8s

Without a Service:
- Pod is not accessibgle
- App is invisible outside the cluster
- Everytime pod restarts, new IP address is assigned & old connection breaks.

What Services does:
- Gives your app a permanent IP
- Lets other apps inside the cluster talk to it
- Lets you expose it to the browser(NodePort)

Service="Give my app a stable address so browsers and apps can reach it"

Service Types:
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName

##### ClusterIP

It exposes app inside the cluster only, won't be accessible from browser. Useful when backend talking to backend & Microservices communication.

**Accesible from:** Inside cluster<br>
**When to use:** Microservices

##### NodePort
Exposes app on a node's IP + port. Makes app accessible from browser. Port range is of **30000-32767**. Used generally for local testing, demos and learning kubenetes.

**Accesible from:** Browser (IP:PORT)<br>
**When to use:** Local/Learning

##### LoadBalancer
Creates a cloud load balancer and gives public IP/DNS. Works with cloud services like AWS, GCP, Azure. Used majorly for prodcution apps with real users.

**Accesible from:** Public Internet<br>
**When to use:** Production

##### ExternalName

Maps Services to an external DNS. Used for external db or third party API

**Accesible from:** External DNS<br>
**When to use:** Outside Service


Summary:

**Deployment** -> created & manage Pods<br>
**Pod**-> runs your app (nginx)<br>
**Service** -> gives stable conection to the Pod<br>
**NodePort** -> lets you access the Service from your browser


**Quick Practice:**
Create a deployment for nginx and access it locally.


1. Create deployment

    `kubectl creater deployment web --image=nginx`

2. Expose the deployment using a service
 
    `kubectl expose deployment web --type=NodePort --port=80`

    It exposes the port 80 of the container as nginx runs on it.

3. Check the service

    `kubectl get service` or `kubectl get svc`

    this will show the service and also will show the port mapping that is the port you can use on your browser to access nginx.


#### Replica Sets - Self Healing in Kubernetes

When a pod gets deleted then Kubernetes automatically creates new pod so that desired state that developer has specified is maintained. 
This happens because of replica sets.

And when we create a deployment, then replica set is created by default with desired stats as 1.

Command to display the replica sets: `kubectl get rs` or `kubectl get replicaset`<br>
Complete detail of a replica set: `kubectl describe rs rs_name`

How to scale the pods?

By default there is 1 replica, we can use this command to update it.

`kubectl scale deployment web --replicas=5`

Check the replica set: `kubectl get rs pod_name`

Before:
```
NAME             DESIRED   CURRENT   READY   AGE
web-7c56dcdb9b   1         1         1       4m46s
```

After --replicas=5
```
NAME             DESIRED   CURRENT   READY   AGE
web-7c56dcdb9b   5         5         5       4m46s
```

#### Rolling Updates with Zero Downtime

Lets, say we want to update the nginx image version that our deployment is currenlty using. For this we will use this command: 

`kubectl set image deployment/web nginx=nginx:latest`

Here:
- deployment/web → Deployment name
- nginx → Container name inside the Pod
- nginx:latest → New image

If you don't know the container name, 

Run:
`kubectl describe deployment web`

You wil see:
```
Containers:
  nginx:
    Image: nginx
```
Here, the container name is `nginx.`

When we do this the kubernetes performa a rolling update:
```
Old Pod
│
├── nginx:1.25
│
▼
Create New Pod
│
├── nginx:1.27
│
▼
Delete Old Pod
```
Therefore, the application stauys available during the update.

To check the rollout status: 

`kubectl rollout status deployment/web`

##### What is a Label?

A label is simply a key-value pair attached to a Kubernetes object.

```yaml
labels:
  app: web
```

To checxk the labels of pods use this command: `kubectl get pods --show-labels`

Output:
```
NAME                   READY   STATUS    RESTARTS   AGE   LABELS
web-6cd89c8677-24f6c   1/1     Running   0          15m   app=web,pod-template-hash=6cd89c8677
```

> **Note:** `app=web` is **not** a default Kubernetes label. It was automatically generated by `kubectl create deployment web --image=nginx`. In your own YAML files, you can choose any label you want.

Both **Deployments** and **Services** use these labels to identify Pods.

- A **Deployment** uses a **selector** (e.g., `app=web`) to determine **which Pods it should manage**.
- A **Service** uses a **selector** (e.g., `app=web`) to determine **which Pods should receive network traffic**.

To check Deployment, service or pod as yaml:

`kubectl get deployment web -o yaml`

`kubectl get service web -o yaml`

Everything in Kubernetes is an object, so we can get YAML for them.

#### Debug Pods

`kubectl logs pod_name`: to get logs

`kubectl logs pod_name -f`: to get live logs

`kubectl exec -it pod_name -- sh`: to get inside the pod

#### Creating Deployment.yaml and Service.yaml files

**Deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: frontend

spec:
  replicas: 2

  selector:
    matchLabels:
      app: frontend

  template:
    metadata:
      labels:
        app: frontend

    spec:
      containers:
        - name: frontend
          image: ritik48/ms-frontend
          ports:
            - containerPort: 80
```

- **kind**: tells Kubernetes what object you want to create. Like Pods, Deployment, Service

Note:

```yaml
selector:
  matchLabels:
    app: frontend
```

```yaml
labels:
  app: frontend
```
Here, selector and labels both should match because that's how the deployment knows which Pods belong to it.

And, `app: frontend` this key value paid can be anything. It can also `be something: fun`,  its not required to use `app`


**Service.yaml**

```yaml
apiVersion: v1
kind: Service

metadata:
  name: frontend

spec:
  selector:
    app: frontend

  ports:
    - port: 80
      targetPort: 3000
      nodePort: 30080
  
  type: NodePort
```

In Service, here we are using the NodePort, so tghat browser can access it.

Why different ports ?

**port: 80**<br>
This is the port exposed by the Service. Other Pods inside the cluster will connect to:`service-name:80`

**targetPort**<br>
This is the port on the Pod/container where the Service forwards traffic.

**nodePort**<br>
This is to allow traffic from oputside. hat is on the browser user has to go to this port.

So the request flow is:

```
Browser
    │
    │ 30080
    ▼
NodePort
    │
    │ 80
    ▼
Service
    │
    │ 3000
    ▼
Pod (Node app)
```
Where:

**nodePort (30080)** → External port on the Kubernetes node.<br>
**port (80)** → Port exposed by the Service inside the cluster.<br>
**targetPort (3000)** → Port on the Pod/container that receives the traffic.

- How to apply the files ?

  `kubectl apply -f deployment.yaml`

  `kubectl apply -f service.yaml`

  or `kubectl apply -f deployment.yaml -f service.yaml`

  or we can also do: `kubectl apply -f .` This applies the yaml that is present in the current folder.

- Deleting the deployments and service:

  `kubectl delete -f service.yaml`




**Adding environment variables to the container**

```yaml
    spec:
      containers:
        - name: frontend
          image: ritik48/ms-frontend
          env:
            - name: Backend
              value: something
            - name: database
              value: something_db
          ports:
            - containerPort: 80
```

But, this is not the preferred way as here we are hardcoding the envs in the deployment yaml file.
Instead of this, kubernetes provides **ConfigMap and Secret** to manage this.

#### ConfigMap & Secret

ConfigMap and Secret are both used to provide configuration to your Pods without hardcoding it into the Docker image.

1. Configmap: 
  - It is used to store the non-sensitive environment variables, API URLs, feature flags, app settings.
  - Stored as plain text.


config.yaml
  ```yaml
  apiVersion: v1
  kind: ConfigMap

  metadata:
    name: sample-config
  
  data:
    BACKEND_MESSAGE: "Hello world"
  ```
and we can check the available config maps like this: `kubectl get configmaps`

2. Secret
  - It is used to store sensitive environment variables, Passwords, API keys, tokens, certificates.
  - Stored as Base64-encoded values (not encryption).

Lets add en: `PASSWORD = Admin@123`, but first we have to convert it to base64 which is `QWRtaW5AMTIz`

secret.yaml
  ```yaml
  apiVersion: v1
  kind: Secret

  metadata:
    name: sample-secret
  
  type: Opaque
  
  data:
    PASSWORD: QWRtaW5AMTIz
  ```

  Now, we have to apply both the ConfigMap and Secret.

  `kubectl apply -f config.yaml -f secret.yaml`

  Use them in the Deployment.yaml:

  ```yaml
    spec:
      containers:
        - name: frontend
          image: ritik48/ms-frontend
          env:
            - name: BACKEND_MESSAGE_ENV 
              valueFrom: 
                configMapKeyRef:
                  name: sample-config
                  key: BACKEND_MESSAGE 
            - name: PASSWORD
              valueFrom:
                secretKeyRef:
                  name: sample-secret
                  key: PASSWORD
          ports:
            - containerPort: 80
```

Here, 

- `sample-config`: is the ConfigMap name. 
- `BACKEND_MESSAGE_ENV`: the environment variable name that gets set inside your container.
- `BACKEND_MESSAGE`: the key inside your ConfigMap's data

To load all the nev form configmap:


  ```yaml
    spec:
      containers:
        - name: frontend
          image: ritik48/ms-frontend
          envFrom:
            - configMapRef:
                name: config-map
          ports:
            - containerPort: 80
```


#### Kubernetes Ingress

What is Ingress?

Ingress is a Kubernetes resource that exposes HTTP/HTTPS Services to users outside the cluster. It acts as a **single entry** point for multiple Services.

**Without Ingress**

We can use service but this way we will have multiple loadbalancers per service. For example, in out microservice application we can have services like:
- backend
- frontend
- payment

So, for all this three we would need Service LoadBalancer. So, eveyones ip is different. 
Like:
- frontend: frontend.company.com
- backend: backend.company.com
- payment: payment.company.com

Problems:
- One Load Balancer per Service
- Higher cloud costs
- Multiple public IPs/DNS names
- Harder to manage

And this is also costly, as we would need three static ips for 3 load balancers.

**With Ingress**

With this we would just need one LoadBalancer, which will route the request to different routes based on the configuration we specify.

```
Internet
    │
    ▼
LoadBalancer
    │
    ▼
Ingress Controller
    │
 ┌──┴──────────┐
 ▼             ▼
Frontend     Backend
Service       Service
```

Benefits:
- One public IP
- One domain
- Lower cost
- Easier routing

**Ingress controller**

Ingress Controller is the actual running software that reads Ingress rules and handles real HTTP traffic routing.

Ingress controller:
- Runs as Pods
- Is managed by a Deployment
- Exposed via a Service

Popular controllers:

- NGINX Ingress Controller
- Traefik
- HAProxy
- AWS Load Balancer Controller


With Ingress we get different routing startegies like:

1. Path-based routing

    ```
    myapp.com/          → frontend-service

    myapp.com/api       → backend-service

    myapp.com/admin     → admin-service
    ```

2. Host-based routing

      ```
      shop.example.com      → shop-service

      api.example.com       → api-service

      admin.example.com     → admin-service
      ```

**Ingress resource yaml:**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: myapp-ingress

spec:
  rules:
    - host: myapp.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend-service
                port:
                  number: 80

          - path: /api
            pathType: Prefix
            backend:
              service:
                name: api-service
                port:
                  number: 80

          - path: /admin
            pathType: Prefix
            backend:
              service:
                name: admin-service
                port:
                  number: 80

```

**Ingress with Regex & Reqriting Rules using NGINX Controller**

This ingress does the following:
- Receives: GET `/api/users`
- Checks rules (top to bottom)
- Matches: `/api/(.*)`
- Captures: `"users"` in `$1`
- Applies reqrite-target: `/$1`
- Transforms to: GET `/users`
- Forward to: `service-a:80`

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: myapp-ingress
  annotations:
    # Required for regex paths
    nginx.ingress.kubernetes.io/use-regex: "true"

    # Rewrite path using capture groups
    nginx.ingress.kubernetes.io/rewrite-target: /$1

spec:
  ingressClassName: nginx
  rules:
    - host: myapp.com
      http:
          - path: /api/(.*)
            pathType: Prefix
            backend:
              service:
                name: api-service
                port:
                  number: 80

          - path: /(.*)
            pathType: Prefix
            backend:
              service:
                name: frontend-service
                port:
                  number: 80


```

Now we have to do, `kubectl apply -f ingress.yml`

But, this doesn't mean it will start working. First, we need to have ingress controller installed only after that this ingress resource will be used by ingress controller and will work.

#### check all the configured clusters

`kubectl config get-contexts`
```
CURRENT   NAME             CLUSTER          AUTHINFO         NAMESPACE
*         docker-desktop   docker-desktop   docker-desktop

```
> The * means this is the cluster kubectl is currently using.

#### Switch to another cluster

`kubectl config use-context <context-name>`