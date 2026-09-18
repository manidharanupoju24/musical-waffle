# Kubernetes Operators — Consolidated Reading List

Your links marked **[yours]**. Everything else is an addition in the same genre
(internals-depth, primary sources, not tutorials).

Suggested order: Layer 1 → 2 → 3 → 4. Layers 5–7 are reference/depth.

---

## Layer 1 — The API server and the object model

The thing your operator is actually a client of.

| Resource | Why |
|---|---|
| [The Kubernetes Control Plane for Busy People Who Like Pictures](https://www.youtube.com/watch?v=zCXiXKMqnuE) — Daniel Smith | Best single overview of how a request becomes an etcd write. Good starting point. |
| [Your second video](https://www.youtube.com/watch?v=_SCzRtU5RRA) **[Essential patterns for Designing and Implementing Operator]** | — |
| [Kubernetes API Concepts](https://kubernetes.io/docs/reference/using-api/api-concepts/) | Resource versions, watch semantics, chunking, `sendInitialEvents`. Short and dense. Read before the list-performance post. |
| [SIG Architecture: API Conventions](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md) | The constitution. Spec/status split, conditions, optional vs required, naming. Every CRD review argument traces back here. |
| [Server-Side Apply](https://kubernetes.io/docs/reference/using-api/server-side-apply/) | Field ownership and conflict semantics — directly relevant when two controllers write the same object. |
| [API Priority and Fairness](https://kubernetes.io/docs/concepts/cluster-administration/flow-control/) | How the apiserver throttles an operator. Pairs with the list-performance post. |

---

## Layer 2 — client-go plumbing

Reflector → DeltaFIFO → Indexer → event handlers → workqueue.

| Resource | Why |
|---|---|
| [sample-controller](https://github.com/kubernetes/sample-controller) + [controller-client-go.md](https://github.com/kubernetes/sample-controller/blob/master/docs/controller-client-go.md) | The canonical wiring diagram. Everything controller-runtime hides is explicit here. |
| [Informers, Listers, Workqueues](https://medium.com/@dhruvbhl/informers-listers-workqueues-the-brain-behind-your-controller-f5b0967026de) | Same material, narrative form. |
| [SIG API Machinery: Writing Controllers](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-api-machinery/controllers.md) | **The highest-value doc on this list.** Written by the people who wrote the controllers. Level-triggered vs edge-triggered, why you re-read instead of trusting the event, why you never assume you saw every change. |
| [client-go `tools/cache` godoc](https://pkg.go.dev/k8s.io/client-go/tools/cache) | Read the package comments on `SharedIndexInformer`, `Reflector`, `DeltaFIFO`. Source comments beat any blog post. |
| [client-go `util/workqueue` godoc](https://pkg.go.dev/k8s.io/client-go/util/workqueue) | The dirty/queue/processing sets and the rate limiter composition. |
| [client-go `examples/workqueue`](https://github.com/kubernetes/client-go/tree/master/examples/workqueue) | ~200 lines, the minimal correct controller. Worth typing out by hand. |
| [client-go `tools/leaderelection` godoc](https://pkg.go.dev/k8s.io/client-go/tools/leaderelection) | Lease mechanics and the failover window. Relevant to conductor-operator. |
| [kubernetes/code-generator](https://github.com/kubernetes/code-generator) | Where typed clients/listers/informers come from. README links Schimanski's "Code Generation for CustomResources" deep dive. |

---

## Layer 3 — controller-runtime / kubebuilder

The layer you actually write against.

| Resource | Why |
|---|---|
| [The Kubebuilder Book](https://book.kubebuilder.io/) | Read [Architecture](https://book.kubebuilder.io/architecture) and the Markers reference properly; skim the tutorial. |
| [controller-runtime godoc](https://pkg.go.dev/sigs.k8s.io/controller-runtime) | Package docs for `pkg/manager`, `pkg/cache`, `pkg/client`, `pkg/builder`, `pkg/predicate`, `pkg/source`. Map each back to its client-go equivalent from Layer 2. |
| [controller-runtime `designs/`](https://github.com/kubernetes-sigs/controller-runtime/tree/main/designs) | Design docs for the abstractions — priority queue, cache options, client behavior. Explains *why* the API looks the way it does. |
| [controller-runtime FAQ](https://github.com/kubernetes-sigs/controller-runtime/blob/main/FAQ.md) | Short, and answers the stale-cache / `APIReader` question people hit in week two. |

---

## Layer 4 — CRD design and schema generation

| Resource | Why |
|---|---|
| [CRD generation pitfalls](https://ahmet.im/blog/crd-generation-pitfalls/) — Ahmet Alp Balkan | Four ways to say "optional", zero-vs-null, nested defaulting, silently-ignored misspelled markers. Audit your own CRDs against this. |
| [CustomResourceDefinitions](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/) | Structural schemas, pruning, subresources, conversion, CEL validation rules. |
| [Versions in CRDs](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/) | Storage version, conversion webhooks, what a version bump actually costs you. |

---

## Layer 5 — Scale and failure modes

| Resource | Why |
|---|---|
| [Kubernetes list performance](https://ahmet.im/blog/kubernetes-list-performance/) — Ahmet Alp Balkan | The `limit=500&resourceVersion=0` trap, watch cache behavior by version, KEP-2340 / 3157 / 4988 / 5116. Read *after* the API Concepts doc. |
| [kubernetes/enhancements (KEPs)](https://github.com/kubernetes/enhancements) | Grep for the KEP numbers above and read the originals. KEPs are the best-written docs in the project. |

---

## Layer 6 — Go runtime underneath it all

| Resource | Why |
|---|---|
| [Understanding the Go Runtime — The Bootstrap](https://internals-for-interns.com/posts/understanding-go-runtime/) **[yours]** | Part 1 of 11. |
| [The Scheduler](https://internals-for-interns.com/posts/go-runtime-scheduler/) | G/M/P model — why `MaxConcurrentReconciles` behaves as it does. |
| [The Garbage Collector](https://internals-for-interns.com/posts/go-garbage-collector/) | Pacer, heap doubling. Explains the `GOGC=200` advice in the list-performance post. |
| [The Memory Allocator](https://internals-for-interns.com/posts/go-memory-allocator/) | Size classes, per-P caches. |
| [The Network Poller](https://internals-for-interns.com/posts/go-netpoller/) | What a long-lived watch connection costs. |
| [Slices, Maps, Channels](https://internals-for-interns.com/posts/go-runtime-slices-maps-channels/) · [select](https://internals-for-interns.com/posts/go-runtime-select/) · [Profiling](https://internals-for-interns.com/posts/go-runtime-profiling/) | Rest of the series. |

---

## Layer 7 — Read real operators

Better than any blog once the fundamentals land.

| Repo | Read it for |
|---|---|
| [cert-manager](https://github.com/cert-manager/cert-manager) | Finalizers, multi-step external flows, condition-driven state machines. The textbook case. |
| [external-dns](https://github.com/kubernetes-sigs/external-dns) | Reconciling into an external DNS provider with rate limits and drift — closest analogue to runway-ingress-operator. |
| [kubebuilder](https://github.com/kubernetes-sigs/kubebuilder) | Scaffolding source; useful when a generated file confuses you. |

---

## Books

- **Programming Kubernetes** (Hausenblas & Schimanski) — strongest on informers/workqueues; 2019, client-go-centric, so translate to controller-runtime yourself.
- **Kubernetes Best Practices** / **Managing Kubernetes** — operational context, lighter on internals.
