
# InfraGuard — Zero-Trust Infrastructure Security Lab

> **Status: Planning / in progress.** This repository documents a proposed local security lab. Features listed under **Planned features** are not implemented or validated yet.

## Overview

InfraGuard is a portfolio project to explore how infrastructure services can enforce zero-trust principles: authenticated service identities, least-privilege access, network segmentation, secrets handling, and security monitoring.

The initial lab will run locally on an Apple Silicon MacBook Air with 8 GB RAM. The design intentionally favors a small number of lightweight services over a full Kubernetes cluster.

## Goals

- Understand how to separate application services across container networks.
- Authenticate service-to-service requests and enforce authorization outside business logic.
- Limit privileged operations and protect configuration secrets.
- Produce auditable security events and automated tests for both allowed and denied requests.
- Document threat models, test results, and design tradeoffs.

## Planned stack

- Python and FastAPI
- Docker Compose and container networking
- SQLite for the first prototype (option to migrate later)
- Pytest
- Git and GitHub

These are intended technologies, not a claim that they are all configured today.

## Proposed architecture

```text
Client / Test Harness
        |
        v
Authentication + Authorization Gateway (FastAPI)
        |
        +------> Protected API Service
        |               |
        |               v
        |         Local Data Store
        |
        +------> Security Audit Events

Docker Compose networks isolate services where applicable.
```

The gateway design is only a starting point. It must not be treated as a substitute for network-level enforcement, identity provisioning, or production-grade secret management.

## Planned features

1. **Network isolation:** Separate public-facing and internal service networks; test blocked paths.
2. **Service identity:** Verify the identity of callers using a simple locally managed authentication mechanism.
3. **Authorization:** Enforce least-privilege permissions for protected API endpoints.
4. **Secrets handling:** Load configuration from environment variables or local secret files excluded from Git; avoid hard-coded credentials.
5. **Audit logging:** Record authentication attempts, access decisions, and rejected requests without leaking secrets.
6. **Security tests:** Use Pytest to verify authorized access, forbidden actions, and selected attack scenarios.

## Roadmap

- [ ] **Milestone 1:** Create the repository, threat model, and minimal FastAPI service.
- [ ] **Milestone 2:** Define two Docker Compose networks and verify expected reachability.
- [ ] **Milestone 3:** Add authenticated requests and authorization checks.
- [ ] **Milestone 4:** Add structured audit events and safe secrets configuration.
- [ ] **Milestone 5:** Write automated security tests and document reproducible results.
- [ ] **Milestone 6:** Review threat assumptions, remaining risks, and possible extensions.

## Threat model (initial scope)

**Assets:** Internal APIs, data-store content, service credentials, and security logs.

**Threats to evaluate:** Unauthorized API access, privilege escalation through endpoint misuse, accidental service exposure, credentials committed to source control, and missing or misleading audit events.

**Out of scope for the initial prototype:** Production deployment, hardware/firmware security, real GPU-cluster security, enterprise identity providers, and compliance certification.

## Evaluation plan

Record reproducible tests rather than projected results:

| Test | Expected result | Observed result |
| --- | --- | --- |
| Authorized client calls permitted endpoint | Allowed | Not tested |
| Unauthenticated client calls protected endpoint | Denied | Not tested |
| Low-privilege client calls restricted endpoint | Denied | Not tested |
| Service attempts a blocked network connection | Denied where network policy applies | Not tested |
| Security event is generated for denied access | Event recorded | Not tested |

Update this table only after running and recording real tests.

## Running the lab

**Not yet available.** Setup commands and prerequisites will be added with the first working implementation.

## Project notes

This is a learning and portfolio lab, not a production-ready security product. Design choices, limitations, and unexpected test results will be documented as the project develops.
