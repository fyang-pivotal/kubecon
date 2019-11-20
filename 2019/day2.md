## Morning keynotes

## Session 1; Binary authorization for container images
- @asyslu22 Aysylu Greenberg, Liron Levin
- layout
  - why we need binary authorization
  - imporve the security of k8s cluster
  - open source tech
  - demo
- software supply chain
  - security audit for container image OS
    - which images are deployed right now?
  - require images to be signed by thrusted authorities
    - QA, DevOps, Security tools
  - Open sources
    - [`grafeas`](https://github.com/grafeas/grafeas), [ `kritis` ](https://github.com/grafeas/kritis)
    - Kritis admission controller for policy enforcement
      - sample spec:
      - `ImageSecurityPolicy`
      - Spec
        - `allowlistCVEs`
- Roadmap
  - More metadata: license, static tests

## Sesssion 2: Prometheus; Observability; DigitalOcean
- Observability for thoughtful Pattern
- Difficult without core understanding and experience
- Prometheus HA
  - constant resource utilization
  - gap in metrics
    - no scraping means no alerting!
- Deploy replicas and protect Prometheus
  - smart proxy (HAProxy)
  - switch instances for queries
  - query logging
- Thanos-query, promxy fan out
- Cardinality of data
  - Avoid creating metrics with unbounded labels
  - higher cardinality queries - `protec`
    - `--query.max-samples` and `--query.max-concurrency`
- Standardized metrics better than bespoke bash scripts
- Exporters
  - Use `ConstMetrics` for getting metrics at scrape time
- Alert
  - Make it the interaction and generate the config
- Establishing SLOs
  - Quantifying it is hard
  - Average overtime (<99%)

## VMware booth demo; `dapr` https://github.com/dapr/cli

## Afternoon session #3: SIG lifecycle intro
- develop the toolings necessary to build a highly automated meta cloud
  - use k8s native components to manage k8s cluster lifecycle
  - avoid the pitfalls of yesteryear
- the stack
  - `etcdadm`, `kubeadm`, `cluster-addons`
  - bootstrap control plane easily
  - clusterAPI: declarative manifest
  - image stamper
- unix philosophy
  - make each programs do one thing well
  - `kubeadm`
    - official tool to bootstrap
    - roadmap
      - simplify day 2 operations (kubeadm operator)
    - client/library layout
    - move `kubeadm` out of the tree
    - `kubeadm` deepdive
  - `clusterAPI`
    - roadmap
      - build/test automation
- different provisioner
  - `minikube`, `kops`
- how to get involved
  - contributor onboarding page
  - slack channels for cluster-api

## session #4; fluent-bit
- fluent-bit
  - stable release: 1.3.3
  - [stream processing](https://docs.fluentbit.io/manual/configuration/stream_processor)
    - CREATE stream syntax
    - stream window
    - buffer output to stream processor
    - looks like another input plugin
    - subkeys operation enabled
    - define a new stream_file_config
  - snapshot creation based on criteria
  - config map: validation of configuration
  - add support for multi-line as json objects





