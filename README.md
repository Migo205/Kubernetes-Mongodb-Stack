


# Kubernetes MongoDB Stack 

This project serves as a complete and practical example of a DevOps application, demonstrating how to deploy and manage a **MongoDB** database and its graphical user interface **Mongo-Express** on a **Kubernetes** platform. The project aims to showcase best practices for managing configurations, sensitive data, and persistent storage.

##  Project Features

  * **Configuration Separation:** Utilizes a **`ConfigMap`** to manage general configurations, making it easy to modify them without changing the application's code.
  * **Sensitive Data Management:** Employs **`Secrets`** to securely store sensitive login credentials (username and password) in an encrypted format.
  * **Persistent Storage:** Ensures the database's data persists even if the container is restarted or deleted, by using a **`PersistentVolumeClaim`** and **`StorageClass`**.
  * **External Access:** Allows external access to the **Mongo-Express** UI through a **`Service`** of type **`LoadBalancer`**.

##  How to Run the Project

### Prerequisites

Ensure you have the following tools installed:

  * **kubectl:** To interact with your Kubernetes Cluster.
  * **Kubernetes Cluster:** Can be local (like **Minikube** or **Kind**) or on a cloud provider (like **AWS EKS** or **Google GKE**).
  * **AWS EBS CSI Driver:** If you're using AWS EKS, ensure this driver is installed to support storage on AWS.

### Steps

1.  **Create Resources:** Apply all the YAML files in the project in the following order:

    ```bash
    kubectl apply -f secret.yaml
    kubectl apply -f storageclass.yaml
    kubectl apply -f pvc.yaml
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    kubectl apply -f configmap.yaml
    kubectl apply -f deployment2.yaml
    kubectl apply -f svc2.yaml
    ```

     **Note:** Make sure the `deployment2.yaml` file has the correct `Secret` name: `mongodb-pass`.

2.  **Check Resource Status:** After applying the files, you can check the status of everything using the following commands:

      * To ensure the **`Pods`** are running:
        ```bash
        kubectl get pods
        ```
      * To get information about the **`Services`**:
        ```bash
        kubectl get services
        ```

3.  **Access the Application:**

      * Once the **`LoadBalancer`** `Service` is ready, it will provide you with an external IP address.
      * Copy this address and paste it into your web browser, specifying port `8081` (e.g., `http://<external-ip>:8081`).
      * This will take you to the **Mongo-Express** UI, where you can manage your database.

##  Project Components

Each YAML file in this project has a specific role:

  * **`secret.yaml`:** Creates a **`Secret`** to store the credentials (username & password) for the MongoDB database.
  * **`storageclass.yaml`:** Defines the type of storage required (in this case, AWS EBS).
  * **`pvc.yaml`:** Requests a persistent storage volume (5Gi) to be used by MongoDB.
  * **`deployment.yaml`:** Defines how to deploy the MongoDB database, linking it to the `Secret` and `PersistentVolumeClaim`.
  * **`service.yaml`:** Creates an internal **`Service`** that allows other applications within the Cluster to access MongoDB.
  * **`configmap.yaml`:** Creates a **`ConfigMap`** to store the internal `Service` address of MongoDB.
  * **`deployment2.yaml`:** Defines how to deploy the Mongo-Express UI, linking it to the `Secret` and `ConfigMap`.
  * **`svc2.yaml`:** Creates a **`Service`** of type `LoadBalancer` to expose Mongo-Express to external traffic.

##  How to Delete the Project

To remove all project resources, run the following command:

```bash
kubectl delete -f .
```

This command will delete all files in the current directory.

