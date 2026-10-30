# Container-Guard
# Project Charter: ContainerGuard

| Field | Details |
| :--- | :--- |
| **Project Name** | ContainerGuard |
| **Version** | 1.0 |
| **Date** | October 2026 |
| **Status** | Phase 0 – Approved |
| **Target Environment** | Ubuntu VPS  |

---

## 1. Executive Summary (The Build in One Sentence)
ContainerGuard is a small Go-based container-configuration auditing utility (`cg-audit`), paired with a deliberately simple Go test service (`demo-service`) and an automated DevSecOps delivery pipeline (`secure-delivery`), backed by documented threat models, vulnerability governance, signed artifacts, and incident recovery evidence.

---

## 2. Core System & Asset Inventory
| Component | Type | Purpose & Security Boundary |
| :--- | :--- | :--- |
| **`cg-audit`** | Go CLI Utility | Offline-first auditor that parses exported `docker inspect` JSON metadata to detect container misconfigurations without requiring privileged `/var/run/docker.sock` access. |
| **`demo-service`** | Go HTTP API | Minimal, private web service used exclusively as a controlled target for SAST, SCA, DAST, and container runtime hardening. |
| **`secure-delivery`** | CI/CD & Policy Engine | Automated pipeline enforcing secret scanning, vulnerability triage, SBOM generation, artifact signing, and policy-as-code gates. |
| **Lab Host** | Ubuntu VPS | Shared host environment where ContainerGuard runs inside dedicated directories, uniquely named containers, and isolated private networks. |

---

## 3. Scope Boundaries

### In-Scope (What Will Be Built)
* **Docker & Linux Operations:** Custom Dockerfiles, multi-container Compose stacks, volume persistence, private bridge networking, and container runtime hardening (non-root, dropped capabilities, read-only rootfs, resource limits).
* **Go Security Tooling:** Maintainable, unit-tested Go CLI with structured JSON and human-readable reporting that handles malformed inputs safely.
* **DevSecOps Pipeline & Supply Chain:** SAST (`gosec`/`Semgrep`), SCA (`govulncheck`/`Trivy`), secret detection (`Gitleaks`), DAST (`OWASP ZAP`), SBOMs (`Syft`), signing (`Cosign`), and policy gates (`OPA`/`Conftest`).
* **Runtime & Incident Recovery:** Telemetry collection, failure playbooks, backup/restore rehearsals, and rollback validation.

### Out-of-Scope (What Will NOT Be Built)
* No bespoke uptime dashboard, feature-rich frontend UI, user authentication portal, or billing system.
* No active scanning or exploitation of third-party or public targets; all vulnerable tests remain strictly inside a private, non-public lab.
* No destructive host changes, blanket `docker system prune` commands, or interference with pre-existing production workloads on the VPS.
* No mandatory Kubernetes deployment until the core 16-mission Docker/DevSecOps capstone is completed.

---

## 4. Laboratory Governance & Rules of Engagement
1. **Workload Isolation:** Every experiment uses a dedicated Compose project name, isolated bridge networks, and bounded resource limits (`cpus`, `mem_limit`) to protect existing VPS services.
2. **No Uncontrolled Exposure:** Vulnerable test containers and databases must never bind to `0.0.0.0`. All test services remain on internal networks or bound strictly to `127.0.0.1`.
3. **Daemon Protection:** Never mount `/var/run/docker.sock` into untrusted or publicly reachable containers.
4. **Secret Hygiene:** Zero real tokens, private keys, or sensitive host logs may be committed to the Git repository.

---

## 5. Primary Learning & Portfolio Outcomes
* **Docker/Linux (25%):** Operate and troubleshoot images, containers, volumes, networks, and Linux isolation boundaries without blind copy/pasting.
* **Go for Security (20%):** Write a deterministic, unit-tested CLI auditor that evaluates container metadata and fails safely on malformed input.
* **Security Risk & AppSec (25%):** Produce a STRIDE threat model, validate SAST/DAST/SCA findings using reachability and EPSS/KEV context, and track remediations or expiring exceptions.
* **Delivery & Supply-Chain Trust (20%):** Enforce automated CI/CD gates that block unsafe commits and verify SBOMs, image digests, and cryptographic signatures.
* **Operations & Communication (10%):** Execute and document a repeatable incident detection, containment, and recovery rehearsal.


