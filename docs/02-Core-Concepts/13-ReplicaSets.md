# ReplicaSets
  - Take me to [Video Tutorial](https://kodekloud.com/courses/539883/lectures/9808168)

In this section, we will take a look at the below
- Replication Controller
- ReplicaSet

#### Controllers are brain behind kubernetes

## What is a Replica and Why do we need a replication controller?
- The Replication Controller helps us run multiple instances of a single pod in a kubernetes cluster thus providing **`high availablility`**.
- Even if you have a single pod, the replication controller can help by automatically bringing up new pod when the existing one fails. Thus the replication controller ensures that the specified number of pods are running at all times, even if it is just 1 or 100.

  ![rc](../../images/rc.PNG)
  
- Another reason we need replication is to create multiple pods to share the load across them - **`Load Balancing & Scaling`**.

  ![rc1](../../images/rc1.PNG)
  
## Difference between ReplicaSet and Replication Controller
- **`Replication Controller`** is the older technology that is being replaced by a **`ReplicaSet`**.
- **`ReplicaSet`** is the new way to setup replication.

## Creating a Replication Controller

## Replication Controller Defination File
  
   ![rc2](../../images/rc2.PNG)
  
```
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: myapp-rc
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
```
  - To Create the replication controller
    ```
    $ kubectl create -f rc-defination.yaml
    ```
  - To list all the replication controllers
    ```
    $ kubectl get replicationcontroller
    ```
  - To list pods that are launch by the replication controller
    ```
    $ kubectl get pods
    ```
    ![rc3](../../images/rc3.PNG)
    
## Creating a ReplicaSet
  
## ReplicaSet Defination File

   ![rs](../../images/rs.PNG)

```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```
#### ReplicaSet requires a selector defination when compare to Replicaton Controller.
   
  - To Create the replicaset
    ```
    $ kubectl create -f replicaset-defination.yaml
    ```
  - To list all the replicaset
    ```
    $ kubectl get replicaset
    ```
  - To list pods that are launch by the replicaset
    ```
    $ kubectl get pods
    ```
   
    ![rs1](../../images/rs1.PNG)
    
## Labels and Selectors
#### What is the deal with Labels and Selectors? Why do we label pods and objects in kubernetes?
- We can provide labels to replicaset, under **`selector`** section we use **`matchLabels`** filter and provides the same label that we used while creating the pod. This way replicaset knows which pods to monitor.

  ![labels](../../images/labels.PNG)
  
## How to scale replicaset
- There are multiple ways to scale replicaset
  - First way is to update the number of replicas in the replicaset-defination.yaml defination file. E.g replicas: 6 and then run 
 ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 6
     selector:
       matchLabels:
        type: front-end
```

  ```
  $ kubectl apply -f replicaset-defination.yaml
  ```
  - Second way is to use **`kubectl scale`** command.
  ```
  $ kubectl scale --replicas=6 -f replicaset-defination.yaml
  ```
  - Third way is to use **`kubectl scale`** command with type and name
  ```
  $ kubectl scale --replicas=6 replicaset myapp-replicaset
  ```
  ![rs2](../../images/rs2.PNG)
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
