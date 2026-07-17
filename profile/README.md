# Aerospike CE Ecosystem

An open-source integrated tooling ecosystem for Aerospike Community Edition.

Declarative Kubernetes deployment · High-performance Python client · Web management UI · CLI control plane · AI development support — a complete stack for operating Aerospike CE.

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)
[![Project Hub](https://img.shields.io/badge/docs-Project%20Hub-6f42c1)](https://aerospike-ce-ecosystem.github.io/project-hub/)

> 📋 **[Project Hub](https://aerospike-ce-ecosystem.github.io/project-hub/)** — The central hub for the ecosystem. Architecture diagrams, ADRs, roadmap, and PR history for all five projects in one place. ([GitHub](https://github.com/aerospike-ce-ecosystem/project-hub))

## Core Projects

| Project | Description | Release | Links |
|---------|-------------|:-------:|-------|
| **[Ecosystem Plugins](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins)** | Claude Code plugin — ACKO deploy/ops/debug guides, aerospike-py API reference, ackoctl usage, and a bug reporter. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins/releases/latest) | [GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins) |
| **[aerospike-py](https://github.com/aerospike-ce-ecosystem/aerospike-py)** | High-performance Python client built on Rust bindings. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-py?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-py/releases/latest) | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-py/) · [PyPI](https://pypi.org/project/aerospike-py/) |
| **[ACKO](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator)** | Aerospike CE Kubernetes Operator. Declarative cluster management via CRDs. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator/releases/latest) | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/) |
| **[Cluster Manager](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager)** | Web-based Aerospike management UI with monitoring, record browser, and query builder. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/aerospike-cluster-manager?color=green)](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager/releases/latest) | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/guide/cluster-manager-ui) |
| **[ackoctl](https://github.com/aerospike-ce-ecosystem/ackoctl)** | Go CLI for Cluster Manager. Manage connections, records, queries, and ACKO reconciliations from terminal or CI. | [![release](https://img.shields.io/github/v/release/aerospike-ce-ecosystem/ackoctl?color=green)](https://github.com/aerospike-ce-ecosystem/ackoctl/releases/latest) | [Install](https://github.com/aerospike-ce-ecosystem/ackoctl#install) |

## Quick Start

Each block expands. A common path: install the **plugins** for AI-assisted development, use **aerospike-py** to talk to a cluster, deploy one with **ACKO**, then drive it from the **Cluster Manager** UI or **ackoctl**.

<details>
<summary><b>1 · Ecosystem Plugins</b> — install the Claude Code plugin</summary>

<br />

```bash
# Add the marketplace, then install
claude plugin marketplace add aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins
claude plugin install aerospike-ce-ecosystem

# Verify
claude plugin list   # aerospike-ce-ecosystem@aerospike-ce-ecosystem ✔ enabled
```

Bundles 9 skills — `acko-deploy`, `acko-operations`, `acko-config-reference`, `acko-e2e-test`, `acko-debugging`, `ackoctl`, `aerospike-py-api`, `aerospike-py-fastapi`, and `bug-reporter` — so Claude Code can deploy, operate, and debug the whole stack. See the [plugin repo](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins) for details.

</details>

<details>
<summary><b>2 · aerospike-py</b> — install the client and connect</summary>

<br />

[![PyPI](https://img.shields.io/pypi/v/aerospike-py.svg)](https://pypi.org/project/aerospike-py/)
[![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/downloads/)

```bash
pip install aerospike-py
```

```python
import asyncio
from aerospike_py import AsyncClient

async def main():
    async with AsyncClient({"hosts": [("127.0.0.1", 3000)]}) as client:
        await client.connect()

        key = ("test", "demo", "user1")
        await client.put(key, {"name": "Alice", "age": 30})

        record = await client.get(key)
        print(record.bins)  # {'name': 'Alice', 'age': 30}

asyncio.run(main())
```

A synchronous `client(...)` API and NumPy batch reads are also available. Full guide: [documentation](https://aerospike-ce-ecosystem.github.io/aerospike-py/).

</details>

<details>
<summary><b>3 · ACKO</b> (aerospike-ce-k8s-operator) — spin up a CE cluster on Kubernetes</summary>

<br />

**1. Create a local Kubernetes cluster**

```bash
kind create cluster
# or
minikube start
```

**2. Install the operator**

```bash
helm install cert-manager jetstack/cert-manager -n cert-manager --create-namespace \
  --set crds.enabled=true --repo https://charts.jetstack.io --wait

helm install -n aerospike-operator --create-namespace acko \
  oci://ghcr.io/aerospike-ce-ecosystem/charts/aerospike-ce-kubernetes-operator
```

**3. Deploy an Aerospike cluster (one-command apply)**

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

**4. Open the Cluster Manager UI**

```bash
kubectl port-forward -n aerospike-operator \
  svc/acko-aerospike-ce-kubernetes-operator-ui-web 3100:3100
```

Then open [http://localhost:3100](http://localhost:3100) to browse the cluster you just deployed.

</details>

<details>
<summary><b>4 · Cluster Manager</b> — the web management UI</summary>

<br />

Cluster Manager ships with ACKO and is deployed by the operator (step 3 above). To reach it, port-forward the web service and open it in a browser:

```bash
kubectl port-forward -n aerospike-operator \
  svc/acko-aerospike-ce-kubernetes-operator-ui-web 3100:3100
# open http://localhost:3100
```

From there you get live monitoring, a record browser, a query builder, and Kubernetes cluster management. Guide: [Cluster Manager UI docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/guide/cluster-manager-ui).

</details>

<details>
<summary><b>5 · ackoctl</b> — drive Cluster Manager from the terminal</summary>

<br />

```bash
# Install (Linux & macOS)
curl -fsSL https://raw.githubusercontent.com/aerospike-ce-ecosystem/ackoctl/main/install.sh | sh
# or, on macOS:
brew install aerospike-ce-ecosystem/tap/ackoctl
```

```bash
# Point it at the Cluster Manager API (port-forward the API service)
kubectl port-forward -n aerospike-operator \
  svc/acko-aerospike-ce-kubernetes-operator-ui-api 8000:80

# Register and use a context, then list managed clusters
ackoctl config set-context kind-local \
  --server=http://localhost:8000/api \
  --workspace-id=default
ackoctl config use-context kind-local
ackoctl k8s cluster list
```

Manage connections, records, queries, secondary indexes, and ACKO reconciliations from the shell or CI. Full command reference: [ackoctl usage](https://github.com/aerospike-ce-ecosystem/ackoctl/blob/main/docs/usage.md).

</details>

## License

All projects are licensed under [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
