## Data Dog; Making most of k8s audit log

- get examples
- describe examples
- delete examples
  - delete + list & watch
- audit logs
  - apiserver tracks all requests and responses
  - answers questions like:
    1. what is it?
    1. who made the request?
    1. why was it approved?
    1. when, and from where?
  - Get is mapped to different verbs (get/list)
  - get + watch
  - create call
- configuration of audit logs
  - two paths: `--audit-log-path` where to store and `--audit-policy-file` what
    to collect
  - policy file: set of rules
    - rules match api call
    - similar to rBAC rules
  - log everything including request body and response body
  - rules are evaluated top down
  - group: "" means core api group (**don't forget to include**)
  - audit-policy
  - getting logs is easy; will get a lot of logs
- calls from group "system:nodes" is high
  - nodes 500 rps; kubelet get refresh for secrets and configmaps
  - list latency by resources
  - compare api request latency perf b/t large and small clusters
  - be extra careful of daemonset doing API calls
- audit logs are verbose
- lifecycle of a create call
  - create events show progression
  - get pods and update container status
  - interaction b/t components and sequence of events
  - events have 1h default TTL
- troubleshooting using audit logs
  - why was a resource deleted?
  - which application is responsible for so many calls?
  - visibility to http status codes
  - 401 - expiring certs
  - 403 - bad rBAC
  - 422 - kube-system:generic-garbage-collector patch on pod; remove owner ref;
    evicted pods; mutating webhook patching immutable fields;
- conclusions
  1. audit logs are valuable
  1. requires a tool to analyze the logs

## session 2: Pivotal; buildpack for cloud native
- Update some dependencies
  - dependencies of buildpack
  - build phase
    - rebuild and analyze only layers that have change
## session 3: Pod runtime; greater accountability; Pod Overhead
- pod.resources != sum(container[].resources)
  - node resources consists of:
    1. system reserved
    2. kube reserved
    3. allocatable
  - sandbox runtime; pod overhead
- Pod overhead; runtime class
  - class: RuntimeClass
  - OCI runtime; single cluster supports multiple runtimes
  - Add overhead memory: "130Mi"
  - Pod Spec: add runtimeclassname
  - mutation controller
- Created runtimeclass admission controller
  - PodOverhead - Kubelet and eviction handling for pod scheduling
- PodOverhead added to admission controller
- As a user severely constrained or over-constrained, inconsistent, poor
  performance
- https://kubernetes.io/docs/concepts/configuration/pod-overhead/
- issue: https://github.com/kubernetes/enhancements/issues/688
  - overhead is static; network io; vCpu overhead
- alpha in k8s 1.6
- relevant in the sandbox environment

## session 4: weighing a cloud; k8s instrumentation
- stackdriver: prometheus client;
  - prometheus data model: timeseries; up value 1;
    - Help text
    - type text
    - types of metric values:
      - counters
      - gauges
      - summaries
      - histograms (raw)
- dimensions of measurements
  - availability
  - latency (apiserver_request_latency_seconds)
  - capacity (load distribution)
  - errors (apiserver_dropped_requests_total)
- PromQL powers metrics analysis and aggregation
  - prototyping and exploration; (prometheus UI)
  - attach Prom to Grafana
  - query the Prom API
- troubleshooting; A node is offline
  - Full-stack debugging
  - observability
- Kubernetes control plan
  - spoke and hubs model
  - node -> kubelete -> pod
  - master node
    - kubelet health check endpoints
    - metrics/logs
  - two master components
    - kubeAPI and etcd
    - KAS (http API)
  - `kubectl <command> -v=9` introspect to KAS (kubeAPI server)
    - audit-logs
    - health endpoints: localhost:8080/healthz?verbose, localhost:8080/livez, and
      .../readyz
    - /metrics
    - kube-apiserver.log
  - etcd distributed key/value store
    - etcd stores data about the cluster
    - `etcdctl`, `auger`, `/metrics`, `/health`, `etcd.log`
- troubleshooting
  - detection strategies:
    - metrics/probes
    - kubelet repair mode/initiated crashloop
  - `curl .../healthz?verbose`
  - `etcd_object_counts` metric number of stored objects at the time of last
    check split by kind
  - default `etcd` size limit: 2GB
  - `apiserver_requeust_count{'somecrd'}`
  - size vs. object count vs. version
  - `auger extract -f <dbfile> -k <key> | wc -c`
- SIG instrumentation deepdive
