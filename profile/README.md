# Aerospike CE Ecosystem

Aerospike Community Edition를 위한 오픈소스 도구 에코시스템입니다.

Kubernetes 배포·운영부터 Python 클라이언트, 웹 관리 UI, AI 개발 지원까지 — CE 사용자에게 필요한 통합 도구 체인을 제공합니다.

## Projects

| Project | Description | Links |
|---------|-------------|-------|
| **[aerospike-py](https://github.com/aerospike-ce-ecosystem/aerospike-py)** | Rust(PyO3) 기반 고성능 Aerospike Python 클라이언트. Sync/Async API, CDT, Expression Filters, 타입 스텁 완전 지원. 기존 aerospike-client-python의 drop-in replacement. | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-py/) · [PyPI](https://pypi.org/project/aerospike-py/) |
| **[ACKO](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator)** | Aerospike CE Kubernetes Operator. `AerospikeCluster` CRD로 클러스터 배포, 스케일링, 롤링 업데이트, Rack-aware 배포를 선언적으로 관리. | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/) |
| **[Cluster Manager](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager)** | 웹 기반 Aerospike 관리 UI. 클러스터 모니터링, Record 브라우저, Query 빌더, AQL 터미널, K8s 클러스터 관리 지원. FastAPI + Next.js. | [Docs](https://aerospike-ce-ecosystem.github.io/aerospike-ce-kubernetes-operator/guide/cluster-manager-ui) |
| **[CE Ecosystem Plugins](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins)** | Claude Code 플러그인. ACKO 배포 가이드, aerospike-py API 레퍼런스, 클러스터 디버깅 에이전트 등 AI 어시스턴트 연동. | [GitHub](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins) |
