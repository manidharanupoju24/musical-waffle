## Kubernetes

| Resource | Notes |
|---|---|
| [QoS classes](https://kubernetes.io/docs/concepts/workloads/pods/pod-qos/) in Kubernetes | |
| [Ownership of objects/subresources](https://kubernetes.io/docs/concepts/overview/working-with-objects/owners-dependents/) in Kubernetes | |
| [Status subresource](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#status-subresource) | |
| How do we add/remove ownerRefs for a Kubernetes resource? — [controllerutil](https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/controller/controllerutil) | |
| How does a finalizer work on a single controller in an operator? — [Finalizers](https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/), [Kubebuilder: using finalizers](https://book.kubebuilder.io/reference/using-finalizers) | |
| Kubernetes [Garbage collection](https://kubernetes.io/docs/concepts/architecture/garbage-collection/) | |
| What is a watcher in a controller and how does watching a resource happen within the controller? — [API watch semantics](https://kubernetes.io/docs/reference/using-api/api-concepts/#efficient-detection-of-changes), [Kubebuilder: watching resources](https://book.kubebuilder.io/reference/watching-resources) | |
| Informers, listers & workqueue — [client-go controller internals](https://github.com/kubernetes/sample-controller/blob/master/docs/controller-client-go.md), [cache](https://pkg.go.dev/k8s.io/client-go/tools/cache), [workqueue](https://pkg.go.dev/k8s.io/client-go/util/workqueue) | |
| resourceVersion & optimistic concurrency — [Resource versions](https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions), [API conventions: concurrency control](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md#concurrency-control-and-consistency) | |
| Level-triggered reconciliation — [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/), [Kubebuilder: controller overview](https://book.kubebuilder.io/cronjob-tutorial/controller-overview) | |
