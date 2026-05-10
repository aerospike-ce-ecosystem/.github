# Aerospike CE Ecosystem

An open-source integrated tooling ecosystem for Aerospike Community Edition.

Declarative Kubernetes deployment · High-performance Python client · Web management UI · AI development support — a complete stack for operating Aerospike CE.

> 📋 **[Project Hub](https://aerospike-ce-ecosystem.github.io/project-hub/)** — The central hub for the ecosystem. Architecture diagrams, ADRs, roadmap, and PR history for all four projects in one place. ([GitHub](https://github.com/aerospike-ce-ecosystem/project-hub))

## Core Projects

| Project | Description | Release | Links |
|---------|-------------|:-------:|-------|
| **[aerospike-py](https://github.com/aerospike-ce-ecosystem/aerospike-py)** | High-performance Python client built on Rust bindings. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-py?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-py/releases/latest) | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-py/) · [PyPI](https://pypi.org/project/aerospike-py/) |
| **[ACKO](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator)** | Aerospike CE Kubernetes Operator. Declarative cluster management via CRDs. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator/releases/latest) | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/) |
| **[Cluster Manager](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager)** | Web-based Aerospike management UI with monitoring, record browser, and query builder. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-cluster-manager?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager/releases/latest) | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/guide/cluster-manager-ui) |
| **[Ecosystem Plugins](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins)** | Claude Code plugin with ACKO deployment guides, aerospike-py API reference, and a cluster debugging agent. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins/releases/latest) | [GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins) |

## ACKO Quick Start

Spin up a local Aerospike CE cluster in four steps.

### 1. Create a local Kubernetes cluster

```bash
kind create cluster
# or
minikube start
```

### 2. Install the operator

```bash
helm install cert-manager jetstack/cert-manager -n cert-manager --create-namespace --set crds.enabled=true --repo https://charts.jetstack.io --wait

helm install -n aerospike-operator --create-namespace acko \
  oci://ghcr.io/aerospike-ce-ecosystem/charts/aerospike-ce-kubernetes-operator
```

### 3-1. Open the Cluster Manager UI

```bash
kubectl port-forward -n aerospike-operator \
  svc/acko-aerospike-ce-kubernetes-operator-ui-web 3100:3100
```

Then open [http://localhost:3100](http://localhost:3100).

### 3-2. Deploy an Aerospike cluster (one-command apply)

```bash
kubectl apply -f - <<'EOF'
apiVersion: acko.io/v1alpha1
kind: AerospikeCluster
metadata:
  name: aerospike-basic
spec:
  size: 1
  image: aerospike:ce-8.1.1.1
  aerospikeConfig:
    namespaces:
      - name: test
        replication-factor: 1
        storage-engine:
          type: memory
          data-size: 1073741824
EOF
```

## License

All projects are licensed under [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
