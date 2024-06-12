
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Multicontainer app deployment in Kubernetes.](#multicontainer-app-deployment-in-kubernetes)
  - [🙋 How does our multicontainer app look in local k8s?](#-how-does-our-multicontainer-app-look-in-local-k8s)
  - [🙋 How to apply all the configuration files in the folder to kubernetes cluster?](#-how-to-apply-all-the-configuration-files-in-the-folder-to-kubernetes-cluster)
    - [Applying All Configuration Files in a Folder to Kubernetes Cluster](#applying-all-configuration-files-in-a-folder-to-kubernetes-cluster)
    - [Command Syntax](#command-syntax)
    - [Example](#example)
    - [Recursive Application](#recursive-application)
    - [Combining all the configurations into a single file](#combining-all-the-configurations-into-a-single-file)
    - [Applying the Combined Configuration](#applying-the-combined-configuration)
  - [🙋 There can be deployments (multi worker pod) without a cluster IP assigned to it explain why?](#-there-can-be-deployments-multi-worker-pod-without-a-cluster-ip-assigned-to-it-explain-why)
    - [Explanation for Multi-Worker Pod Not Assigned with Cluster IP in the Image](#explanation-for-multi-worker-pod-not-assigned-with-cluster-ip-in-the-image)
      - [Specific Focus: Multi-Worker Pod](#specific-focus-multi-worker-pod)
  - [🙋 Why are volumes needed for a databse on kubernetes?](#-why-are-volumes-needed-for-a-databse-on-kubernetes)
    - [Issue with volume within a pod.](#issue-with-volume-within-a-pod)
    - [Explanation for Volumes Needed for Database on Kubernetes](#explanation-for-volumes-needed-for-database-on-kubernetes)
  - [🙋 Differences Between Kubernetes Volume, Persistent Volume, and Persistent Volume Claim?](#-differences-between-kubernetes-volume-persistent-volume-and-persistent-volume-claim)
    - [Kubernetes Volume](#kubernetes-volume)
    - [Persistent Volume (PV)](#persistent-volume-pv)
    - [Persistent Volume Claim (PVC)](#persistent-volume-claim-pvc)
    - [Relationships and Workflow](#relationships-and-workflow)
    - [Summary Table](#summary-table)
  - [🙋 Statically and Dynamically Provisioned Persistent Volume Claims in Kubernetes](#-statically-and-dynamically-provisioned-persistent-volume-claims-in-kubernetes)
    - [Statically Provisioned PVCs](#statically-provisioned-pvcs)
    - [Dynamically Provisioned PVCs](#dynamically-provisioned-pvcs)
    - [Summary Table](#summary-table-1)
  - [🙋 Discuss Kubernetes access modes.](#-discuss-kubernetes-access-modes)
    - [Summary Table](#summary-table-2)
    - [Important Considerations](#important-considerations)
  - [🙋 Discuss about the provisioner in kubernetes for storage class while using PVC?](#-discuss-about-the-provisioner-in-kubernetes-for-storage-class-while-using-pvc)
    - [Provisioner in Kubernetes for Storage Class with PVC](#provisioner-in-kubernetes-for-storage-class-with-pvc)
    - [Key Concepts](#key-concepts)
    - [Example of a Storage Class](#example-of-a-storage-class)
    - [Common Provisioners](#common-provisioners)
    - [How PVC Uses Storage Class and Provisioner](#how-pvc-uses-storage-class-and-provisioner)
    - [Steps Involved in Provisioning](#steps-involved-in-provisioning)
  - [🙋 Explain with an example how persistent volume claim is used in a postgress pod.](#-explain-with-an-example-how-persistent-volume-claim-is-used-in-a-postgress-pod)
    - [Explanation of Key Sections](#explanation-of-key-sections)
    - [database-persistent-volume-claim.yaml](#database-persistent-volume-claimyaml)
    - [Detailed Comments for `postgres-deployment.yaml`](#detailed-comments-for-postgres-deploymentyaml)

<!-- /code_chunk_output -->

# Multicontainer app deployment in Kubernetes.

## 🙋 How does our multicontainer app look in local k8s?
![Local app deployment](/Kubernetes/assets/003/image/k8s_local_app_deployment.png)

## 🙋 How to apply all the configuration files in the folder to kubernetes cluster?
### Applying All Configuration Files in a Folder to Kubernetes Cluster

To apply all configuration files in a folder to a Kubernetes cluster, you can use the `kubectl apply` command with the `-f` flag, pointing to the directory containing the configuration files. This will apply all YAML or JSON files within the specified directory.

### Command Syntax

```sh
kubectl apply -f <directory-path>
```

### Example

1. **Directory Structure:**
   - Suppose you have the following directory structure:

   ```
   k8s-configs/
   ├── deployment.yaml
   ├── service.yaml
   ├── configmap.yaml
   ```

2. **Navigate to Directory:**
   - Open your terminal and navigate to the directory containing the configuration files:

   ```sh
   cd path/to/k8s-configs
   ```

3. **Apply All Configurations:**
   - Run the following command to apply all the configuration files in the directory:

   ```sh
   kubectl apply -f .
   ```

   - Alternatively, you can specify the directory path directly:

   ```sh
   kubectl apply -f path/to/k8s-configs
   ```

### Recursive Application

If your directory contains subdirectories with additional configuration files, you can apply them recursively using the `--recursive` flag.

```sh
kubectl apply -f path/to/k8s-configs --recursive
```

### Combining all the configurations into a single file

**Combine into a Single File:**

   Create a new file called `combined-config.yaml` and concatenate the contents of each file using `---` as a separator.

   **combined-config.yaml:**
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
           ports:
           - containerPort: 80
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     selector:
       app: my-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 80
     type: ClusterIP
   ---
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: my-config
   data:
     key: value
   ```

### Applying the Combined Configuration

Use the `kubectl apply -f` command to apply the combined configuration file:

```sh
kubectl apply -f combined-config.yaml
```

## 🙋 There can be deployments (multi worker pod) without a cluster IP assigned to it explain why?

![Local app deployment](/Kubernetes/assets/003/image/k8s_local_app_deployment.png)

### Explanation for Multi-Worker Pod Not Assigned with Cluster IP in the Image

#### Specific Focus: Multi-Worker Pod

In the diagram, the **multi-worker pod** is not directly assigned a ClusterIP service, unlike the other pods. This design choice could be due to several reasons:

1. **Role of the Multi-Worker Pod:**
   - The multi-worker pod might be performing background tasks, processing jobs, or handling data that doesn't require direct service exposure.
   - It could be consuming messages from a queue or interacting directly with other components via internal communication mechanisms.

2. **Internal Communication:**
   - The multi-worker pod might be designed to communicate with other services (e.g., Redis, Postgres) internally without needing an external service endpoint.
   - It can access other pods or services using environment variables, service discovery, or direct pod IPs.

3. **No Direct Client Requests:**
   - If the multi-worker pod is not intended to handle incoming client requests or external traffic, it might not need a ClusterIP service.
   - It can still be managed and scaled by the deployment controller.

4. **Resource Optimization:**
   - By not assigning a ClusterIP service, resources can be optimized, avoiding unnecessary service endpoints that are not needed.

## 🙋 Why are volumes needed for a databse on kubernetes?
### Issue with volume within a pod.
![Issue with data within pod](/Kubernetes/assets/003/image/Database_volume_reason_1.png)

### Explanation for Volumes Needed for Database on Kubernetes
![volume outside POD](/Kubernetes/assets/003/image/Database_volume_reason_2.png)

## 🙋 Differences Between Kubernetes Volume, Persistent Volume, and Persistent Volume Claim?

Understanding the differences between Kubernetes Volume, Persistent Volume (PV), and Persistent Volume Claim (PVC) is crucial for effectively managing storage in a Kubernetes cluster.

### Kubernetes Volume

![Docker vs Kuberenetes volume](/Kubernetes/assets/003/image/Docker_vs_kubernetes_volume.png)

**Definition:**
- A volume in Kubernetes is a directory accessible to containers in a pod. It is used to store data that is accessible across container restarts.

**Characteristics:**
- Volumes are tied to the lifecycle of a pod. When the pod ceases to exist, the volume and its data may also be deleted, depending on the type of volume.
- Volumes are used to share data between containers in the same pod.

**Types:**
- **EmptyDir:** Temporary storage that is created when a pod is assigned to a node and exists as long as that pod is running.
- **HostPath:** Maps a directory on the host node’s filesystem into a pod.
- **ConfigMap, Secret:** Special volumes for storing configuration data or secrets.
- **NFS, GlusterFS, etc.:** Network file systems that allow sharing storage between pods.

![Kubernetes_volume](/Kubernetes/assets/003/image/kuberenetes_volume.png)

**Example:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    # Mounts the volume below to the specified path in the container
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: my-volume
  # Defines a volume that can be mounted into containers
  volumes:
  - name: my-volume
    emptyDir: {}

```

### Persistent Volume (PV)

**Definition:**
- A Persistent Volume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using storage classes. It is a cluster-wide resource.

**Characteristics:**
- PVs are not tied to the lifecycle of a pod and exist independently of pods.
- PVs have a lifecycle defined by the administrator and provide storage resources to the cluster.

**Provisioning:**
- **Static:** An administrator manually creates PVs.
- **Dynamic:** Kubernetes dynamically provisions PVs based on storage classes.

![Volume vs Persistent Volume](/Kubernetes/assets/003/image/Volume_vs_persistent_volume.png)

**Example:**
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi  # Specifies the storage capacity
  accessModes:
    - ReadWriteOnce  # Defines the access mode as read-write by a single node
  persistentVolumeReclaimPolicy: Retain  # Policy for retaining the volume after it is released
  nfs:
    path: /path/to/nfs  # NFS server mount path
    server: nfs-server.example.com  # NFS server address

```

### Persistent Volume Claim (PVC)

**Definition:**
- A Persistent Volume Claim (PVC) is a request for storage by a user. It is similar to a pod in that pods consume node resources and PVCs consume PV resources.

**Characteristics:**
- PVCs allow users to request storage without needing to know the details of the underlying storage infrastructure.
- A PVC specifies the size and access mode required, and Kubernetes binds the PVC to an appropriate PV that meets the requirements.

**Binding:**
- Kubernetes automatically binds a PVC to a suitable PV that meets the requested storage size and access mode.
![PVC_1](/Kubernetes/assets/003/image/PVC_1.png)
![PVC_2](/Kubernetes/assets/003/image/PVC_2.png)

**Example:**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc  # Name of the Persistent Volume Claim
spec:
  accessModes:
    - ReadWriteOnce  # The PVC can be mounted as read-write by a single node
  resources:
    requests:
      storage: 10Gi  # Requesting 10 GiB of storage

```

### Relationships and Workflow

1. **Admin creates PVs**: Static or dynamic provisioning based on storage classes.
2. **User creates PVCs**: Users request storage by creating PVCs.
3. **Kubernetes binds PVCs to PVs**: The control plane matches and binds the PVC to an appropriate PV.
4. **Pods use PVCs**: Pods request storage by referencing PVCs, which in turn are backed by PVs.

### Summary Table

| **Component**  | **Definition**                                                                 | **Lifecycle**                    | **Use Case**                               |
|----------------|-------------------------------------------------------------------------------|----------------------------------|--------------------------------------------|
| **Volume**     | Directory accessible to containers in a pod.                                  | Tied to the lifecycle of a pod.  | Temporary or shared storage within a pod.  |
| **PV**         | Cluster-wide storage resource provisioned by admin or dynamically.            | Independent of pod lifecycle.    | Persistent storage for cluster applications. |
| **PVC**        | User request for storage that gets bound to a PV.                             | Independent of pod lifecycle.    | Abstracts storage details from users.      |

By understanding these components and their roles, you can effectively manage and provision storage resources in your Kubernetes cluster.

## 🙋 Statically and Dynamically Provisioned Persistent Volume Claims in Kubernetes

In Kubernetes, Persistent Volume Claims (PVCs) can be provisioned either statically or dynamically. Here's an explanation of both methods:

### Statically Provisioned PVCs

**Definition:**
- Static provisioning involves manually creating Persistent Volumes (PVs) before they are claimed by Persistent Volume Claims (PVCs).

**Process:**
1. **Administrator Creates PVs:** An administrator pre-creates PVs that specify storage details such as capacity, access modes, and storage backend.
2. **User Creates PVCs:** Users create PVCs to request storage.
3. **Kubernetes Binds PVCs to PVs:** Kubernetes matches and binds the PVCs to appropriate pre-created PVs.

**Advantages:**
- **Control:** Administrators have full control over the storage provisioning process and can ensure that specific storage configurations are used.
- **Predictability:** The storage characteristics are predefined and predictable.

**Disadvantages:**
- **Manual Effort:** Requires manual creation and management of PVs, which can be time-consuming.
- **Inflexibility:** Less flexible in dynamic environments where storage requirements can change rapidly.

**Example:**
**Persistent Volume (PV):**
``` yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv  # Name of the Persistent Volume
spec:
  capacity:
    storage: 10Gi  # Specifies the size of the storage
  accessModes:
    - ReadWriteOnce  # The volume can be mounted as read-write by a single node
  nfs:
    path: /path/to/nfs  # NFS server mount path
    server: nfs-server.example.com  # NFS server address
```

**Persistent Volume Claim (PVC):**
``` yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc  # Name of the Persistent Volume Claim
spec:
  accessModes:
    - ReadWriteOnce  # The claim can request a volume that can be mounted as read-write by a single node
  resources:
    requests:
      storage: 10Gi  # Requests a volume of at least 10Gi
```

### Dynamically Provisioned PVCs

**Definition:**
- Dynamic provisioning allows Kubernetes to automatically create PVs as needed when PVCs are requested, using storage classes that define the storage backend and parameters.

**Process:**
1. **Administrator Defines Storage Classes:** Storage classes define different types of storage (e.g., fast, slow) and the underlying storage provisioner.
2. **User Creates PVCs:** Users create PVCs that reference a storage class.
3. **Kubernetes Dynamically Creates PVs:** Kubernetes automatically creates a PV that matches the PVC’s requirements using the specified storage class.

**Advantages:**
- **Automation:** Reduces manual effort as PVs are created on-demand.
- **Flexibility:** More flexible and scalable, suitable for dynamic environments.
- **Efficiency:** Better resource utilization and easier to manage at scale.

**Disadvantages:**
- **Less Control:** Administrators have less direct control over the exact configuration of the PVs.
- **Complexity:** Requires proper setup and configuration of storage classes and provisioners.

**Example:**
**Storage Class:**
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage  # Name of the storage class
provisioner: kubernetes.io/aws-ebs  # AWS EBS provisioner
parameters:
  type: gp2  # General Purpose SSD volume type

```

**Persistent Volume Claim (PVC):**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-dynamic-pvc  # Name of the Persistent Volume Claim
spec:
  accessModes:
    - ReadWriteOnce  # Defines the access mode as ReadWriteOnce
  resources:
    requests:
      storage: 10Gi  # Specifies the amount of storage requested
  storageClassName: fast-storage  # Specifies the storage class to use

```

### Summary Table

| **Provisioning Method** | **Process**                                                                                     | **Advantages**                                           | **Disadvantages**                                      |
|-------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------|-------------------------------------------------------|
| **Static Provisioning** | Administrator pre-creates PVs, users create PVCs, Kubernetes binds them.                       | Full control, predictability.                           | Manual effort, less flexible.                         |
| **Dynamic Provisioning**| Administrator defines storage classes, users create PVCs with storage class, Kubernetes creates PVs. | Automation, flexibility, efficiency.                    | Less control, requires proper setup of storage classes. |

By choosing the appropriate provisioning method, you can balance control, flexibility, and automation according to your Kubernetes cluster's needs and the nature of the workloads.

## 🙋 Discuss Kubernetes access modes.

![Kubernetes_access_modes](/Kubernetes/assets/003/image/k8s_volume_access_modes.png)

### Summary Table

| **Access Mode** | **Description**                                          | **Use Case**                                              |
|-----------------|----------------------------------------------------------|-----------------------------------------------------------|
| **ReadWriteOnce (RWO)** | Volume can be mounted as read-write by a single node.     | Stateful applications needing exclusive write access.      |
| **ReadOnlyMany (ROX)**  | Volume can be mounted as read-only by multiple nodes.     | Sharing static data across multiple pods.                 |
| **ReadWriteMany (RWX)** | Volume can be mounted as read-write by multiple nodes.    | Collaborative applications needing shared write access.   |

### Important Considerations

- **Storage Backend Support:** Not all storage backends support all access modes. Verify the supported access modes for your chosen storage provisioner.
- **Performance:** Concurrent access (RWX) may introduce performance considerations depending on the storage backend.
- **Consistency:** Ensure your application handles the consistency model of the underlying storage system, especially for RWX.

Understanding these access modes helps you design and configure your storage requirements in a Kubernetes environment effectively, aligning with your application’s needs and ensuring optimal performance and reliability.

## 🙋 Discuss about the provisioner in kubernetes for storage class while using PVC?

### Provisioner in Kubernetes for Storage Class with PVC

In Kubernetes, a provisioner is a piece of software responsible for dynamically provisioning persistent volumes (PVs) based on storage classes. Storage classes define the types of storage available in a Kubernetes cluster, and the provisioner specifies how these storage types should be created and managed.

### Key Concepts

1. **Storage Class:**
   - A Kubernetes object that provides a way to describe the "classes" of storage offered in a cluster.
   - Defines the provisioner, parameters, and reclaim policy for dynamically provisioned PVs.
   
2. **Provisioner:**
   - The component that fulfills the storage request by creating the actual storage resource in the underlying storage system.
   - Each provisioner is specific to a storage backend, such as AWS EBS, GCE PD, NFS, Ceph, etc.

3. **Persistent Volume Claim (PVC):**
   - A user's request for storage.
   - PVCs can specify a storage class to use a specific provisioner.

### Example of a Storage Class

Here’s an example of a storage class definition in Kubernetes:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  iopsPerGB: "10"
  fsType: ext4
reclaimPolicy: Delete
```

### Common Provisioners

1. **AWS EBS (Elastic Block Store):**
   - `provisioner: kubernetes.io/aws-ebs`
   - Parameters can include `type` (gp2, io1, etc.), `iopsPerGB`, and `fsType`.

2. **Google Cloud Persistent Disk (GCE PD):**
   - `provisioner: kubernetes.io/gce-pd`
   - Parameters can include `type` (pd-standard, pd-ssd) and `fsType`.

3. **Azure Disk:**
   - `provisioner: kubernetes.io/azure-disk`
   - Parameters can include `storageaccounttype` (Standard_LRS, Premium_LRS).

4. **NFS (Network File System):**
   - Third-party provisioners can be used, such as `nfs-client`.

5. **Ceph RBD:**
   - `provisioner: ceph.com/rbd`
   - Parameters include `monitors`, `pool`, `adminId`, `adminSecretNamespace`, and `adminSecretName`.

6. **OpenEBS:**
   - `provisioner: openebs.io/provisioner-iscsi`
   - Parameters can include `storagePool`, `replicaCount`, and `casType`.

### How PVC Uses Storage Class and Provisioner

When a PVC specifies a storage class, Kubernetes uses the provisioner defined in that storage class to create the required persistent volume. Here’s an example:

**Persistent Volume Claim (PVC):**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: fast-storage
```

### Steps Involved in Provisioning

1. **User Creates PVC:**
   - The user creates a PVC specifying a storage class.
   
2. **Kubernetes Identifies Storage Class:**
   - Kubernetes identifies the storage class and its provisioner.
   
3. **Provisioner Creates PV:**
   - The provisioner dynamically provisions a PV that matches the PVC's requirements.
   
4. **PVC Binds to PV:**
   - Kubernetes binds the PVC to the newly created PV.
   
5. **Pod Uses PVC:**
   - The pod can now use the PVC to access the storage provided by the PV.

## 🙋 Explain with an example how persistent volume claim is used in a postgress pod.

### Explanation of Key Sections

- **spec.template.spec.volumes:**
  - This section defines the volumes that the pod will use. In this case, it references a Persistent Volume Claim (PVC) named `database-persistent-volume-claim`. Kubernetes will use this claim to find and attach the necessary storage to the pod.

- **spec.template.spec.containers.volumeMounts:**
  - This section specifies how the volumes should be mounted within the container. The `mountPath` is where in the container's filesystem the volume will be accessible. The `subPath` ensures that data is stored within a specific subdirectory named `postgres` within the persistent volume.

- **persistentVolumeClaim.claimName:**
  - The `claimName` matches the name of the PVC created in the `database-persistent-volume-claim.yaml` file. This tells Kubernetes to use the storage requested by this PVC.

### database-persistent-volume-claim.yaml

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim  # Name used to reference this PVC
spec:
  accessModes:
    - ReadWriteOnce  # The volume can be mounted as read-write by a single node
  resources:
    requests:
      storage: 2Gi  # Requesting 2Gi of storage
```

### Detailed Comments for `postgres-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres  # Name of the Postgres deployment
spec:
  replicas: 1  # Ensuring there is only one pod running at a time for this deployment
  selector:
    matchLabels:
      app: postgres  # Label selector to match the pods
  template:
    metadata:
      labels:
        app: postgres  # Labels applied to the pod template
    spec:
      containers:
      - name: postgres  # Container name
        image: postgres:latest  # Image to use for the container, latest version of Postgres
        ports:
        - containerPort: 5432  # Exposing port 5432 for Postgres
        # Volume mounts for the container
        volumeMounts:
        - name: postgres-storage  # Name of the volume mount, must match the volume name below
          mountPath: /var/lib/postgresql/data  # Path inside the container where the volume will be mounted
          subPath: postgres  # Use a subdirectory named 'postgres' within the persistent volume to store data
      # Volumes section to define the persistent storage
      volumes:
      - name: postgres-storage  # Name of the volume, referenced in volumeMounts
        persistentVolumeClaim:
          claimName: database-persistent-volume-claim  # Referencing the PVC created earlier to request storage
```
