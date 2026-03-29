# Aerospike CE Ecosystem

Aerospike Community Edition을 위한 오픈소스 통합 도구 에코시스템.

Kubernetes 선언적 배포 · 고성능 Python 클라이언트 · 웹 관리 UI · AI 개발 지원 — CE 운영에 필요한 전체 스택을 제공합니다.

```
┌─────────────────────────────────────────────────────────────────┐
│                        사용자 / DevOps                          │
└──────┬────────────────────┬─────────────────────┬──────────────┘
       │ HTTP               │ kubectl              │ CLI
┌──────▼──────┐    ┌────────▼────────┐    ┌───────▼───────┐
│   Cluster   │    │      ACKO       │    │   Claude Code │
│   Manager   │    │  (K8s Operator) │    │    Plugins    │
│  Next.js +  │    │  AerospikeCluster   │    │  Skills +     │
│  FastAPI    │    │  CRD + Helm     │    │  Agent        │
└──────┬──────┘    └────────┬────────┘    └───────────────┘
       │ aerospike-py        │ StatefulSet
┌──────▼──────┐    ┌────────▼────────┐
│ aerospike-py│    │  Aerospike CE   │
│ Rust/PyO3   ├───►│  Server 8.1     │
│ Client      │    │  (Pod)          │
└─────────────┘    └─────────────────┘
```

## Projects

### aerospike-py &ensp; `v0.0.4` &ensp; [![Python](https://img.shields.io/badge/Python-3.10+-blue)](https://pypi.org/project/aerospike-py/) [![Rust](https://img.shields.io/badge/Rust-PyO3-orange)](https://github.com/aerospike-ce-ecosystem/aerospike-py)

Rust(PyO3) 기반 고성능 Aerospike Python 클라이언트. GIL-free async, CDT, Expression Filters, NumPy batch, OpenTelemetry 통합.

```python
from aerospike_py import Client
client = Client(hosts=[("localhost", 3000)])
client.put(("ns", "set", "key"), {"name": "hello", "count": 42})
record = client.get(("ns", "set", "key"))
```

[GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-py) · [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-py/) · [PyPI](https://pypi.org/project/aerospike-py/)

---

### ACKO (Aerospike CE Kubernetes Operator) &ensp; `v0.1.7` &ensp; [![Go](https://img.shields.io/badge/Go-Kubebuilder_v4-00ADD8)](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator)

`AerospikeCluster` CRD로 Aerospike CE 클러스터를 선언적으로 배포·관리. Rack-aware 배포, 자동 rolling restart, CE 제약 validation webhook 내장.

```yaml
apiVersion: acko.aerospike-ce-ecosystem.io/v1alpha1
kind: AerospikeCluster
metadata:
  name: my-cluster
spec:
  size: 3
  image: aerospike/aerospike-server-ce:8.1
```

[GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator) · [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/)

---

### Cluster Manager &ensp; [![TypeScript](https://img.shields.io/badge/Next.js_16-TypeScript-black)](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager) [![FastAPI](https://img.shields.io/badge/FastAPI-Python-009688)](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager)

웹 기반 Aerospike 관리 UI. 클러스터 모니터링, Record 브라우저, Query 빌더, K8s 클러스터 관리, Config Diff Viewer.

[GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager) · [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/guide/cluster-manager-ui)

---

### CE Ecosystem Plugins &ensp; [![Claude Code](https://img.shields.io/badge/Claude_Code-Plugin-blueviolet)](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins)

Claude Code 플러그인 — 5 Skills + 1 Agent. ACKO 배포 가이드, aerospike-py API 레퍼런스, 클러스터 자율 디버깅 에이전트.

[GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins)

---

### Project Hub &ensp; [![Docusaurus](https://img.shields.io/badge/Docusaurus-Docs-green)](https://aerospike-ce-ecosystem.github.io/project-hub/)

에코시스템 중앙 문서 허브. 아키텍처 다이어그램, ADR, 프로젝트 타임라인, PR 히스토리, 릴리스 매트릭스.

[GitHub](https://github.com/aerospike-ce-ecosystem/project-hub) · [Docs](https://aerospike-ce-ecosystem.github.io/project-hub/)

## Tech Stack

| Layer | Technologies |
|-------|-------------|
| **Client** | Rust, PyO3 0.28, tokio, Python 3.10+ |
| **Operator** | Go, Kubebuilder v4, controller-runtime, Helm |
| **Web UI** | Next.js 16, React 19, Tailwind CSS 4, FastAPI |
| **Infra** | Podman, Kubernetes, Aerospike CE 8.1 |
| **AI** | Claude Code Skills, Agents, Hooks |
| **Docs** | Docusaurus, ReactFlow, Mermaid |

## License

All projects are licensed under [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
