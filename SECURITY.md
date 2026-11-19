# Security policy

Lleme sits between applications and LLM providers and handles provider
credentials, so security reports are taken seriously.

## Reporting a vulnerability

Do not open a public issue, discussion or pull request for a security
problem.

Report it privately, by one of these channels:

- The "Report a vulnerability" button on the Security tab of the
  repository (preferred).
- Email to `security@lleme.dev`.

Include:

- The version or commit you tested, and the deployment target (native
  server or Cloudflare Workers build).
- Step-by-step instructions to reproduce the problem.
- What you observed and what you expected.
- The impact you believe it has.

## Reports must be reproduced

A report needs steps and evidence that you verified yourself. Reports
that only restate the output of a scanner or of an AI tool, with no
reproduction, are closed. See `AI_POLICY.md`.

## What to expect

The project has one maintainer at the moment. These are targets, not
guarantees:

| Step                                   | Target            |
|----------------------------------------|-------------------|
| Acknowledgement of the report          | 5 business days   |
| First assessment                       | 10 business days  |
| Fix or mitigation for a confirmed issue| 90 days           |

We will keep you informed of progress, credit you in the advisory if
you want, and agree a disclosure date with you. Please do not disclose
the problem publicly before a fix is released or 90 days have passed
since your report, whichever comes first.

There is no bug bounty.

## Supported versions

While the project is in the `0.x` series, only the latest released
minor version receives security fixes.

## Scope

In scope:

- The gateway, in the native and Workers builds
- The admin API and the portal
- Handling, storage and logging of virtual keys and provider
  credentials
- The official container image and release artifacts
- The workflows in this repository

Out of scope:

- Vulnerabilities in LLM providers or in other third-party services
- Problems that require a deployment configured against the
  documentation, such as an admin API exposed without authentication
- Denial of service by volume against a deployment you do not own
- Content produced by a model, including prompt injection in the
  applications that call the gateway, unless it leads to a flaw in
  Lleme itself

## Testing

Test only against your own deployment. Do not access data that is not
yours, and do not run tests against deployments operated by other
people.

## Supply chain

This section applies from the first public release. Releases will be
built by CI from signed tags, container images will be signed, and
release artifacts will carry provenance attestations. If you find a
published artifact that does not match its tag, report it as a
vulnerability.
