# tutorial11-kubernetes
> Soros Febriano 2206083445 | AdPro-B

## Reflection on Hello Minikube
### 1. Compare the application logs before and after you exposed it as a Service.
  Try to open the app several times while the proxy into the Service is running.
  What do you see in the logs? Does the number of logs increase each time you open the app?

**Service Logs**
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/b47a2abb-0dcd-4271-8495-e14a9a0f488a)
After exposing the pod as a Service, the pod can receive external requests. When I tried to open the app several times while it is running, the number of requests are recorded in the logs, causing it to increase.

### 2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?
> Hint: Do some reading about [Namespace in Kubernetes
documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

The `-n` option is crucial for specifying and operating within different **namespaces**. If `-n` isn't specified in the command, then it will operate the command in the `default` namespace. If the output does not list the pods/services you have explicitly created, it could be due to the resources that have been created are in a **different namespace**

<br>

## Reflection on Rolling Update & Kubernetes Manifest File
### 1. What is the difference between Rolling Update and Recreate deployment strategy?
> Hint: Read the Deployments documentation.

_**Rolling Update Strategy:**_
- **Zero Downtime:** Rolling updates aim to update Pods with zero downtime. This is done by incrementally replacing old Pods with new ones.
- **Incremental Updates:** New Pods are created before old Pods are terminated. This allows the application to remain available to users throughout the update process.
- **Use Cases:** Ideal for production environments where maintaining application availability is critical. Useful for applications that can handle having different versions running simultaneously during the update process.

_**Recreate Strategy:**_
- **Downtime:** The Recreate strategy involves bringing down all existing Pods before starting new ones. This results in a period of downtime where no instances of the application are running.
- **Complete Replacement:** This strategy replaces all instances of the application simultaneously. Once all old Pods are terminated, new Pods are started.
- **Use Cases:** Suitable for applications where downtime is acceptable or necessary, such as batch jobs or non-critical applications. Useful when the application cannot run multiple versions simultaneously, or if running different versions concurrently might cause issues.

### 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.

**Sebelum memakai Recreate Deployment Strategy:**
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/e681fa1c-6d48-4d03-9ec7-ddbad87dab46)

1. **Change strategy type to `Recreate`:** `strategy:
    type: Recreate`
2. **Apply updated deployment.yaml file:**
`kubectl apply -f recreate-deployment.yaml`
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/ca49c273-6ac4-4d4b-87ba-34bc2f6ba452)
3. **Apply service.yaml file:**
`kubectl apply -f service.yaml`
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/900ccf34-8489-4fa6-a93e-2d3f46bbeb34)

4. **Results:** <br>
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/dccabf76-37d9-428f-ae52-0ea2182865c0)
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/e1d822a8-77e2-430a-ad53-eaf5955c32ae)
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/c739ed97-342b-441b-ab46-009fca023bb5)
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/7f7cacc9-cc64-4f2b-b837-3254f2ab7766)

### 3. Prepare different manifest files for executing Recreate deployment strategy.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "4"
  creationTimestamp: "2024-05-14T14:09:30Z"
  generation: 5
  labels:
    app: spring-petclinic-rest
  name: spring-petclinic-rest
  namespace: default
  resourceVersion: "6387"
  uid: f7a01d40-ed66-4277-90e7-4d402701b10a
spec:
  progressDeadlineSeconds: 600
  replicas: 4
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: spring-petclinic-rest
  strategy:
    type: Recreate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: spring-petclinic-rest
    spec:
      containers:
      - image: docker.io/springcommunity/spring-petclinic-rest:3.2.1
        imagePullPolicy: IfNotPresent
        name: spring-petclinic-rest
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
  - lastTransitionTime: "2024-05-14T14:51:30Z"
    lastUpdateTime: "2024-05-14T14:51:30Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2024-05-14T14:09:30Z"
    lastUpdateTime: "2024-05-14T14:57:11Z"
    message: ReplicaSet "spring-petclinic-rest-54f476f68" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 5
  readyReplicas: 4
  replicas: 4
  updatedReplicas: 4
```

### 4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.
Using Kubernetes manifest files provides several benefits compared to deploying applications manually that allows me to define the desired state of the application in a declarative manner. You specify what you want, and Kubernetes takes care of creating and maintaining that state. When using manual deployment, it involves executing a series of kubectl commands to create deployments, services, and other resources. Each step must be performed in the correct order, so it is more prone to errors, difficult to replicate, time-consuming, and lacks consistency. Keeping track of all manual changes is difficult.

### 5. (Optional) Do the same tutorial steps, but on a managed Kubernetes cluster (e.g., GCP). You need to provision a Kubernetes cluster on Google Cloud Platform. Then, re-run the tutorial steps (Hello Minikube and Rolling Update) on the remote cluster. Document your attempt and highlight the differences and any issues you encountered.
