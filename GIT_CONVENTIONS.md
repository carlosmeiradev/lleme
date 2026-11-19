# Git conventions

Rules for branches, commits, versions and tags in this repository.
They apply to maintainers and external contributors alike.

## Branches

`main` is the only long-lived branch. It is always releasable and only
changes through squash-merged pull requests.

### Naming

```
<type>/<issue>-<short-description>
```

- lowercase, words separated by hyphens, no more than 50 characters
- `<issue>` is the GitHub issue number; omit it only for trivial changes
- `<type>` is one of the prefixes below

| Prefix      | Use for                                   | Example                        |
| ----------- | ----------------------------------------- | ------------------------------ |
| `feat/`     | New behavior                              | `feat/12-context-filter`       |
| `fix/`      | Bug fix                                   | `fix/31-stream-timeout`        |
| `refactor/` | Internal change, same behavior            | `refactor/40-split-adapters`   |
| `perf/`     | Performance work                          | `perf/52-pool-connections`     |
| `test/`     | Tests only                                | `test/18-router-fallback`      |
| `docs/`     | Documentation only                        | `docs/27-edge-deploy-guide`    |
| `ci/`       | Workflows and build tooling               | `ci/9-wasm-target-check`       |
| `chore/`    | Dependencies, housekeeping                | `chore/bump-hyper`             |
| `release/`  | Maintenance line for a published minor    | `release/0.3`                  |
| `hotfix/`   | Urgent fix that targets a `release/` line | `hotfix/77-key-leak-in-logs`   |

### Lifecycle

- Branch from `main`, open a draft pull request early, keep it rebased.
- One branch, one purpose. If a second concern appears, open a second branch.
- Branches are deleted automatically after merge.
- `release/X.Y` is created only when an older minor needs a patch. Fixes
  land on `main` first and are cherry-picked back, never the other way.

## Commits

Commits follow [Conventional Commits 1.0](https://www.conventionalcommits.org/).
Because pull requests are squash-merged, the pull request title becomes
the commit on `main` and must follow the same format.

### Format

```
<type>(<scope>)<!>: <summary>

<body>

<footers>
```

- **summary**: imperative mood, lowercase first letter, no trailing period,
  72 characters or fewer
- **body**: optional; explains why the change is needed, wrapped at 72
- **footers**: optional; issue references and breaking-change notes

### Types

| Type       | Meaning                                         | Appears in changelog |
| ---------- | ----------------------------------------------- | -------------------- |
| `feat`     | New user-visible behavior                       | Yes                  |
| `fix`      | Bug fix                                         | Yes                  |
| `perf`     | Faster or lighter, same behavior                | Yes                  |
| `refactor` | Code change with no behavior change             | No                   |
| `test`     | Tests only                                      | No                   |
| `docs`     | Documentation only                              | No                   |
| `ci`       | Workflows, build scripts                        | No                   |
| `chore`    | Dependencies, tooling, repository housekeeping  | No                   |
| `revert`   | Reverts an earlier commit                       | Yes                  |

### Scopes

The scope names the part of the repository that changes.

| Scope    | Covers                                  |
| -------- | --------------------------------------- |
| `core`   | `crates/core`                           |
| `server` | `crates/server`                         |
| `edge`   | `crates/edge` (Cloudflare Workers)      |
| `portal` | `portal/`                               |
| `deps`   | Dependency updates                      |

Omit the scope when a change spans several areas or touches only
repository-level files.

### Breaking changes

Mark a breaking change in both places:

- `!` after the type or scope: `feat(core)!: rename policy schema fields`
- a `BREAKING CHANGE:` footer describing what breaks and how to migrate

### Footers

| Footer             | Use                                             |
| ------------------ | ----------------------------------------------- |
| `Refs #12`         | Related issue, stays open                       |
| `Closes #12`       | Issue closed by this change                     |
| `BREAKING CHANGE:` | Required for any incompatible change            |

Do not add tool-generated trailers. AI usage is declared in the pull
request, as described in `AI_POLICY.md`, not in commit messages.

### Rules

- One intention per commit. A refactor and a behavior change are two commits.
- Every commit builds and passes tests on its own.
- Commits are signed. Unsigned commits cannot be merged.
- The message is written by the author and describes the reason for the
  change, not a restatement of the diff.

### Examples

```
feat(core): add context window filter to candidate selection

Requests larger than a model's context window were routed and then
rejected by the provider. Filter those models out before the policy
runs, using the token estimate already computed for cost.

Refs #12
```

```
fix(server): stop buffering streamed responses

Closes #31
```

```
feat(core)!: rename policy schema fields

BREAKING CHANGE: `strong_model` and `weak_model` are replaced by a
`tiers` list. Existing policy files must be migrated; see
docs/migrations/0.4.md.
```

## Versioning

Versions follow [Semantic Versioning 2.0](https://semver.org/):
`MAJOR.MINOR.PATCH`. All crates in the workspace, the portal and the
container image share one version number.

### Before 1.0

While the version is `0.x.y`, the public API is not yet stable.

| Change                                  | Bump    | Example           |
| --------------------------------------- | ------- | ----------------- |
| Breaking change (`!`)                   | MINOR   | `0.3.2` → `0.4.0` |
| `feat`, `fix`, `perf`                   | PATCH   | `0.3.2` → `0.3.3` |

### From 1.0 on

| Change                                  | Bump    | Example           |
| --------------------------------------- | ------- | ----------------- |
| Breaking change (`!`)                   | MAJOR   | `1.4.2` → `2.0.0` |
| `feat`                                  | MINOR   | `1.4.2` → `1.5.0` |
| `fix`, `perf`                           | PATCH   | `1.4.2` → `1.4.3` |

Commits of type `refactor`, `test`, `docs`, `ci` and `chore` do not
trigger a release on their own.

### Pre-release suffixes

A pre-release is the target version followed by a hyphen, a stage and
a counter separated by a dot.

| Suffix     | Stage             | Meaning                                        |
| ---------- | ----------------- | ---------------------------------------------- |
| `-alpha.N` | Alpha             | Incomplete; interfaces may change at any time  |
| `-beta.N`  | Beta              | Feature complete; fixing bugs                  |
| `-rc.N`    | Release candidate | Becomes the final release if nothing is found  |

- `N` starts at 1 and increases within a stage: `0.4.0-rc.1`, `0.4.0-rc.2`.
- Always use the dot before the counter. `0.4.0-rc.10` sorts after
  `0.4.0-rc.2`; `0.4.0-rc10` does not.
- Order of precedence: `0.4.0-alpha.1` < `0.4.0-beta.1` < `0.4.0-rc.1` < `0.4.0`.
- Build metadata (`+something`) is not used. It carries no precedence
  and is not valid in container image tags.

## Tags and releases

- Git tags are the version prefixed with `v`: `v0.4.0`, `v0.4.0-rc.1`.
- Tags are annotated, signed, and created only by the release workflow.
- A published tag is never moved or deleted. A bad release is fixed by
  a new version.

### Container image tags

| Tag           | Points to                                  | Moves |
| ------------- | ------------------------------------------ | ----- |
| `0.4.1`       | Exactly that release                       | No    |
| `0.4`         | Latest patch of the `0.4` line             | Yes   |
| `latest`      | Latest stable release                      | Yes   |
| `0.5.0-rc.1`  | Exactly that pre-release                   | No    |
| `main`        | Latest commit on `main`, unreleased        | Yes   |

Pre-releases never move `latest` or the `MAJOR.MINOR` tag. Production
deployments should pin an exact version or an image digest.

### Release artifacts

| Artifact               | Name                                          |
| ---------------------- | --------------------------------------------- |
| Native binary          | `lleme-<version>-<target-triple>.tar.gz`  |
| Cloudflare Workers build | `lleme-edge-<version>.tar.gz`           |
| Checksums              | `lleme-<version>-checksums.txt`           |

## Quick reference

```
branch   feat/12-context-filter
commit   feat(core): add context window filter to candidate selection
pr title feat(core): add context window filter to candidate selection
tag      v0.4.0        v0.4.0-rc.1
image    0.4.0  0.4  latest  main
```
