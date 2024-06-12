
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [🙋 What initial steps should be taken to verify the Kubernetes setup on Mac OS and Windows?**](#-what-initial-steps-should-be-taken-to-verify-the-kubernetes-setup-on-mac-os-and-windows)
    - [Kubernetes general Architecture.](#kubernetes-general-architecture)
    - [Components of Kubernetes local setup](#components-of-kubernetes-local-setup)
- [🙋  What are the key differences between Docker Compose and Kubernetes in terms of image handling, container creation, and networking?**](#--what-are-the-key-differences-between-docker-compose-and-kubernetes-in-terms-of-image-handling-container-creation-and-networking)
  - [Key Differences Between Docker Compose and Kubernetes](#key-differences-between-docker-compose-and-kubernetes)
- [🙋  How do we translate Docker Compose concepts into Kubernetes, and what are the necessary steps for deploying a container?**](#--how-do-we-translate-docker-compose-concepts-into-kubernetes-and-what-are-the-necessary-steps-for-deploying-a-container)
- [🙋  What are the specific tasks required to get the multi-client image running on Kubernetes?**](#--what-are-the-specific-tasks-required-to-get-the-multi-client-image-running-on-kubernetes)
- [🙋  What is the significance of the `kind` property in Kubernetes configuration files?**](#--what-is-the-significance-of-the-kind-property-in-kubernetes-configuration-files)
- [🙋  What are Kubernetes objects and what roles do they play in a cluster?**](#--what-are-kubernetes-objects-and-what-roles-do-they-play-in-a-cluster)
- [🙋 How do the API version and kind properties interact in a Kubernetes configuration file?**](#-how-do-the-api-version-and-kind-properties-interact-in-a-kubernetes-configuration-file)
- [🙋 What does `kind: Pod` mean in Kubernetes?](#-what-does-kind-pod-mean-in-kubernetes)
    - [Detailed Explanation:](#detailed-explanation)
- [🙋 In-Depth Explanation of `client-pod.yaml`](#-in-depth-explanation-of-client-podyaml)
    - [Full YAML File:](#full-yaml-file)
    - [Breakdown:](#breakdown)
  - [Summary](#summary)
- [🙋 Explain Service kind.](#-explain-service-kind)
  - [Kubernetes Service Kind](#kubernetes-service-kind)
  - [Types of Services](#types-of-services)
  - [Summary of Service Types](#summary-of-service-types)
- [🙋 Explain client-node-port.yaml](#-explain-client-node-portyaml)
  - [In-Depth Explanation of `client-node-port.yaml`](#in-depth-explanation-of-client-node-portyaml)
    - [Full YAML File:](#full-yaml-file-1)
    - [Breakdown:](#breakdown-1)
  - [Summary](#summary-1)
  - [Visual Representation](#visual-representation)
- [🙋 What are the `kubectl apply` and `kubectl get` commands in Kubernetes, and how are they used?**](#-what-are-the-kubectl-apply-and-kubectl-get-commands-in-kubernetes-and-how-are-they-used)
    - [`kubectl apply`](#kubectl-apply)
    - [`kubectl get`](#kubectl-get)
    - [Summary](#summary-2)
- [🙋 Why `<minikube_ip>:<nodeport>` has to used  and explain why `localhost:<nodeport>` cannot be used to access the service on k8s cluster?](#-why-minikube_ipnodeport-has-to-used--and-explain-why-localhostnodeport-cannot-be-used-to-access-the-service-on-k8s-cluster)
- [🙋 Explain kubectl apply deployment workflow.](#-explain-kubectl-apply-deployment-workflow)
  - [`kubectl` Deployment Workflow Based on the Image](#kubectl-deployment-workflow-based-on-the-image)
- [🙋 Declarative and Imperative deployments](#-declarative-and-imperative-deployments)
- [🙋 Why `NodePort` is Used Only in Development Environments and Not in Production?](#-why-nodeport-is-used-only-in-development-environments-and-not-in-production)
    - [Example Comparison:](#example-comparison)
  - [Summary](#summary-3)

<!-- /code_chunk_output -->


---
## 🙋 What initial steps should be taken to verify the Kubernetes setup on Mac OS and Windows?**

- **A:** Open the terminal and run `Minikube status` to check if Minikube and the cluster are running, and `kubectl` is correctly configured. Also, run `kubectl cluster-info` to ensure the cluster information is displayed correctly. If there are any warnings or errors, refer to the QA section for troubleshooting.

#### Kubernetes general Architecture.
![kubernetes general architecture](/Kubernetes/assets/001/images/Kubernetes_general_architecture.png)

#### Components of Kubernetes local setup
![local_setup_components](/Kubernetes/assets/001/images/Local_component_setup.png)
## 🙋  What are the key differences between Docker Compose and Kubernetes in terms of image handling, container creation, and networking?**

- **A:**
### Key Differences Between Docker Compose and Kubernetes

| Feature               | Docker Compose                                               | Kubernetes                                                           |
|-----------------------|--------------------------------------------------------------|----------------------------------------------------------------------|
| **Image Handling**    | Images can be built as part of the Docker Compose process.   | All images must be pre-built and ready for deployment. No build process within Kubernetes. |
| **Container Creation**| Uses a single file to define and create containers.          | Uses multiple configuration files to create different objects, not just containers.          |
| **Networking**        | Simplifies networking and port mapping with straightforward entries in the configuration file. | Requires manual setup for networking and port mapping.               |
## 🙋  How do we translate Docker Compose concepts into Kubernetes, and what are the necessary steps for deploying a container?**

- **A:**
    ![Docker_compose_vs_kubernetes](/Kubernetes/assets/001/images/Docker_compose_vs_kubernetes.png)

## 🙋  What are the specific tasks required to get the multi-client image running on Kubernetes?**

- **A:**
![Kubernetes_deployment_overview](/Kubernetes/assets/001/images/Kubernetes_to_cluster.png)


## 🙋  What is the significance of the `kind` property in Kubernetes configuration files?**

- **A:** The `kind` property in a Kubernetes configuration file specifies the type of object being created. Each object type in Kubernetes has a distinct purpose, such as running a container (Pod) or setting up networking (Service).

## 🙋  What are Kubernetes objects and what roles do they play in a cluster?**

- **A:** Kubernetes objects are entities that represent the state and configuration of various components in a Kubernetes cluster. Examples of objects include Pods, Services, StatefulSets, and ReplicaSets. Each type of object has a specific role:
  - **Pod:** Runs one or more containers.
  - **Service:** Manages networking and makes a set of Pods accessible.
  - **StatefulSet:** Manages stateful applications.
  - **ReplicaSet:** Ensures a specified number of replicas of a Pod are running.
![Object_types](/Kubernetes/assets/001/images/object_types.png)

## 🙋 How do the API version and kind properties interact in a Kubernetes configuration file?**

- **A:** The `apiVersion` property determines the API group and version under which the objects can be created, while the `kind` property specifies the type of object. For example, `apiVersion: v1` allows for the creation of objects like Pods and Services. If a different API version is used, such as `apps/v1`, it allows for the creation of different object types like StatefulSets and Deployments. The combination of `apiVersion` and `kind` dictates the available objects and their configurations.
![APIversion](/Kubernetes/assets/001/images/apiVersion.png)


## 🙋 What does `kind: Pod` mean in Kubernetes?

- **A:** In Kubernetes, the `kind: Pod` declaration within a configuration file signifies that the file is defining a Pod object. A Pod is the smallest and simplest Kubernetes object that you can create and manage. It represents a single instance of a running process in your cluster and can contain one or more containers that share storage, network, and a specification on how to run.

#### Detailed Explanation:

1. **Basic Unit of Deployment:**
   - A Pod is the basic unit of deployment in Kubernetes. It encapsulates one or more application containers, along with storage resources, a unique network IP, and options that govern how the containers should run.

2. **Container Sharing:**
   - Containers within the same Pod share the same network namespace, including IP address and port space, and can communicate with each other using `localhost`. They also share storage volumes that can be mounted within the Pod.

3. **Lifecycle:**
   - Pods are designed to run a single instance of a process. When a Pod is created, Kubernetes schedules it to run on a Node in the cluster. If a Node fails, Kubernetes can reschedule the Pod on a different Node.

4. **Configuration Example:**
   - Here is a basic example of a Pod configuration file (`pod.yaml`):

     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: my-pod
     spec:
       containers:
       - name: my-container
         image: nginx:latest
         ports:
         - containerPort: 80
     ```

   - **Explanation of the YAML file:**
     - `apiVersion: v1`: Specifies the API version used to create this object.
     - `kind: Pod`: Indicates that the object being created is a Pod.
     - `metadata`: Contains metadata about the Pod, such as its name (`my-pod`).
     - `spec`: Defines the specifications of the Pod, including the containers it runs.
       - `containers`: Lists the containers within the Pod. In this case, there's a single container named `my-container` that uses the `nginx:latest` image.
       - `ports`: Specifies the ports that the container listens on, with `containerPort` set to 80.

5. **Use Cases:**
   - **Single Container Pod:** Often used for simple applications that do not require multiple containers. For example, a web server running in an Nginx container.
   ![single_container_pod](/Kubernetes/assets/001/images/singlecontainer_pod.png)

   - **Multi-Container Pod:** Useful for tightly coupled processes that need to share resources, such as a main application container and a sidecar container for logging.
   ![multi_container_pod](/Kubernetes/assets/001/images/multicontainer_pod.png)

6. **Advantages:**
   - **Simplified Networking:** Containers in a Pod share the same IP address, making it easier for them to communicate.
   - **Shared Storage:** Containers in a Pod can share volumes, allowing them to read and write to the same storage.
   - **Atomic Deployment:** Pods provide a way to deploy a group of containers that need to be managed as a single unit.

7. **Lifecycle Management:**
   - Kubernetes manages the lifecycle of Pods through controllers like Deployments, StatefulSets, and ReplicaSets, which ensure the desired number of Pods are running and handle scaling and updates.

## 🙋 In-Depth Explanation of `client-pod.yaml`

Let's break down each part of the provided `client-pod.yaml` file to understand its purpose and configuration in detail.

#### Full YAML File:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

#### Breakdown:

1. **apiVersion: v1**
   - **Description:** This specifies the version of the Kubernetes API that should be used to create this object. `v1` is the stable version for core resources such as Pods.
   - **Details:** The API version determines what resources and features are available. Using `v1` means we are utilizing the core stable API.

2. **kind: Pod**
   - **Description:** This indicates that the object being created is a Pod.
   - **Details:** In Kubernetes, `kind: Pod` tells the system that the configuration file defines a Pod, which is the smallest deployable unit that can be created and managed.

3. **metadata:**
   - **name: client-pod**
     - **Description:** The name of the Pod. This must be unique within the namespace where the Pod is created.
     - **Details:** `client-pod` is the identifier for this Pod. It allows Kubernetes users and tools to refer to this specific Pod.
   - **labels:**
     - **component: web**
       - **Description:** Labels are key-value pairs attached to objects. They are used to organize and select subsets of objects.
       - **Details:** The label `component: web` categorizes this Pod as part of the "web" component. Labels can be used for grouping and selecting Pods using label selectors.

4. **spec:**
   - **Description:** This defines the desired state of the Pod, including the containers that should run within it.
   - **Details:** The `spec` field contains the specifications of the Pod's contents and behavior.

5. **containers:**
   - **Description:** This specifies the list of containers that will run in the Pod.
   - **Details:** Each container has its own configuration, including the image it runs, ports it exposes, etc.

   - **name: client**
     - **Description:** The name of the container within the Pod.
     - **Details:** This name must be unique within the Pod. The container is named `client`, which helps in identifying and managing it.

   - **image: stephengrider/multi-client**
     - **Description:** The container image to use for this container.
     - **Details:** `stephengrider/multi-client` is a pre-built Docker image stored in a container registry (likely Docker Hub). This image contains the application's executable code and dependencies.

   - **ports:**
     - **Description:** This specifies the ports that the container will expose.
     - **Details:** This is a list of ports that are exposed by the container. In this case, the container exposes a single port.
       - **containerPort: 3000**
         - **Description:** The port on which the container will listen for incoming traffic.
         - **Details:** Port 3000 is commonly used for web applications, indicating that the application inside the container will listen for HTTP requests on this port.

### Summary

- **API Version:** Using `v1` ensures compatibility with core stable resources.
- **Kind:** Declares the creation of a Pod.
- **Metadata:** Names the Pod `client-pod` and labels it with `component: web`.
- **Spec:** Defines the desired state with a single container.
- **Container Details:** Specifies a container named `client` using the `stephengrider/multi-client` image and exposing port 3000.

This YAML file is a configuration that Kubernetes will use to deploy and manage a Pod running a containerized application. By defining the necessary attributes such as the image, ports, and metadata, it provides Kubernetes with all the information needed to manage the application's lifecycle within a cluster.

## 🙋 Explain Service kind.

![service types](/Kubernetes/assets/001/images/Service_types.png)

### Kubernetes Service Kind

In Kubernetes, a **Service** is an abstraction that defines a logical set of Pods and a policy by which to access them. Services enable loose coupling between dependent Pods. Kubernetes Services provide a stable network endpoint for a set of Pods and enable load balancing, service discovery, and decoupling of services.

### Types of Services

1. **ClusterIP (Default)**
   - **Description:** Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
   - **Use Case:** For internal communication between different services within the cluster.
   - **Example:**
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       selector:
         app: MyApp
       ports:
         - protocol: TCP
           port: 80
           targetPort: 9376
     ```

2. **NodePort**
   - **Description:** Exposes the Service on each Node's IP at a static port (the NodePort). A ClusterIP Service is automatically created and the NodePort Service routes traffic to the ClusterIP Service.
   - **Use Case:** For external access to the Service using `<NodeIP>:<NodePort>`.
   - **Example:**
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       type: NodePort
       selector:
         app: MyApp
       ports:
         - protocol: TCP
           port: 80
           targetPort: 9376
           nodePort: 30007
     ```

3. **LoadBalancer**
   - **Description:** Exposes the Service externally using a cloud provider's load balancer. A ClusterIP and NodePort Service are automatically created, and the LoadBalancer routes external traffic to the NodePort Service.
   - **Use Case:** For external access to the Service with a cloud-managed load balancer.
   - **Example:**
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       type: LoadBalancer
       selector:
         app: MyApp
       ports:
         - protocol: TCP
           port: 80
           targetPort: 9376
     ```

4. **ExternalName**
   - **Description:** Maps a Service to the contents of the `externalName` field (e.g., `foo.bar.example.com`), returning a CNAME record with its value. No proxying of any kind is set up.
   - **Use Case:** For Service discovery with an external DNS name.
   - **Example:**
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: my-service
     spec:
       type: ExternalName
       externalName: my.database.example.com
     ```

### Summary of Service Types

- **ClusterIP:** Internal access within the cluster.
- **NodePort:** External access using Node IP and static port.
- **LoadBalancer:** External access with cloud-managed load balancer.
- **ExternalName:** Maps to an external DNS name.

By using these different types of Services, Kubernetes can efficiently manage networking, load balancing, and service discovery within and outside the cluster.

## 🙋 Explain client-node-port.yaml

### In-Depth Explanation of `client-node-port.yaml`

Let's break down each part of the provided `client-node-port.yaml` file to understand its purpose and configuration in detail.

#### Full YAML File:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - port: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

#### Breakdown:

1. **apiVersion: v1**
   - **Description:** Specifies the version of the Kubernetes API that should be used to create this object. `v1` is the stable version for core resources such as Services.
   - **Details:** The API version determines what resources and features are available. Using `v1` means we are utilizing the core stable API.

2. **kind: Service**
   - **Description:** Indicates that the object being created is a Service.
   - **Details:** In Kubernetes, `kind: Service` tells the system that the configuration file defines a Service, which is used to expose an application running on a set of Pods as a network service.

3. **metadata:**
   - **name: client-node-port**
     - **Description:** The name of the Service. This must be unique within the namespace where the Service is created.
     - **Details:** `client-node-port` is the identifier for this Service. It allows Kubernetes users and tools to refer to this specific Service.

4. **spec:**
   - **Description:** This defines the desired state of the Service, including its type, the ports it will expose, and the selectors used to identify the Pods it should route traffic to.
   - **Details:** The `spec` field contains the specifications of the Service's contents and behavior.

5. **type: NodePort**
   - **Description:** Specifies the type of Service being created.
   - **Details:** `NodePort` is a type of Service that exposes the Service on a static port on each Node in the cluster. This allows external traffic to access the Service via the Node's IP address and the specified port.

6. **ports:**
   - **Description:** This specifies the list of ports that the Service will expose.
   - **Details:** This is a list of ports that are exposed by the Service. Each entry specifies a mapping between ports.

   - **port: 3050**
     - **Description:** The port that the Service will expose.
     - **Details:** Port 3050 is the port on which the Service will be accessible.

   - **targetPort: 3000**
     - **Description:** The port on the Pod that the traffic will be directed to.
     - **Details:** Target port 3000 is the port on the selected Pods that will receive the traffic sent to the Service's port 3050.

![port_understanding](/Kubernetes/assets/001/images/service_port_understanding.png)

   - **nodePort: 31515**
     - **Description:** The port on each Node that will route traffic to the Service.
     - **Details:** NodePort 31515 is the port on the Node that maps to the Service's port 3050. This allows external traffic to reach the Pods via the Node's IP address and this port.

7. **selector:**
   - **Description:** This defines the criteria used to select which Pods the Service routes traffic to.
   - **Details:** The selector specifies a label query used to identify the Pods that will receive the traffic.

   - **component: web**
     - **Description:** The label used to select Pods.
     - **Details:** The selector `component: web` matches Pods that have the label `component` with the value `web`. This ensures that the traffic is routed only to the Pods intended to handle it.

![selector_understanding](/Kubernetes/assets/001/images/selector_understanding.png)

### Summary

- **API Version:** Using `v1` ensures compatibility with core stable resources.
- **Kind:** Declares the creation of a Service.
- **Metadata:** Names the Service `client-node-port`.
- **Spec:** Defines the desired state with a `NodePort` type and port mappings.
- **Ports:** Maps Service port 3050 to Pod port 3000, and exposes it on Node port 31515.
- **Selector:** Routes traffic to Pods labeled with `component: web`.

### Visual Representation

To better understand the relationships, consider this visual representation:

```plaintext
External Client
       |
       | Access Node's IP on port 31515
       v
+------------------+
|    Kubernetes    |
|      Node        |
+------------------+
       |
       | Routes traffic from port 31515 to Service port 3050
       v
+------------------+
|    Service       |  (client-node-port)
|   port: 3050     |
+------------------+
       |
       | Forwards traffic to Pod's container port 3000
       v
+------------------+
|      Pod         |  (Matching label `component: web`)
| containerPort: 3000 |
|                  |
+------------------+
```

By understanding the structure and details of this YAML file, you can see how Kubernetes exposes an application running in Pods to external traffic using a NodePort Service. This configuration allows external clients to access the application through the specified Node port.

## 🙋 What are the `kubectl apply` and `kubectl get` commands in Kubernetes, and how are they used?**

#### `kubectl apply`
- **Description:** The `kubectl apply` command is used to create or update resources defined in configuration files. It applies the configuration specified in the file to the Kubernetes cluster.
- **Usage:** This command reads the configuration files (YAML or JSON) and applies the specified settings to create or update the objects in the cluster.

**Basic Syntax:**
```sh
kubectl apply -f <filename>
```

**Example:**
```sh
kubectl apply -f client-pod.yaml
kubectl apply -f client-node-port.yaml
```
- **Explanation:** These commands apply the configurations defined in `client-pod.yaml` and `client-node-port.yaml` to the Kubernetes cluster, creating or updating the specified Pods and Services.

**Common Options:**
- `-f, --filename`: Specifies the file or directory containing the configuration to apply.
- `-R, --recursive`: Recursively processes the directory and its sub-directories for configuration files.

**Advanced Usage:**
- Applying multiple files or a directory:
  ```sh
  kubectl apply -f config-directory/
  ```
- Applying resources defined in multiple files:
  ```sh
  kubectl apply -f client-pod.yaml -f client-node-port.yaml
  ```

#### `kubectl get`
- **Description:** The `kubectl get` command retrieves information about Kubernetes resources. It lists one or more resources and displays their details.
- **Usage:** This command provides a snapshot of the current state of the specified resources in the cluster.

**Basic Syntax:**
```sh
kubectl get <resource_type> [<resource_name>]
```

**Example:**
```sh
kubectl get pods
kubectl get services
kubectl get pod client-pod
kubectl get service client-node-port
```
- **Explanation:** 
  - `kubectl get pods`: Lists all Pods in the current namespace.
  - `kubectl get services`: Lists all Services in the current namespace.
  - `kubectl get pod client-pod`: Retrieves detailed information about the Pod named `client-pod`.
  - `kubectl get service client-node-port`: Retrieves detailed information about the Service named `client-node-port`.

**Common Options:**
- `-o, --output`: Specifies the output format (e.g., `json`, `yaml`, `wide`, `name`).
- `--watch`: Watches for changes in the specified resources and updates the output in real-time.
- `--all-namespaces`: Lists resources across all namespaces.

**Advanced Usage:**
- Retrieving detailed information in YAML format:
  ```sh
  kubectl get pod client-pod -o yaml
  ```
- Watching for changes to Pods:
  ```sh
  kubectl get pods --watch
  ```
- Listing resources across all namespaces:
  ```sh
  kubectl get pods --all-namespaces
  ```

#### Summary
- **`kubectl apply`**: Used to create or update resources from configuration files. It reads the files and applies the configurations to the Kubernetes cluster.
- **`kubectl get`**: Used to list and retrieve information about resources. It displays the current state and details of the specified resources in the cluster.

These commands are fundamental for managing and interacting with Kubernetes resources, enabling you to apply configurations and monitor the state of your applications efficiently.

## 🙋 Why `<minikube_ip>:<nodeport>` has to used  and explain why `localhost:<nodeport>` cannot be used to access the service on k8s cluster?
- `<minikube_ip>:<nodeport>`: This combination is used to access the service because it correctly points to the Minikube VM's IP address and the allocated NodePort where the service is exposed.
- `localhost:<nodeport>`: This cannot be used because localhost refers to the host machine's loopback interface, not the Minikube VM. The services running inside the Minikube VM are isolated and not directly accessible via localhost on the host machine.
- By using `<minikube_ip>:<nodeport>`, you ensure that you are correctly targeting the network environment where the Kubernetes services are running.

---
## 🙋 Explain kubectl apply deployment workflow.
![kubectl deployment workflow](/Kubernetes/assets/001/images/kubectl_deployment_workflow.png)

### `kubectl` Deployment Workflow Based on the Image

**Step-by-Step Workflow:**

1. **Preparation of Docker Images:**
   - **Docker Hub:** Store pre-built Docker images (`multi-worker`, `multi-client`, `multi-server`) on Docker Hub.

2. **Create Deployment File:**
   - **Deployment Specification:** Define a Deployment file indicating the desired state, such as the number of replicas. Example: Request 4 replicas of `multi-worker`.

3. **Apply Deployment:**
   - **kubectl apply:** Use the `kubectl apply -f <deployment-file>.yaml` command to submit the Deployment file to the Kubernetes cluster.

4. **kube-apiserver Processing:**
   - **Master Node:** The `kube-apiserver` on the Master node processes the Deployment request.
   - **Desired State:** Tracks the desired state (4 replicas of `multi-worker`).

5. **Scheduler and Nodes:**
   - **Node Allocation:** The Kubernetes Scheduler assigns Pods to Nodes based on resource availability.
   - **Pod Creation:** Each Node runs Docker to pull the `multi-worker` image from Docker Hub and create containers.

6. **Running Containers:**
   - **Active Containers:** The specified number of `multi-worker` containers (4 in this case) are running across the allocated Nodes.

This workflow ensures that the specified number of container replicas are running across the cluster, with Kubernetes managing the deployment and scaling automatically.

## 🙋 Declarative and Imperative deployments
![Declarative_vs_Imparative](/Kubernetes/assets/001/images/Declarative_imperative.png)

## 🙋 Why `NodePort` is Used Only in Development Environments and Not in Production?

#### Example Comparison:

| Feature                   | NodePort                                | LoadBalancer                        | Ingress                              |
|---------------------------|-----------------------------------------|-------------------------------------|--------------------------------------|
| **Port Range**            | 30000-32767                             | Standard HTTP/HTTPS ports           | Standard HTTP/HTTPS ports            |
| **User Experience**       | Non-standard URLs (e.g., `example.com:31515`)      | Friendly URLs (e.g., `example.com`) | Friendly URLs with path-based routing|
| **Security**              | Higher attack surface                   | Better security with cloud features | Advanced security options            |
| **Load Balancing**        | Basic routing                           | Advanced cloud load balancing       | Advanced routing and load balancing  |
| **SSL Termination**       | Not supported                           | Supported                           | Supported                            |
| **Scalability**           | Limited                                 | High                                | High                                 |

### Summary

`NodePort` is primarily used in development environments due to its simplicity and ease of use for testing purposes. However, its limitations in port range, user experience, security, and scalability make it unsuitable for production environments. Instead, production deployments typically use `LoadBalancer` services or `Ingress` controllers, which offer advanced features, better security, and more user-friendly access to applications.