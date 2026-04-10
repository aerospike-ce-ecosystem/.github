# Project Goals

Development direction and constraints for each project in the Aerospike CE Ecosystem.

---

## 1. aerospike-py

> Aerospike Python Rust binding async client

1-1. **Maintain high performance** — Always preserve the performance advantages provided by the Rust (PyO3) foundation.

1-2. **Track aerospike-client-rust v2** — Maintain working knowledge of [aerospike-client-rust v2](https://github.com/aerospike/aerospike-client-rust/tree/v2) via Skills. Evaluate adoption whenever a new version is released.

1-3. **Improve NumPy batch operations** — Continuously improve and maintain `batch_read_numpy` and `batch_write_numpy` so they work reliably as specified.

1-4. **Maintain observability** — Keep logging, metrics, and tracing (OpenTelemetry) in good shape. Verify correct behavior in FastAPI via the `sample-fastapi` example.

1-5. **batch_write retry** — Use the `batch_write(..., retry=10)` option to maximize client-side control over `batch_write` failures and prevent data loss.

---

## 2. aerospike-cluster-manager

> Web-based Aerospike cluster management UI (FastAPI + Next.js)

2-1. **Maintain frontend UI component reusability** — Manage shared component reuse carefully to keep the component library healthy.

2-2. **Backend read/write timeout and limit management** — Control pagination limits on data table queries and keep performance issues in check.

2-3. **ACKO UI cluster management** — Provide reliable control over ACKO-integrated Kubernetes cluster management and continuously improve UX convenience.

2-4. **No broad UI restructuring** — Do not arbitrarily change the overall layout or navigation structure.

2-5. **ACKO cluster creation convenience** — The wizard-based cluster creation flow must remain intuitive and easy to use.

2-6. **ACKO cluster management, browsing, and editing** — Both UX convenience and performance are equally important.

2-7. **Backend ↔ Frontend type synchronization** — Backend Pydantic models and frontend TypeScript types (`lib/api/types.ts`) must be kept in sync manually. Update both sides whenever a model changes.

2-8. **Record browser performance with large datasets** — Apply appropriate limits, pagination, and timeouts for scan/query operations so the UI remains stable even with large datasets.

---

## 3. ACKO (Aerospike CE Kubernetes Operator)

> Kubernetes Operator for Aerospike CE cluster lifecycle management

3-1. **Keep the CRD structure simple** — Manage clusters using standard Kubernetes resources (Pod, StatefulSet, Service, PVC, etc.). Do not add Custom Resources arbitrarily.

3-2. **Maintain and evolve Cluster Template intent**
  - **minimal**: for development
  - **soft-rack**: for staging environments (1 Node, N Pods)
  - **hard-rack**: for production environments (N Nodes, N Pods)
  - Preserve these semantics while iterating and improving each template.

3-3. **Helm chart version management** — Maintain a systematic versioning and release process for the Helm chart under `charts/aerospike-ce-kubernetes-operator/`.

3-4. **Expand E2E test coverage** — Extend Kind-based E2E test scenarios to cover operational cases such as scale, rolling update, ACL, and monitoring.

3-5. **CE constraint Webhook reliability** — Always keep CE constraint validations accurate: size ≤ 8, namespaces ≤ 2, no XDR/TLS, etc. Provide clear error messages when Enterprise features are attempted.

---

## 4. aerospike-ce-ecosystem-plugins

> Claude Code plugin — AI assistant integration

4-1. **Keep Skills up to date** — Reflect API and feature changes from each project in Skills promptly (e.g., aerospike-py API changes, ACKO CRD changes).

4-2. **Debugging agent accuracy** — Keep the ACKO cluster debugging agent (`acko-cluster-debugger`) accurate and effective for real-world troubleshooting scenarios.
