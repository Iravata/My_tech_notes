
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [🙋 What does Kubectl match to update an object if there is a change in the config file?](#-what-does-kubectl-match-to-update-an-object-if-there-is-a-change-in-the-config-file)
- [🙋 What does `kubectl describe` do in Kubernetes?](#-what-does-kubectl-describe-do-in-kubernetes)
    - [Key Features of `kubectl describe`:](#key-features-of-kubectl-describe)
    - [Basic Syntax:](#basic-syntax)
    - [Example Output:](#example-output)
- [🙋 What is the limiation of kind pod in kubernetes?](#-what-is-the-limiation-of-kind-pod-in-kubernetes)
- [🙋 How are deployments better than pods explain?](#-how-are-deployments-better-than-pods-explain)
  - [Pods](#pods)
  - [Deployments](#deployments)
  - [Summary of Differences](#summary-of-differences)
- [🙋 Deployments have pod templates explain.](#-deployments-have-pod-templates-explain)
- [🙋 Explain Kubectl delete and its imperative nature to remove the objects.](#-explain-kubectl-delete-and-its-imperative-nature-to-remove-the-objects)
  - [Kubectl Delete in Kubernetes](#kubectl-delete-in-kubernetes)
  - [Key Points:](#key-points)
- [🙋 Explain Kubectl Get in Kubernetes](#-explain-kubectl-get-in-kubernetes)
  - [Common Examples:](#common-examples)
  - [Flags and Options:](#flags-and-options)
  - [Examples with Flags:](#examples-with-flags)
  - [Key Points:](#key-points-1)
  - [Custom Columns Example:](#custom-columns-example)
  - [Usage in Scripting and Automation:](#usage-in-scripting-and-automation)
  - [Example Script Usage:](#example-script-usage)
  - [Summary:](#summary)
- [🙋  Why Use Services Instead of Connecting Directly to Pods/Deployments in Kubernetes?](#--why-use-services-instead-of-connecting-directly-to-podsdeployments-in-kubernetes)
  - [Reasons for Using Services:](#reasons-for-using-services)
  - [Summary:](#summary-1)
- [🙋 How to Automatically Pull the Latest Image in Kubernetes Deployment?](#-how-to-automatically-pull-the-latest-image-in-kubernetes-deployment)
  - [Example Deployment Configuration](#example-deployment-configuration)
  - [Implementing Rolling Updates in CI/CD Pipeline](#implementing-rolling-updates-in-cicd-pipeline)
  - [Example Script in CI/CD Pipeline](#example-script-in-cicd-pipeline)
  - [Summary](#summary-2)
- [🙋 How to Imperatively Updating a Deployment Image with `kubectl set image` ?](#-how-to-imperatively-updating-a-deployment-image-with-kubectl-set-image-)
  - [Benefits of Using `kubectl set image`](#benefits-of-using-kubectl-set-image)
  - [Potential Downsides and Considerations](#potential-downsides-and-considerations)
  - [Best Practices](#best-practices)
  - [Summary](#summary-3)
- [Why to reconfigure docker CLI if using minikube on local setup (not for docker-desktop mac installations)?](#why-to-reconfigure-docker-cli-if-using-minikube-on-local-setup-not-for-docker-desktop-mac-installations)
  - [Reconfiguring Docker CLI with Minikube](#reconfiguring-docker-cli-with-minikube)
  - [How to Use `eval $(minikube docker-env)`](#how-to-use-eval-minikube-docker-env)
  - [Summary](#summary-4)

<!-- /code_chunk_output -->

---
## 🙋 What does Kubectl match to update an object if there is a change in the config file?

It uses Kind and Name as the unique identifier for an object.
![Object update config](/Kubernetes/assets/002/images/Updating_an_object.png)

**Original config file**
``` yaml
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

**Modified config file**
``` yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-worker #changed from client to worker
      ports:
        - containerPort: 3000
```

## 🙋 What does `kubectl describe` do in Kubernetes?

A: The `kubectl describe` command provides detailed information about a specific Kubernetes resource, such as a Pod, Service, Deployment, Node, or other objects. This command is useful for troubleshooting and understanding the current state and events related to the resource.

#### Key Features of `kubectl describe`:

1. **Detailed Information:**
   - Provides a comprehensive description of the resource, including its configuration, status, and events.
   - Displays information such as labels, annotations, resource limits, and detailed status.

2. **Events:**
   - Lists recent events related to the resource, such as creation, updates, and errors.
   - Useful for diagnosing issues and understanding the sequence of actions taken by Kubernetes.

3. **Nested Resources:**
   - Shows information about related nested resources, such as containers within a Pod, or the replicas managed by a Deployment.

#### Basic Syntax:

```sh
kubectl describe <resource_type> <resource_name>
```

- `<resource_type>`: The type of resource (e.g., pod, service, deployment).
- `<resource_name>`: The name of the specific resource.

#### Example Output:

**Pod Description:**

```sh
kubectl describe pod my-pod
```

**Sample Output:**

```plaintext
Name:         my-pod
Namespace:    default
Node:         node-1
Labels:       app=myapp
Annotations:  <none>
Status:       Running
IP:           10.1.1.1
Containers:
  my-container:
    Image:      my-image
    Port:       80/TCP
    Host Port:  0/TCP
    State:      Running
      Started:  Mon, 24 May 2021 10:00:00 +0000
    Ready:      True
    Restart Count:  0
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  1m    default-scheduler  Successfully assigned default/my-pod to node-1
  Normal  Pulled     1m    kubelet, node-1    Container image "my-image" already present on machine
  Normal  Created    1m    kubelet, node-1    Created container my-container
  Normal  Started    1m    kubelet, node-1    Started container my-container
```

**Explanation:**
- **Name, Namespace, Node:** Basic metadata about the Pod.
- **Labels, Annotations:** Metadata used for organizing and selecting the Pod.
- **Status, IP:** Current status and IP address of the Pod.
- **Containers:** Information about each container within the Pod, including its image, ports, state, and readiness.
- **Events:** Recent events related to the Pod, useful for understanding its lifecycle and troubleshooting issues.

## 🙋 What is the limiation of kind pod in kubernetes?
Very few configurations only can be updated

![pod_limitaion](/Kubernetes/assets/002/images/Pod_limitations.png)

## 🙋 How are deployments better than pods explain?
![Pods vs Deployments](/Kubernetes/assets/002/images/Pods_vs_deployments.png)

In Kubernetes, "pods" and "deployments" are fundamental concepts but serve different purposes and levels of abstraction. Here’s a detailed comparison:

### Pods
1. **Basic Unit**: 
   - A pod is the smallest and simplest Kubernetes object. It represents a single instance of a running process in your cluster.
   
2. **Composition**: 
   - A pod can contain one or more containers (usually, it contains a single container). All containers in a pod share the same network namespace, including IP address and port space, and can communicate with each other using `localhost`.
   
3. **Lifecycle**:
   - Pods are ephemeral. They are created, run, and terminated. When a pod dies, it is not resurrected; instead, a new pod is created to replace it (which may have a different IP address) only if managed by high level controllers like `Deployments`.

4. **Use Cases**:
   - Pods are typically used directly in very simple scenarios, such as running a single, non-replicated application.

### Deployments
1. **Higher-Level Abstraction**:
   - A deployment is a higher-level abstraction that manages a group of pods and provides declarative updates to them.

2. **Desired State Management**:
   - Deployments specify a desired state for your application, such as the number of replicas of the pod that should be running. The deployment controller ensures that the current state of your application matches the desired state.

3. **Rolling Updates and Rollbacks**:
   - Deployments support rolling updates, meaning you can update the application to a new version without downtime by gradually replacing old pods with new ones. If something goes wrong, deployments also support rolling back to previous versions.

4. **Scaling**:
   - Deployments make it easy to scale your application up or down by simply changing the number of replicas in the deployment configuration.

5. **Self-Healing**:
   - If a pod managed by a deployment fails or is deleted, the deployment controller automatically creates a new pod to replace it, ensuring the desired number of replicas is maintained.

### Summary of Differences

| Feature                | Pods                                     | Deployments                              |
|------------------------|------------------------------------------|------------------------------------------|
| Level of Abstraction   | Basic unit of deployment                 | Higher-level abstraction managing pods   |
| Composition            | One or more containers                   | A group of pods managed collectively     |
| Lifecycle Management   | Ephemeral, no automatic replacement      | Ensures the desired state, auto-replace  |
| Updates                | Manual                                   | Supports rolling updates and rollbacks   |
| Scaling                | Manual                                   | Declarative scaling                      |
| Self-Healing           | No                                        | Yes                                      |
| Use Case               | Simple, single-instance applications     | Scalable, resilient, and updatable apps  |

In essence, use pods when you need simple, single-instance applications, and use deployments for more complex, scalable, and resilient application management.

## 🙋 Deployments have pod templates explain.
![Pod templates in deployments](/Kubernetes/assets/002/images/Deployment_pod_templates.png)

``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-app
  labels:
    app: web
spec:
  replicas: 3 # tells deployment to run 3 pods matching the template
  selector:
    matchLabels:
      app: web # used to get a refrence for the pods once they are created, used to managage the pods
# Pod template similar to Pod configuration
  template: 
    metadata:
      labels:
        app: web # used to name the pods
    spec:
      containers:
      - name: web-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

## 🙋 Explain Kubectl delete and its imperative nature to remove the objects.

### Kubectl Delete in Kubernetes

**Definition:**
- `kubectl delete` is a command used to delete resources in a Kubernetes cluster, such as pods, services, deployments, etc.

**Usage:**
- `kubectl delete` can be used with different resource types and names, and can also target resources defined in YAML or JSON files.

**Examples:**
```sh
# Delete a pod
kubectl delete pod my-pod

# Delete a deployment
kubectl delete deployment my-deployment

# Delete resources defined in a file
kubectl delete -f resource.yaml
```

**Why `kubectl delete` is Imperative:**
1. **Direct Action:** 
   - It performs an immediate action to delete the specified resource(s).
   - The command explicitly states "delete this resource now."

2. **No Desired State Definition:**
   - Unlike declarative commands (e.g., `kubectl apply`), which describe the desired state, `kubectl delete` does not describe a desired end state for the resource.
   - It does not check or reconcile the state of the cluster to ensure resources match a defined state; it simply removes them.

3. **One-Time Operation:**
   - `kubectl delete` operates once per command execution.
   - There is no ongoing management or state reconciliation for deleted resources after the command is executed.

### Key Points:
- **Imperative commands** are useful for performing immediate, ad-hoc operations.
- **Declarative configuration** is better for managing and maintaining the desired state of the entire cluster over time.
- `kubectl delete` is often used in scenarios where you need to quickly remove resources without maintaining any desired state.

## 🙋 Explain Kubectl Get in Kubernetes

**Definition:**
- `kubectl get` is a command used to list resources in a Kubernetes cluster, providing information about the current state of those resources.

**Usage:**
- It can be used to retrieve details about various resources like pods, services, deployments, nodes, and more.

**Basic Syntax:**
```sh
kubectl get [resource_type] [resource_name] [flags]
```

### Common Examples:
```sh
# List all pods in the default namespace
kubectl get pods

# List all services in the default namespace
kubectl get services

# List all deployments in the default namespace
kubectl get deployments

# List all nodes in the cluster
kubectl get nodes

# List all pods in a specific namespace
kubectl get pods -n my-namespace

# Get detailed information about a specific pod
kubectl get pod my-pod -o wide
```

### Flags and Options:
- **Namespace:** Use `-n` or `--namespace` to specify a namespace.
- **Output Format:** Use `-o` to define the output format (e.g., `json`, `yaml`, `wide`).
- **Labels:** Use `-l` or `--selector` to filter resources by labels.

### Examples with Flags:
```sh
# List all pods in JSON format
kubectl get pods -o json

# List all services in YAML format
kubectl get services -o yaml

# List all pods with detailed information
kubectl get pods -o wide

# List all pods with a specific label
kubectl get pods -l app=my-app

# List all resources across all namespaces
kubectl get pods --all-namespaces
```

### Key Points:
- **Current State:** `kubectl get` provides a snapshot of the current state of the specified resources.
- **Multiple Resource Types:** You can list multiple resource types by separating them with commas (e.g., `kubectl get pods,services`).
- **Custom Columns:** Use `-o custom-columns` to define custom output columns for more specific information.

### Custom Columns Example:
```sh
# Custom output with specific columns
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
```

### Usage in Scripting and Automation:
- **Parsing Output:** The `-o json` or `-o yaml` formats are particularly useful for scripting and automation, as they allow easy parsing of the command output.

### Example Script Usage:
```sh
# Get all pod names in a namespace and process them in a loop
for pod in $(kubectl get pods -n my-namespace -o jsonpath='{.items[*].metadata.name}'); do
  echo "Processing pod: $pod"
done
```

### Summary:
- `kubectl get` is a versatile command for retrieving and listing resources in a Kubernetes cluster, offering various options for filtering and formatting the output to suit different needs. It is essential for monitoring and debugging the state of the cluster.

## 🙋  Why Use Services Instead of Connecting Directly to Pods/Deployments in Kubernetes?

**Definition:**
- **Service:** An abstraction in Kubernetes that defines a logical set of pods and a policy by which to access them. Services provide a stable IP address and DNS name for a set of pods.

### Reasons for Using Services:

1. **Pod Ephemerality:**
   - Pods are ephemeral and can be created, destroyed, or rescheduled at any time. Their IP addresses are not static.
   - Services provide a stable IP address and DNS name that persist even as the underlying pods change.

2. **Load Balancing:**
   - Services distribute traffic across multiple pods, ensuring load balancing.
   - This improves availability and reliability by preventing any single pod from being overwhelmed with traffic.

3. **Decoupling:**
   - Services decouple the application logic from the infrastructure details.
   - Applications can reference a service without needing to know about individual pod IP addresses.

4. **Discovery and Routing:**
   - Kubernetes has built-in mechanisms for service discovery.
   - Services enable automatic discovery and routing to the correct set of pods, simplifying the architecture.

5. **Scalability:**
   - Services support scaling up or down the number of pods without impacting clients.
   - As pods are added or removed, the service maintains consistent access to them.

6. **Network Policies:**
   - Services enable the implementation of network policies to control traffic flow within the cluster.
   - This enhances security by allowing fine-grained control over communication between different parts of the application.

### Summary:
Using services in Kubernetes abstracts away the dynamic nature of pods, providing stable access, load balancing, scalability, and enhanced security. This abstraction simplifies application deployment and management, ensuring reliable and efficient communication within the cluster.

## 🙋 How to Automatically Pull the Latest Image in Kubernetes Deployment?

To automatically pull the latest image in a Kubernetes deployment without updating the config file, you can use the following strategies:

1. **Set ImagePullPolicy to Always:**
   - Ensure that the `imagePullPolicy` is set to `Always`. This forces Kubernetes to always pull the image from the repository every time a pod is started.

2. **Image Tagging:**
   - Use a specific tag like `latest`, though it’s generally recommended to use a unique tag for each build to avoid issues with caching and ensure the correct version is deployed.

3. **Rolling Updates with CI/CD:**
   - Integrate a Continuous Integration/Continuous Deployment (CI/CD) pipeline to trigger rolling updates whenever a new image is pushed to the registry.

### Example Deployment Configuration

Here is an example deployment configuration that sets the `imagePullPolicy` to `Always`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-repo/my-image:latest
        imagePullPolicy: Always
```

### Implementing Rolling Updates in CI/CD Pipeline

To ensure your Kubernetes deployment pulls the latest image automatically, you can follow these steps:

1. **Build and Push Image:**
   - Configure your CI/CD pipeline to build the Docker image and push it to the registry with a unique tag.

2. **Trigger Rolling Update:**
   - Use `kubectl rollout restart` to trigger a rolling update of your deployment. This command will restart the pods, pulling the latest image.

### Example Script in CI/CD Pipeline

Here is an example script you might include in your CI/CD pipeline:

```sh
# Build the Docker image
docker build -t my-repo/my-image:latest .

# Push the Docker image to the repository
docker push my-repo/my-image:latest

# Trigger a rolling update in Kubernetes
kubectl rollout restart deployment/my-deployment
```

### Summary

- **Set `imagePullPolicy` to `Always`:** Ensures the latest image is pulled each time a pod is started.
- **Use CI/CD Pipeline:** Automate the build, push, and deployment process.
- **Rolling Updates:** Use `kubectl rollout restart` to trigger updates and ensure the latest image is used.

By following these strategies, you can automate the process of pulling the latest image in a Kubernetes deployment without manually updating the configuration file each time.

## 🙋 How to Imperatively Updating a Deployment Image with `kubectl set image` ?

**Definition:**
- `kubectl set image` is a command used to update the image of containers in a Kubernetes deployment, daemonset, or statefulset imperatively.

**Usage:**
- This command directly updates the image of the specified container(s) within the deployment without the need to edit the configuration file manually.

**Syntax:**
```sh
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
```

**Example:**
```sh
# Update the image of the container named 'my-container' in the deployment 'my-deployment' to 'my-repo/my-image:latest'
kubectl set image deployment/my-deployment my-container=my-repo/my-image:latest
```

### Benefits of Using `kubectl set image`

1. **Simplicity:**
   - Quick and straightforward to update the image without needing to edit and apply a configuration file.
   
2. **Immediate Effect:**
   - Changes take effect immediately, which is useful for urgent updates or fixes.
   
3. **Less Error-Prone:**
   - Reduces the chance of errors that might occur while manually editing a YAML file.

### Potential Downsides and Considerations

1. **Imperative Nature:**
   - **Lack of Version Control:** Changes made imperatively are not tracked in version control systems.
   - **Inconsistency:** Imperative updates can lead to inconsistencies if not documented properly, as the deployment configuration file does not reflect the change.

2. **Reproducibility:**
   - Declarative configurations allow for easier reproducibility and rollback, which can be more challenging with imperative commands.

3. **Documentation and Auditing:**
   - Declarative changes are usually better documented and auditable, as they are typically stored in configuration files.

### Best Practices

1. **Combine with Version Control:**
   - If using `kubectl set image` for a quick fix, ensure the deployment configuration file is updated accordingly and committed to version control.

2. **Automation:**
   - Use CI/CD pipelines to automate image updates, which can trigger declarative updates using `kubectl apply` with a configuration file.

3. **Rollback Strategy:**
   - Have a strategy in place for rolling back changes if something goes wrong. This is easier with declarative configurations.

### Summary

- **`kubectl set image`** is a useful command for quickly and imperatively updating the image of containers in a Kubernetes deployment.
- **Pros:** Simplicity, immediate effect, reduced error-prone.
- **Cons:** Lack of version control, potential inconsistencies, less reproducibility.
- **Best Practices:** Use in combination with version control, automate with CI/CD, have a rollback strategy.

Using `kubectl set image` is not inherently bad, but for long-term maintainability, consistency, and auditability, a declarative approach is generally recommended.

## Why to reconfigure docker CLI if using minikube on local setup (not for docker-desktop mac installations)?

![Multidocker_CLI_reconfigure](/Kubernetes/assets/002/images/multiple_docker_CLI_reconfigure.png)

### Reconfiguring Docker CLI with Minikube

**Purpose:**
- `eval $(minikube docker-env)` is a command used to configure the Docker CLI to interact with the Docker daemon inside a Minikube VM rather than the local Docker daemon.

### How to Use `eval $(minikube docker-env)`

1. **Start Minikube:**
   - Ensure Minikube is running by starting it with the command:
     ```sh
     minikube start
     ```

2. **Reconfigure Docker CLI:**
   - Run the following command to point your Docker CLI to the Minikube Docker daemon:
     ```sh
     eval $(minikube docker-env)
     ```
### Summary

- **`eval $(minikube docker-env)`** redirects the Docker CLI to use the Minikube Docker daemon.
- **Benefits:** Consistency, efficiency, and simplicity in the development workflow.
- **Steps:**
  1. Start Minikube with `minikube start`.
  2. Reconfigure Docker CLI with `eval $(minikube docker-env)`.
  3. Build Docker images and deploy them directly to Minikube.
- **Example Workflow:** Start Minikube, reconfigure Docker, build image, verify, and deploy.

Using this approach ensures a seamless and efficient development experience when working with Minikube and Docker.