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

