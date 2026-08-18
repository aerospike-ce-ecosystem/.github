# Security Policy

This policy applies to every repository in the
[aerospike-ce-ecosystem](https://github.com/aerospike-ce-ecosystem) organisation.
It is stored in the org-level `.github` repository, so any repository that does
not publish its own `SECURITY.md` inherits this one.

## Reporting a vulnerability

**Do not open a public issue, discussion, or pull request for a security
problem.** A public report tells everyone about the vulnerability before there
is a fix, including the people you would least like to tell.

Use one of these two channels instead.

### 1. GitHub private vulnerability reporting (preferred)

Go to the affected repository's **Security** tab → **Report a vulnerability**,
or open this URL with the repository name substituted:

```
https://github.com/aerospike-ce-ecosystem/<repository>/security/advisories/new
```

This creates a private advisory that only you and the maintainers can see. It
keeps the whole exchange, the draft fix, and the eventual CVE in one place, and
it needs no email round-trip.

> **If that page returns 404**, private vulnerability reporting has not been
> enabled for that repository yet. Use the email channel below — do not fall
> back to a public issue.

### 2. Email

Email **KimSoungRyoul@gmail.com** with `[SECURITY]` in the subject line. This
route needs no GitHub account.

### What to include

Whichever channel you use, a report is far easier to act on when it contains:

- **Which product and version** — for example `aerospike-py 0.14.1`,
  `ACKO v1.10.3`, `ackoctl v0.3.4`. See [Collecting version
  information](#collecting-version-information) below.
- **What the impact is** — data disclosure, privilege escalation, denial of
  service, remote code execution, and who is exposed to it.
- **How to reproduce it** — the smallest sequence of steps or a minimal script.
  A proof of concept is welcome; please keep it pointed at your own
  infrastructure.
- **Your environment** — operating system, Python/Go/Node version, Kubernetes
  version, Aerospike CE server version, deployment shape.
- **A suggested fix**, if you have one. Optional and appreciated.

### What happens next

- **Acknowledgement within 48 hours** that the report arrived and is being
  looked at.
- An assessment of severity and affected versions, shared with you.
- A fix prepared privately, then released.
- Public disclosure coordinated with you. If you want credit, say so and how
  you would like to be named; if you would rather stay anonymous, that is fine
  too.

If 48 hours pass with no acknowledgement, please send a follow-up — the first
message may have been missed.

## Supported versions

Every product in this organisation releases from `main`; there are no
long-lived maintenance or LTS branches. Security fixes therefore land in
`main` and go out in the **next release of the affected product**. Only the
latest release line receives fixes.

| Product | Where fixes are released |
|---|---|
| [aerospike-py](https://github.com/aerospike-ce-ecosystem/aerospike-py) | PyPI + GitHub Releases |
| [ACKO](https://github.com/aerospike-ce-ecosystem/aerospike-ce-kubernetes-operator) | GHCR container images + Helm chart (OCI) |
| [aerospike-cluster-manager](https://github.com/aerospike-ce-ecosystem/aerospike-cluster-manager) | GHCR container images |
| [ackoctl](https://github.com/aerospike-ce-ecosystem/ackoctl) | GitHub Releases + Homebrew tap |
| [aerospike-ce-ecosystem-plugins](https://github.com/aerospike-ce-ecosystem/aerospike-ce-ecosystem-plugins) | GitHub Releases |

If you are running an older release, the remedy is to upgrade. Back-porting to
an older line is not offered.

## Scope

**In scope** — code, configuration, and published artefacts of the
organisation's own repositories:

- `aerospike-py` — the Python package and its Rust native extension
- `aerospike-ce-kubernetes-operator` (ACKO) — operator, CRDs, admission
  webhooks, Helm charts
- `aerospike-cluster-manager` — FastAPI backend and Next.js UI
- `ackoctl` — the CLI, its installer script, and the Homebrew formula it
  publishes
- `aerospike-ce-ecosystem-plugins` — the Claude Code skills
- the supporting repositories: `project-hub`, `workspace`, `homebrew-tap`,
  `aerospike-py-performance-report`, and this `.github` repository

**Out of scope** — report these upstream, not here:

- [Aerospike Server](https://github.com/aerospike/aerospike-server) and
  Aerospike Community Edition itself
- [Aerospike Rust client](https://github.com/aerospike/aerospike-client-rust),
  which `aerospike-py` builds on
- Kubernetes, Helm, Podman, Docker, and other third-party dependencies. If a
  vulnerable dependency is *pinned* by one of our repositories, that pin is in
  scope even when the flaw itself is upstream — tell us and we will bump it.

Also out of scope, because they are documented behaviour rather than defects:

- Aerospike **Community Edition has no authentication, authorisation, TLS, or
  encryption-at-rest**. An unauthenticated CE server is CE working as designed,
  not a vulnerability in these tools. ACKO's admission webhook deliberately
  rejects Enterprise-only security settings.
- Findings from automated scanners with no demonstrated exploit path.
- Missing hardening headers or best-practice deviations with no security
  consequence you can articulate.

## Collecting version information

```bash
# aerospike-py
python -c "import aerospike_py; print(aerospike_py.__version__)"

# ackoctl
ackoctl version

# ACKO (operator deployment; `aerospike-operator` is the namespace the
# documented `helm install` uses — substitute yours if you changed it)
kubectl -n aerospike-operator get deploy -o jsonpath='{.items[*].spec.template.spec.containers[*].image}'

# Cluster Manager (container image tag)
kubectl -n <namespace> get deploy -o jsonpath='{.items[*].spec.template.spec.containers[*].image}'

# Aerospike CE server
asinfo -v build
```

## Safe harbour

We will not pursue or support legal action against anyone who, in good faith:

- reports a vulnerability through one of the private channels above,
- limits testing to systems they own or are authorised to test,
- avoids accessing, modifying, or destroying data belonging to anyone else,
- avoids degrading the availability of anyone else's systems, and
- gives us a reasonable opportunity to fix the issue before disclosing it
  publicly.

This is not a paid bug bounty programme. There is no monetary reward, but
credit is offered gladly.

## Related

- [Contributing guide](./CONTRIBUTING.md) — for non-security bugs and changes
- [Code of Conduct](./CODE_OF_CONDUCT.md)
- The `bug-reporter` skill in `aerospike-ce-ecosystem-plugins` is an
  AI-assisted path for **ordinary** bug reports from Claude Code users. It is
  not a security channel and does not create private reports.
