# tutorial11-kubernetes

1. Compare the application logs before and after you exposed it as a Service.
  Try to open the app several times while the proxy into the Service is running.
  What do you see in the logs? Does the number of logs increase each time you open the app?

**Before Service**
![beforeService](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/f11a65a3-d0d7-4f2a-9ce6-81745a34b43a)

**After Service**
![image](https://github.com/sorfeb/tutorial11-kubernetes/assets/112263712/b47a2abb-0dcd-4271-8495-e14a9a0f488a)


3. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to
`kube-system`.
What is the purpose of the `-n` option and why did the output not list the pods/services that you
explicitly created?
> Hint: Do some reading about [Namespace in Kubernetes
documentation](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/).
