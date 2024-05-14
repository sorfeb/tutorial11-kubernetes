# tutorial11-kubernetes
> Soros Febriano 2206083445 | AdPro-B

## Reflection on Hello Minikube
### 1. Compare the application logs before and after you exposed it as a Service.
  Try to open the app several times while the proxy into the Service is running.
  What do you see in the logs? Does the number of logs increase each time you open the app?

**Service Logs**
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/b47a2abb-0dcd-4271-8495-e14a9a0f488a)
After exposing the pod as a Service, the pod can receive external requests. When I tried to open the app several times while it is running, the number of requests are recorded in the logs, causing it to increase.

### 2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to
`kube-system`.
What is the purpose of the `-n` option and why did the output not list the pods/services that you
explicitly created?
> Hint: Do some reading about [Namespace in Kubernetes
documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).

The `-n` option is crucial for specifying and operating within different **namespaces**. If `-n` isn't specified in the command, then it will operate the command in the `default` namespace. If the output does not list the pods/services you have explicitly created, it could be due to the resources that have been created are in a **different namespace**

## Reflection on Rolling Update & Kubernetes Manifest File
### 1. What is the difference between Rolling Update and Recreate deployment strategy?
> Hint: Read the Deployments documentation.
### 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document
your attempt.
### 3. Prepare different manifest files for executing Recreate deployment strategy.
### 4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience
in deploying the app manually and compare it to your experience when deploying the same app
by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.
### 5. (Optional) Do the same tutorial steps, but on a managed Kubernetes cluster (e.g., GCP). You
need to provision a Kubernetes cluster on Google Cloud Platform. Then, re-run the tutorial steps
(Hello Minikube and Rolling Update) on the remote cluster. Document your attempt and highlight
the differences and any issues you encountered.

