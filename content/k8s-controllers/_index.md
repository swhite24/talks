+++
title = "My presentation"
outputs = ["Reveal"]
+++

## K8S Controllers

Steven White

---

{{% section %}}

### What is a controller?

- More generic term for "operator"
- Controllers enable you to watch resources / other structured data in K8s and perform custom actions
- Often paried with a custom resource, providing a **declarative** API
- Full integration with K8s tooling, such as `kubectl`

---

### How do controllers work?

- Controllers "watch" types defined in the K8s API
- Reconcile the desired state of an object with the current state in response to changes
- Can be deployed / registered in a cluster just as any other workload

---

<!-- ### Diagram showing data flowing from controller <-> k8s api -->
<img src="/img/controller-data-flow.png" style="max-height: 500px" />

{{% /section %}}

---

{{% section %}}

### What is a CRD?

- **C**ustom **R**esource **D**efinition
- Set of endpoints in the K8s API that store a collection of API objects for a distinct kind
- `Spec` can be anything you want
- Can be versioned, but must provide backward compatibility

---

### What can you do with CRDs?

- Create an "aggregate" resource type for common patterns in your cluster (demo later)
- Perform actions external to your cluster in response to lifecycle actions of resources in your cluster (demo later)
- Many other examples. See [prometheus operator](https://github.com/coreos/prometheus-operator), [nginx ingress controller](https://github.com/kubernetes/ingress-nginx), [istio operator](https://github.com/banzaicloud/istio-operator) for inspiration.

{{% /section %}}

---

{{% section %}}

### Other customizations

---

### Mutating admission webhook

- HTTP callback invoked by kubernetes API when new objects created
- Can be helpful to set defaults / modify objects

---

### Validating admission webhook

- HTTP callback invoked by kubernetes API **after** mutating admission webhooks
- Can be helpful to reject objects / enforce policies

{{% /section %}}

---

{{% section %}}

### Building custom controllers / CRDs

- Several tools are available for building custom kubernetes applications
  - [kubebuilder](https://github.com/kubernetes-sigs/kubebuilder)
  - [operator-sdk](https://github.com/operator-framework/operator-sdk)
  - Others probably
- Most have come to settle on the [controller-runtime](https://github.com/kubernetes-sigs/controller-runtime) as a common set of libraries

---

### Kubebuilder

- _"SDK for building Kubernetes APIs using CRDs"_
- [Documentation](https://book-v1.book.kubebuilder.io/)
- Provides cli to bootstrap new controllers / APIs
- V2 currently in pre-release

---

### Kubebuilder

- `kubebuilder init --domain k8s.io --license apache2 --owner "The Kubernetes Authors"`
  - initialize new project
- `kubebuilder create api --group ships --version v1beta1 --kind Sloop`
  - add new resource / controller pairing

---

### Kubebuilder

- `make install`
  - install CRD definitions to cluster
- `make run`
  - run controller using local kube config

{{% /section %}}

---

{{% section %}}

### Aggregate CRD Demo

---

### Aggregate CRD Demo

- We'll create a new CRD, called `MyApp`
- `MyApp` will have an associated controller watching for instances to be created / modified
- The controller will reconcile the `MyApp` spec with the created deployment, service, and ingress objects

---

{{< slide transition="slide-in" transition-speed="fast" >}}

### Aggregate CRD Demo

<pre><code class="hljs yaml" data-trim data-line-numbers="1-2">
apiVersion: 'cn.meetup.com/v1'
kind: MyApp
metadata:
  name: test-app
  namespace: default
spec:
  image: 'nginx:latest'
  port: 80
</code></pre>

---

{{< slide transition="none" transition-speed="fast" >}}

### Aggregate CRD Demo

<pre><code class="hljs yaml" data-trim data-line-numbers="3-5">
apiVersion: 'cn.meetup.com/v1'
kind: MyApp
metadata:
  name: test-app
  namespace: default
spec:
  image: 'nginx:latest'
  port: 80
</code></pre>

---

{{< slide transition="none" transition-speed="fast" >}}

### Aggregate CRD Demo

<pre><code class="hljs yaml" data-trim data-line-numbers="6-8">
apiVersion: 'cn.meetup.com/v1'
kind: MyApp
metadata:
  name: test-app
  namespace: default
spec:
  image: 'nginx:latest'
  port: 80
</code></pre>

{{% /section %}}

---

# Thank you!
