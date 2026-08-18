# Project Goals

Development direction and constraints for each repository in the Aerospike CE
Ecosystem.

## Scope of this document

This file covers **every repository in the organisation**, so that a reader can
tell at a glance whether a given repo is a supported product, a supporting tool,
or archived. It states direction and constraints — the "do not do this" as much
as the "do this".

For the *why* behind the ecosystem — the motivation, the ecosystem-wide
commitments, and the longer per-project narrative for the five core products —
see [Project Goals in Project
Hub](https://aerospike-ce-ecosystem.github.io/project-hub/docs/goals/project-goals),
which is the fuller document. This file is the complete inventory; that one is
the argument. When they disagree about a core product, Project Hub is correct
and this file needs fixing.

| Repository | Tier | Section |
|---|---|---|
| [aerospike-py](https://github.com/aerospike-ce-ecosystem/aerospike-py) | Core product | [1](#1-aerospike-py) |
| [aerospike-cluster-manager](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager) | Core product | [2](#2-aerospike-cluster-manager) |
| [aerospike-ce-kubernetes-operator](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator) | Core product | [3](#3-acko-aerospike-ce-kubernetes-operator) |
| [aerospike-ce-ecosystem-plugins](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins) | Core product | [4](#4-aerospike-ce-ecosystem-plugins) |
| [ackoctl](https://github.com/aerospike-ce-ecosystem/ackoctl) | Core product | [5](#5-ackoctl) |
| [project-hub](https://github.com/aerospike-ce-ecosystem/project-hub) | Supporting | [6](#6-project-hub) |
| [workspace](https://github.com/aerospike-ce-ecosystem/workspace) | Supporting | [7](#7-workspace) |
| [homebrew-tap](https://github.com/aerospike-ce-ecosystem/homebrew-tap) | Supporting | [8](#8-homebrew-tap) |
| [aerospike-py-performance-report](https://github.com/aerospike-ce-ecosystem/aerospike-py-performance-report) | Supporting | [9](#9-aerospike-py-performance-report) |
| [.github](https://github.com/aerospike-ce-ecosystem/.github) | Supporting | [10](#10-github-org-repository) |
| [aerospike-ce-ui-kit](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ui-kit) | **Archived** | [11](#11-aerospike-ce-ui-kit-archived) |

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

2-7. **Backend ↔ Frontend type synchronization** — Backend Pydantic models in `api/` and the frontend TypeScript types in `ui/src/lib/types/` must not drift apart.
  - **Today this is manual.** Whenever a Pydantic model changes, update the matching TypeScript type in the same PR. There is no generator and no CI check; `aerospike-cluster-manager/CLAUDE.md` records the same rule.
  - **The direction is codegen.** [ADR-0019](https://aerospike-ce-ecosystem.github.io/project-hub/docs/architecture/adr/2026-03-30-codegen-type-sync) proposes generating the TypeScript types from the FastAPI OpenAPI schema via `openapi-typescript`, tracked in [aerospike-cluster-manager#165](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager/issues/165). Once that lands, the manual rule above is replaced by "regenerate, do not hand-edit", and the generated file must not be edited directly.
  - Until it lands, treat the manual rule as binding. Do not remove the manual sync obligation on the strength of a proposal.

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

4-2. **Debugging skill accuracy** — Keep the `acko-debugging` skill accurate and effective for real-world ACKO troubleshooting scenarios: pod failures, deployment errors, and cluster issues.

4-3. **The plugin ships skills, not agents** — There is no `agents/` directory and none should be reintroduced without a decision to that effect. The `acko-cluster-debugger` agent was demoted to the `acko-debugging` skill in `f66a4c0` (tag `v1.3.1`, 2026-05-08), which deleted `agents/acko-cluster-debugger.md` and removed the now-empty directory. Anything that still names `acko-cluster-debugger` outside a changelog is stale and should be fixed.

4-4. **Keep the advertised inventory honest** — The skill list in the plugin README, in `.github/profile/README.md`, and in project-hub's plugin docs must match the directories under `skills/`. The current set is nine: `acko-config-reference`, `acko-debugging`, `acko-deploy`, `acko-e2e-test`, `acko-operations`, `ackoctl`, `aerospike-py-api`, `aerospike-py-fastapi`, `bug-reporter`.

---

## 5. ackoctl

> Go CLI for the ACKO stack — terminal and CI control plane

5-1. **Stay a REST client of Cluster Manager** — ackoctl calls Cluster Manager's FastAPI surface at `/api/v1/*`. It does **not** talk to Kubernetes or to Aerospike directly, and must not grow a second access path. Add a new capability by confirming the endpoint shape against a running server, then types → client method → cobra command → tests at both the HTTP and command level.

5-2. **Hold the `<noun> <verb>` grammar and machine-readable output** — Commands read `ackoctl connection list`, gh/aws style, not `ackoctl get connections`. Default output is a table; every list and get command must also offer `-o json` and `-o yaml`, and those must round-trip faithfully — no silently dropped fields, no case mangling.

5-3. **Keep install and self-upgrade verifiable** — The installer script and `ackoctl upgrade` both verify the release archive against `checksums.txt` before installing. A missing checksum file, a missing entry, or a missing hashing tool is a fatal error, never a warn-and-continue.

5-4. **Explicit workspace scoping, bring-your-own token** — Every resource command honours `--workspace` and must never silently fall back to "the first workspace". Authentication is a bearer token the user supplies; there is deliberately no interactive `ackoctl login`, and config containing tokens is written atomically with `0600` permissions.

5-5. **Reach a v1.0 stability promise** — Until then the CLI surface may change between minors. `admin` commands are knowingly non-functional against Community Edition, which ships no security module; keep that clearly signposted rather than quietly failing.

---

## 6. project-hub

> Cross-repo documentation and issue-tracking hub. Documentation only — no product code

6-1. **Be the single source of truth for ADRs, goals, roadmap, and release history** — Architecture decisions, per-project goals, quarterly roadmaps, release history, and the compatibility matrix live here rather than being duplicated per repo. A decision recorded anywhere else is a copy, not the record.

6-2. **Never ship a broken docs build or a dead link** — The Docusaurus build runs on every PR touching `docs/**` with `onBrokenLinks: 'throw'`, and deploys to GitHub Pages on merge. A doc change that does not build does not merge.

6-3. **Keep every document frontmatter-navigable** — Each page carries `title`, `description`, `scope`, `repos`, `tags`, and `last_updated`, so a reader (or a tool) can judge relevance and filter by repository without reading the body. `last_updated` must reflect the content, not the last time someone touched the file.

6-4. **Keep the Release Compatibility Matrix trustworthy** — It is the page operators use to choose versions, so a stale entry is worse than no entry. Current-version data must be generated from release tags rather than transcribed by hand, and the docs build must fail when the freshness stamp drifts ahead of the content. The hand-maintained compatibility rows gain a row only when an integration test actually happened — never by inferring one from release dates.

6-5. **Diagram conventions are fixed** — ReactFlow for architecture and flow diagrams; Mermaid only where ReactFlow cannot express the shape. Diagrams follow the four-layer vertical convention: user → tools → management → infrastructure.

---

## 7. workspace

> Meta-repository — one clone to get the whole ecosystem, via git submodules

7-1. **One-command onboarding** — `git clone --recursive` plus `make init` gets a contributor a working environment for every project. `make doctor` checks the required toolchain (uv, podman, kind, rust, go, node, pre-commit) rather than letting a build fail obscurely later.

7-2. **Keep submodule pins fresh without human babysitting** — Submodules are bumped automatically in dependency order, one per cycle, each as its own PR that auto-merges only once verification is green. A pin that has drifted months behind defeats the point of the meta-repo.

7-3. **Stay a thin smoke gate** — Workspace CI runs workspace-level checks only; each submodule runs its own deeper CI in its own repository. Do not migrate per-project test suites here. The `verify` job name is referenced by branch protection and by the auto-merge gate, so it must not be renamed casually.

7-4. **Own the cross-repo dependency and merge order** — The chain is `aerospike-py → ACKO → cluster-manager → plugins`, and this repo documents it along with the impact table for API and CRD changes. Loose coupling is deliberate: every project stays independently usable, and integration is opt-in.

---

## 8. homebrew-tap

> Homebrew tap for the ecosystem's CLI tools

8-1. **Never hand-edit a formula** — `Formula/*.rb` is generated by goreleaser when a release is tagged in the source repository, and carries a `DO NOT EDIT` header. A hand edit will be silently overwritten by the next release; fix the generator input instead.

8-2. **Serve macOS Intel, macOS ARM, and Linux from checksum-pinned archives** — Each platform block carries its own URL and SHA-256 taken from the release, plus a smoke test that runs the installed binary.

8-3. **Be the second supported install path** — Homebrew and the checksum-verifying curl installer are the two supported ways to install `ackoctl`. There are deliberately no apt or yum repositories, so these two must both keep working.

8-4. **Correctness is inherited, not tested here** — This repo has no CI and needs none; the guarantee comes from the source repo's release pipeline. Adding logic here that is not generated would break that property.

---

## 9. aerospike-py-performance-report

> Analysis and animated visualisation of why aerospike-py and the official C-binding client differ in performance

9-1. **Explain mechanisms, not scores** — The report attributes the performance difference to specific named causes — I/O model, GIL hold time, GIL management pattern, query/scan callback pattern, async client structure, Python↔native boundary cost, allocation pattern — and argues that GIL *contention frequency* and the I/O model, not GIL hold time itself, are the real bottleneck.

9-2. **Ground every claim in real source on both sides** — Claims cite the official client's C source and aerospike-py's Rust source. A claim that cannot be pointed at code does not belong here.

9-3. **Be honest about where there is no advantage** — The scenario table says plainly that single-threaded single-record `get` is roughly equal, and concentrates the claimed advantage in high-concurrency, batch, query/scan, and asyncio workloads. Do not let the visualisation oversell.

9-4. **Stay a self-deploying static site** — Vite + React, published to GitHub Pages on merge. This is a report, not a library: it is never published to npm, and measured benchmark work belongs in aerospike-py and its ADRs rather than here.

---

## 10. .github (org repository)

> Organisation profile page, org-wide community health files, and this document

10-1. **Give a first-time visitor one runnable path through the stack** — The profile README's Quick Start sequences plugins → aerospike-py → ACKO → Cluster Manager / ackoctl with copy-pasteable commands. Its value is that the commands actually work when followed in order.

10-2. **Keep the project table accurate** — Every repository listed as a Core Project in the profile README must also appear as a Core Product section in this document. The two files in this repository disagreeing about what counts as a first-class project is the failure this rule exists to prevent; that is what happened when `ackoctl` was added to the profile README but never to these goals.

10-3. **Supply the org-wide community health files** — `SECURITY.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, and `.github/ISSUE_TEMPLATE/` live here and are inherited by every repository that does not define its own, so a gap here is a gap in ten repositories at once.

10-4. **Route depth to Project Hub rather than duplicating it** — Architecture, ADRs, roadmap, and release history belong in project-hub. This repo holds the front door and the org-wide policy, and nothing that has to be kept in sync with code.

---

## 11. aerospike-ce-ui-kit (archived)

> Shared React component library — **archived, not maintained**

This repository is archived. It accepts no pull requests and runs no CI, and
its `dist/` directory is committed so existing git+ssh consumers keep working.

There is **no goal to revive it**. No repository in the organisation depends on
it — its role was taken over by the components maintained inside
`aerospike-cluster-manager` under `ui/src/components/`, which independently grew
its own `Accordion`, `Badge`, `Button`, `Card`, and the rest. It is listed here
so that its absence from the sections above reads as a decision rather than an
oversight. If shared components are ever wanted again, that is a new decision
and should get an ADR in project-hub before any code moves.
