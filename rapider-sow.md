<!-- Rapider Official App SOW & Specification | App #102: Continuous Integration / Deployment (CI/CD) (ci-cd) | Domain: DEV -->

# #102 Continuous Integration / Deployment (CI/CD) (`ci-cd`) — Scope of Work (SOW)

- **App Number & Slug:** `#102` / `102-continuous-integration-deployment-ci-cd`
- **Proposed Rapider App Key:** `ci-cd`
- **Suite / Domain Key:** `DEV` (Software Engineering & DevSecOps Suite — `Dev*` Canonical Prefix)
- **Representative Product (`P1`):** CircleCI
- **Combined Feature Catalog Reference:** [../combined-features/102-continuous-integration-deployment-ci-cd-features.md](../combined-features/102-continuous-integration-deployment-ci-cd-features.md)

---

## 1. Executive Outcome & Target Audience

`ci-cd` is the automated Continuous Integration, Smarter Test Intelligence, SLSA Artifact Provenance, and Progressive Continuous Delivery engine of the Rapider `DEV` suite. Engineering organizations struggle when slow, flaky test suites stall `#101` Merge Queues, long-lived cloud deployment keys create security exposure, or bad production releases require 45-minute manual rebuilds to recover. `ci-cd` combines high-concurrency DAG build pipelines (`DevPipelineDefinition`, `DevPipelineRun`, `DevPipelineStageJob`) with flaky test detection & parallel shard splitting, keyless OIDC federation, immutable SLSA-attested artifacts (`DevBuildArtifact`), and continuous-verification progressive rollouts (`DevDeploymentEnvironment`, `DevDeploymentRecord`).

- **SME Fast-Path Defaults:** Engineering teams pick a Reusable Pipeline Orb/Template (e.g., Node/TypeScript, Python, Go, Flutter, or Cloud Run API with direct `.run.app` URL & `ingress: all` best-practice defaults), connect a `#101` `DevRepository` in two clicks, and immediately get PR status checks, JUnit test insights, `STAGING` auto-deploy, and 1-click `PRODUCTION` promotion.
- **Enterprise Progressive Disclosure:** Platform SRE and Release Engineering teams unlock historical timing-balanced parallel test splitting, flaky test auto-quarantine via Home `WorkItem`s, SLSA build provenance attestations, Segregation-of-Duties `PRODUCTION` approval gates, deployment freeze windows (aligned with `FIN` close calendars), Canary/Blue-Green traffic shifting with `#103`/`#097` auto-rollback verification, and 4-key DORA metrics.
- **Measurable Success Outcomes:**
  - `40–70%` faster CI feedback cycles via lockfile layer caching, monorepo path filtering, and timing-balanced test splitting.
  - Zero static cloud credentials stored in pipeline configs via short-lived OIDC tokens and Platform Secret Vault injection.
  - Sub-60-second production recovery by rolling back to the prior immutable `DevBuildArtifact` digest (`sha256`).
- **Top 6 Global Benchmarks Synthesized:** `P1` CircleCI, `P2` GitHub Actions, `P3` GitLab CI/CD, `P4` Harness CD & GitOps, `P5` Microsoft Azure Pipelines, and `P6` Atlassian Bitbucket Pipelines.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### In-Scope
1. Declarative multi-stage DAG pipelines (`DevPipelineDefinition`), reusable pipeline templates/orbs, monorepo path filters, matrix executions (`DevPipelineRun`), and real-time secret-redacted job logs (`DevPipelineStageJob`).
2. Smarter Test Intelligence: JUnit/Playwright report ingestion, `FLAKY` test detection, single auto-rerun for flakes, and historical timing-balanced shard splitting.
3. Immutable build artifacts & SLSA provenance attestations (`DevBuildArtifact`), elastic/self-hosted runner pools (`DevRunnerPool`) with `Decimal128` minute budgets, and keyless OIDC + Secret Vault runtime injection.
4. Deployment environments (`DevDeploymentEnvironment`), exclusive concurrency locks, freeze blackout windows, Home `WorkItem` prod approvals, Canary/Blue-Green progressive rollouts, continuous post-deploy verification, and instant digest rollback (`DevDeploymentRecord`).
5. Four-key DORA engineering metrics and grounded AI job failure root-cause diagnostics (`projectChannelMessage`).

### Out-of-Scope
1. Git repository storage, pull request code review diffs, and branch `CODEOWNERS` rules — owned by `#101 Source Code Hosting & Version Control` (`source-control`).
2. Authoring OpenAPI specifications, interactive API request collections, and mock servers — owned by `#103 API Development & Testing` (`api-testing`), invoked by `ci-cd` during test/verification gates.
3. Sprint boards, engineering story estimation, and defect backlogs — owned by `APM` (`apm`, `planning`).
4. Customer-facing incident ticketing queues — owned by `SUP` (`support`).

### Cross-Suite Ownership & Integration Contract

| Domain / App | Owns (Single Source of Truth) | Integration Contract with `ci-cd` (`#102`) |
| :--- | :--- | :--- |
| **`ci-cd` (`#102`)** | `DevPipelineDefinition`, `DevPipelineRun`, `DevPipelineStageJob`, `DevBuildArtifact`, `DevRunnerPool`, `DevDeploymentEnvironment`, `DevDeploymentRecord` | Executes builds, tests, scans, and deployments; publishes commit status checks, release markers, and DORA metrics. |
| **`#101` Source Control (`source-control`)** | `DevRepository`, `DevPullRequest`, `DevReleaseTag` | Consumes push, PR, Merge Queue, and release-tag events to trigger `DevPipelineRun`s; reports exact commit SHA status back to `#101` `DevBranchRule` gates. |
| **`#103` API Testing (`api-testing`)** | `DevApiCollection`, `DevApiTestRun`, `DevApiMonitor` | Invokes `#103` contract suites during `STAGING` gates and listens to `#103` synthetic monitor failures during post-deploy Canary verification (`CICD-022`). |
| **`#097` Product Analytics & `#096` BI** | `DatTrackedEvent`, `DatDashboard` | Publishes `dev.cicd.deployment.succeeded.v1` markers to `#097` timelines and exports DORA 4-key metrics to `#096` executive scorecards. |
| **`APM` (`apm`)** | `ApmProject`, `ApmIssue` | Verifies linked `ApmIssue` change-readiness on `PRODUCTION` releases and posts deployment environment badges onto `ApmIssue` timelines. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (12 Pages)

All visible sidebar sections enforce **`>= 3` visible items (`displayOnMenu: true`)**, while detail views, log consoles, and drawers set `displayOnMenu: false`.

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `cicd-overview` | Delivery & DORA Hub | `Pipelines & Builds` | `true` (Order 1) | 4-Key DORA scorecard (Deploy Frequency, Lead Time, CFR, MTTR), active build queue, runner minute burn (`Decimal128`), `#activeFilterTagsBar`. |
| 2 | `pipelines` | Pipeline Definitions | `Pipelines & Builds` | `true` (Order 2) | `findSWR` directory of `DevPipelineDefinition`s with repo filter, pass-rate sparkline, clickable pipeline name, `Run Pipeline` trigger, icon+text `Edit`, icon-only `Delete`. |
| 3 | `pipeline-runs` | Build & Run Ledger | `Pipelines & Builds` | `true` (Order 3) | Cross-repo `DevPipelineRun` history (`RUNNING`, `PASSED`, `FAILED`, `CANCELED`, `AWAITING_APPROVAL`), `#activeFilterTagsBar` + `Clear all`, commit SHA & PR links. |
| 4 | `pipeline-templates` | Reusable Orbs & Templates | `Pipelines & Builds` | `true` (Order 4) | Versioned library of standardized CI/CD workflow templates (Microapp QA, Container SLSA, Cloud Run `.run.app` deploy, `#098` dbt runner). |
| 5 | `test-insights` | Smarter Test & Flake Insights | `Quality & Artifacts` | `true` (Order 5) | Slowest tests, highest failure rates, `FLAKY` test registry with auto-quarantine status and linked remediation Home `WorkItem`s. |
| 6 | `build-artifacts` | Artifacts & SLSA Provenance | `Quality & Artifacts` | `true` (Order 6) | `DevBuildArtifact` registry showing immutable `sha256` digest, SLSA attestation badge, CVE scan status, and promoted environments (`STAGING`, `PROD`). |
| 7 | `runner-pools` | Runner Pools & Compute FinOps | `Quality & Artifacts` | `true` (Order 7) | `DevRunnerPool` fleet monitor (`LINUX_X64`, `ARM64`, `MACOS`, `GPU`), live concurrency utilization, queue latency, and `Decimal128` minute budget caps. |
| 8 | `environments` | Deployment Environments | `Deployments & Releases` | `true` (Order 8) | Live environment board (`DEV`, `QA`, `PREVIEW`, `STAGING`, `PRODUCTION`) displaying active artifact version, health status, lock state, and freeze banner. |
| 9 | `deployments` | Releases & Rollbacks | `Deployments & Releases` | `true` (Order 9) | `DevDeploymentRecord` kanban + table (`<rapider-kanban>` + table) across `PENDING_APPROVAL -> DEPLOYING -> VERIFYING -> SUCCEEDED / ROLLED_BACK` with `1-Click Rollback`. |
| 10 | `freeze-policies` | Freeze Windows & Release Gates | `Deployments & Releases` | `true` (Order 10) | Blackout calendar (`DevDeploymentEnvironment` freeze schedules), Segregation-of-Duties approver rules, and Continuous Verification rollback thresholds. |
| 11 | `pipeline-run-detail` | DAG Graph, Logs & AI RCA | *(Hidden)* | `false` | Interactive fan-out/fan-in stage DAG (`#/pipeline-run-detail?id=...`), streaming `DevPipelineStageJob` ANSI log console, JUnit failures tab, AI Root-Cause drawer, and `Back to Runs`. |
| 12 | `deployment-detail` | Progressive Rollout & War-Room | *(Hidden)* | `false` | Canary traffic weight progression (`10% -> 50% -> 100%`), live `#103`/`#097` health verification chart, `projectChannelMessage` release war room, plus sliding `pipeline-env-drawer` (`displayOnMenu: false`). |

---

## 4. Canonical Data Models (`Dev*` — 7 Models)

All models strictly omit `projectId`, store zero plaintext secrets, use platform UUID `id` + optimistic `version`, and use `Decimal128` for compute credits, budgets, and verification thresholds.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`DevPipelineDefinition`** | `id: UUID`, `code: string`, `name: string`, `repositoryId: UUID`, `yamlSpec: string`, `templateOrbCode: string?`, `triggers: enum(PUSH, PULL_REQUEST, MERGE_QUEUE, RELEASE_TAG, CRON, MANUAL)[]`, `monorepoPathGlobs: string[]`, `defaultRunnerPoolId: UUID`, `oidcCloudRoleArnOrUri: string?`, `vaultSecretRefs: string[]`, `flakyAutoRerunMax: int`, `isEnabled: boolean`, `version: int` | `repositoryId -> DevRepository.id` (`#101`); `defaultRunnerPoolId -> DevRunnerPool.id`. | **Zero-secret invariant:** `yamlSpec` is validated on save to reject inline secrets/tokens (`vaultSecretRefs` and keyless `oidcCloudRoleArnOrUri` only). Cloud Run deploy templates enforce direct `.run.app` URLs and `ingress: all` when configured as public APIs. |
| **`DevPipelineRun`** | `id: UUID`, `pipelineId: UUID`, `runNumber: int`, `repositoryId: UUID`, `commitSha: string`, `branchOrTag: string`, `pullRequestId: UUID?`, `triggerType: string`, `triggeredByUserId: UUID?`, `status: enum(QUEUED, RUNNING, AWAITING_APPROVAL, PASSED, FAILED, CANCELED, TIMED_OUT)`, `startedAt: datetime?`, `completedAt: datetime?`, `durationSeconds: int`, `computeCreditsUsed: Decimal128`, `flakyTestsDetectedCount: int`, `discussionChannelId: UUID?`, `idempotencyKey: string` | `pipelineId -> DevPipelineDefinition.id`; `pullRequestId -> DevPullRequest.id` (`#101`); `discussionChannelId -> projectChannelMessage`. | `(pipelineId, runNumber)` monotonically increasing. Terminal states (`PASSED`, `FAILED`, `CANCELED`, `TIMED_OUT`) are immutable; re-running creates a linked new attempt record. |
| **`DevPipelineStageJob`** | `id: UUID`, `pipelineRunId: UUID`, `stageName: string`, `jobName: string`, `shardIndex: int`, `shardTotal: int`, `dependsOnJobNames: string[]`, `runnerPoolId: UUID`, `status: enum(PENDING, RUNNING, PASSED, FAILED, SKIPPED, CANCELED)`, `exitCode: int?`, `cacheHit: boolean`, `testSummaryJson: {passed, failed, skipped, flakyNames}[]`, `redactedLogFileId: UUID?`, `durationMs: int` | `pipelineRunId -> DevPipelineRun.id`; `runnerPoolId -> DevRunnerPool.id`; `redactedLogFileId -> Platform Drive File.id`. | Console logs written to `redactedLogFileId` pass through synchronous multi-pattern + vault-value secret redaction (`***REDACTED***`). |
| **`DevBuildArtifact`** | `id: UUID`, `pipelineRunId: UUID`, `repositoryId: UUID`, `commitSha: string`, `artifactName: string`, `artifactType: enum(CONTAINER_IMAGE, WEB_BUNDLE, BINARY_PACKAGE, SBOM_ATTESTATION, TEST_REPORT)`, `immutableDigestSha256: string`, `storageUriOrFileId: string`, `sizeBytes: int`, `slsaProvenanceJson: object`, `criticalCveCount: int`, `retentionExpiresAt: datetime?`, `version: int` | `pipelineRunId -> DevPipelineRun.id`; `repositoryId -> DevRepository.id` (`#101`). | `immutableDigestSha256` and `slsaProvenanceJson` are write-once immutable; `PRODUCTION` deployments reject artifacts where `criticalCveCount > 0` unless break-glass approved. |
| **`DevRunnerPool`** | `id: UUID`, `code: string`, `name: string`, `poolMode: enum(MANAGED_CLOUD, SELF_HOSTED)`, `archType: enum(LINUX_X64, LINUX_ARM64, MACOS_ARM64, GPU_LINUX)`, `maxConcurrency: int`, `activeJobsCount: int`, `creditRatePerMin: Decimal128`, `monthlyCreditBudget: Decimal128`, `creditsUsedMonth: Decimal128`, `status: enum(ONLINE, DEGRADED, DRAINING, PAUSED_BUDGET)`, `version: int` | Referenced by `DevPipelineDefinition` & `DevPipelineStageJob`. | All credit rates and monthly caps use `Decimal128`. Crossing `90%` of `monthlyCreditBudget` opens a FinOps Home `WorkItem`. |
| **`DevDeploymentEnvironment`** | `id: UUID`, `code: string`, `name: string`, `tier: enum(DEV, QA, PREVIEW, STAGING, PRODUCTION)`, `rolloutStrategy: enum(ROLLING, BLUE_GREEN, CANARY)`, `allowedBranchGlobs: string[]`, `requireApproval: boolean`, `approverUserIds: UUID[]`, `blockCommitAuthorApproval: boolean`, `freezeWindowsJson: {name, startAt, endAt}[]`, `errorRateRollbackThresholdPct: Decimal128`, `activeArtifactId: UUID?`, `isLockedByDeploymentId: UUID?`, `version: int` | `activeArtifactId -> DevBuildArtifact.id`; `isLockedByDeploymentId -> DevDeploymentRecord.id`. | At most one active `DEPLOYING` / `VERIFYING` `DevDeploymentRecord` may hold `isLockedByDeploymentId` per environment at a time. |
| **`DevDeploymentRecord`** | `id: UUID`, `environmentId: UUID`, `pipelineRunId: UUID`, `artifactId: UUID`, `previousArtifactId: UUID?`, `commitSha: string`, `linkedApmIssueIds: UUID[]`, `status: enum(PENDING_APPROVAL, QUEUED_LOCK, DEPLOYING, VERIFYING, SUCCEEDED, ROLLED_BACK, FAILED, REJECTED)`, `canaryWeightPct: int`, `approvedByUserId: UUID?`, `rollbackOfDeploymentId: UUID?`, `verificationMetricsJson: object`, `activeWorkItemId: UUID?`, `discussionChannelId: UUID?`, `startedAt: datetime`, `completedAt: datetime?`, `version: int` | `environmentId -> DevDeploymentEnvironment.id`; `artifactId, previousArtifactId -> DevBuildArtifact.id`; `linkedApmIssueIds -> ApmIssue.id[]`; `activeWorkItemId -> Platform WorkItem.id`; `discussionChannelId -> projectChannelMessage`. | Cannot enter `DEPLOYING` on a `PRODUCTION` environment during an active `freezeWindowsJson` window (without Incident Commander break-glass) or without non-author `approvedByUserId` when `blockCommitAuthorApproval: true`. |

---

## 5. Core End-to-End User Journeys (5 Journeys)

1. **Journey 1 — PR Commit Trigger, Timing-Balanced Test Split & Flaky Test Detection:**
   - Merging or pushing to a PR in `#101` triggers `DevPipelineRun` `#814`. `ci-cd` splits 600 Playwright tests across 4 `LINUX_ARM64` shards balanced by historical duration (`CICD-009`).
   - One intermittent test fails on Shard 2 and passes on immediate `flakyAutoRerunMax: 1` retry (`CICD-008`). `DevPipelineRun` reports `PASSED` back to `#101` Merge Queue while badging that test `FLAKY` on `test-insights` and opening a quality Home `WorkItem`.
2. **Journey 2 — Build Once, SLSA Provenance & Segregation-of-Duties `PRODUCTION` Approval:**
   - Upon merge to `main`, `ci-cd` builds container `sha256:8f41...c209`, generates a signed SLSA provenance attestation (`DevBuildArtifact`), auto-deploys to `STAGING`, and passes `#103` API regression tests (`CICD-010`).
   - Promoting the exact same `sha256:8f41...c209` digest to `PRODUCTION` (`CICD-015`) pauses in `PENDING_APPROVAL` and dispatches a Rapider Home `WorkItem` containing the AI Pre-Flight Risk Summary (`CICD-027`).
   - Because `blockCommitAuthorApproval: true`, the commit author cannot approve; the designated SRE Lead approves the `WorkItem`, acquiring the environment concurrency lock.
3. **Journey 3 — Canary Progressive Rollout, Post-Deploy Verification Breach & Instant Auto-Rollback:**
   - `PRODUCTION` (`rolloutStrategy: 'CANARY'`) shifts `10%` traffic to `sha256:8f41...c209` (`VERIFYING`).
   - During the 5-minute bake window, `#103` synthetic monitor error rate hits `3.80%` (exceeding `errorRateRollbackThresholdPct: 1.00%`).
   - `ci-cd` automatically aborts the canary, restores `100%` traffic to `previousArtifactId` in `18 seconds`, marks status `ROLLED_BACK`, opens a high-priority SRE Home `WorkItem`, and posts metrics to the `projectChannelMessage` war room (`CICD-022`).
4. **Journey 4 — Click-Through DAG Log Inspection, AI Root-Cause & Back-State Restoration:**
   - On `pipeline-runs`, a developer filters by `Status: "FAILED"` (`#activeFilterTagsBar` displays `Status: "FAILED" ✕`), clicks Run `#819` to open `#/pipeline-run-detail?id=...`, inspects the failed `typecheck-and-build` node, and invokes the AI Root-Cause Analyzer (`CICD-025`), which cites missing type export in `src/billing/invoice.ts:44`. Clicking `"Back to Runs"` restores `Status: "FAILED"`.
5. **Journey 5 — Fiscal Close Deployment Freeze Enforcement:**
   - During `FIN` quarter-end blackout configured on `freeze-policies` (`CICD-023`), a standard feature deployment to `PRODUCTION` is deterministically held/rejected with clear freeze window details, protecting ledger close stability.

---

## 6. Consolidated Feature Priorities & Work Packages (`P0`, `P1`, `P2`)

### `P0` Foundation Work Packages
- **`WP-P0-1` DAG Pipelines, Matrix Builds & Secret-Redacted Logs (`CICD-001`, `CICD-002`, `CICD-003`):** Build `cicd-overview`, `pipelines`, `pipeline-runs`, and `pipeline-run-detail` with `#101` triggers, monorepo path filters, and streaming redacted job logs (`Developer` / `DevOps`).
- **`WP-P0-2` Test Ingestion & Flaky Test Detection (`CICD-007`, `CICD-008`):** Build `test-insights` with JUnit/Playwright parsing, `FLAKY` auto-rerun, and quarantine `WorkItem`s (`QA / Developer`).
- **`WP-P0-3` Immutable SLSA Artifacts, Keyless OIDC & Runner Pools (`CICD-012`, `CICD-013`, `CICD-014`):** Build `build-artifacts` and `runner-pools` with `sha256` digest provenance, OIDC/Vault secret injection, and `Decimal128` runner minute budgets (`Platform Engineer`).
- **`WP-P0-4` Environments, Home `WorkItem` Prod Approvals & 1-Click Rollback (`CICD-018`, `CICD-019`, `CICD-020`):** Build `environments`, `deployments` (`<rapider-kanban>` + table), and `deployment-detail` with concurrency locks, SoD approvals, and instant digest rollback (`Release Approver` / `SRE`).

### `P1` Scale & Enterprise Governance Work Packages
- **`WP-P1-1` Reusable Orbs, Layer Caching, YAML Linter & Timing Split (`CICD-004`, `CICD-005`, `CICD-006`, `CICD-009`, `CICD-010`):** Build `pipeline-templates`, cache telemetry, historical parallel shard splitting, and native `#103`/`#098` gate steps (`DevOps Engineer`).
- **`WP-P1-2` Digest Promotion, CVE Gates, PR Previews, Canary Auto-Rollback, Freezes & DORA (`CICD-015`, `CICD-016`, `CICD-017`, `CICD-021`, `CICD-022`, `CICD-023`, `CICD-024`):** Build `freeze-policies`, Canary/Blue-Green continuous verification rollback, ephemeral PR previews, and 4-key DORA `#096` exports (`SRE` / `Engineering Director`).

### `P2` Adjacent & Grounded AI Work Packages
- **`WP-P2-1` Test Impact Selection & AI Failure / Cost Advisors (`CICD-011`, `CICD-025`, `CICD-026`):** Diff-based fast test selection, grounded failed-log RCA over `projectChannelMessage`, and runner cost/caching optimization hints (`Developer` / `FinOps`).
- **`WP-P2-2` Approver Risk Summaries & Release War-Room Threads (`CICD-027`, `CICD-028`):** Automated commit/migration risk digests on prod approval `WorkItem`s and per-release `projectChannelMessage` coordination threads (`Release Approver`).

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **`Pipeline Viewer`** | View `cicd-overview` DORA metrics, `pipeline-runs`, `test-insights`, `environments`, and redacted logs. | Cannot trigger deployments, cancel peer builds, or view OIDC/Vault binding identifiers. |
| **`Software Engineer`** | Author `DevPipelineDefinition`s via PR, trigger feature/staging runs, inspect `pipeline-run-detail` logs & AI RCA, deploy to `DEV/QA/PREVIEW`. | Cannot deploy unapproved artifacts to `PRODUCTION`, cannot self-approve own commit to `PRODUCTION` when `blockCommitAuthorApproval: true`, and never sees raw secret values. |
| **`Release Approver / Engineering Lead`** | Approve/reject `PRODUCTION` `DevDeploymentRecord` Home `WorkItem`s (for commits authored by others), trigger 1-click rollbacks. | Cannot bypass active `freezeWindowsJson` or `criticalCveCount > 0` blocks without `Incident Commander`. |
| **`Platform / DevOps Engineer`** | Author `pipeline-templates` orbs, manage `DevRunnerPool` concurrency & `Decimal128` budgets, configure OIDC roles & `vaultSecretRefs`. | Cannot read secret values back out of Platform Vault; all runner pool & OIDC edits are audit-logged. |
| **`SRE / Incident Commander`** | Execute emergency break-glass deployments during freeze windows (with mandatory incident justification), abort/rollback live canaries, lock environments. | Every break-glass action writes an immutable SOX/SOC 2 compliance alert. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### Guarded Commands
1. `dev.cicd.triggerPipelineRun({ pipelineId, commitSha, branchOrTag, parameters, idempotencyKey, correlationId })` — Evaluates monorepo path globs, checks `DevRunnerPool` budget state, and schedules DAG execution.
2. `dev.cicd.promoteArtifactToEnvironment({ artifactId, environmentId, idempotencyKey, correlationId })` — Validates branch restrictions, `criticalCveCount === 0`, and freeze windows; either opens an Approval Home `WorkItem` (`PENDING_APPROVAL`) or acquires environment lock (`DEPLOYING`).
3. `dev.cicd.decideDeploymentApproval({ deploymentId, decision: 'APPROVE' | 'REJECT', notes, expectedVersion, idempotencyKey, correlationId })` — Enforces `approverId !== commitAuthorId` when `blockCommitAuthorApproval: true` and resolves the Home `WorkItem`.
4. `dev.cicd.rollbackDeployment({ environmentId, targetArtifactId, reason, idempotencyKey, correlationId })` — Preempts failed/degraded rollout and re-deploys `previousArtifactId` immutable digest immediately.

### Domain Events Published & Consumed (Kafka)
- **Published:** `dev.cicd.pipeline_run.completed.v1` (consumed by `#101` PR/Merge Queue), `dev.cicd.artifact.attested.v1`, `dev.cicd.deployment.succeeded.v1` (consumed by `#097` & `APM`), `dev.cicd.deployment.rolled_back.v1`, `dev.cicd.dora_metrics.updated.v1`.
- **Consumed:** `dev.scm.push.accepted.v1`, `dev.scm.pull_request.opened.v1`, `dev.scm.merge_queue.batch_ready.v1`, `dev.scm.release.published.v1` (all from `#101`), `dev.api.monitor.breached.v1` (from `#103` for Canary auto-rollback).

### `requiredWorkflows`
- `cicdDagExecutionOrchestratorWorkflow` — Fan-out/fan-in stage scheduler, shard splitter, flaky single-rerun handler, and SLSA provenance generator.
- `cicdProductionApprovalWorkflow` — Routes `PRODUCTION` promotions through Segregation-of-Duties Home `WorkItem`s.
- `cicdProgressiveCanaryVerificationWorkflow` — Steps canary weights (`10% -> 50% -> 100%`), polls `#103`/`#097` health signals, and triggers auto-rollback on threshold breach.

### Grounded Rapider AI Governance Rules
- AI Failure RCA (`CICD-025`) operates exclusively on secret-redacted job logs (`redactedLogFileId`), cites exact log line numbers and commit SHAs, and **NEVER** triggers `PRODUCTION` promotions, rollbacks, or freeze overrides without explicit human execution.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **Supply-Chain & SOX/SOC 2 Compliance (SLSA / NIST SSDF):** Enforces keyless OIDC authentication, immutable `sha256` artifact promotion (`build once, deploy anywhere`), Segregation-of-Duties production approvals, and tamper-evident SLSA provenance records retained for 7 years.
- **Strict Secret Redaction & Cloud Run Best Practices:** Job logs strip any dynamic vault value (including base64/URL-encoded variants) prior to disk write; built-in Cloud Run deployment templates configure direct `.run.app` URLs and `ingress: all` for public APIs to avoid custom domain SSL and internal ingress misconfigurations.
- **Precision & Host Tenancy:** `Decimal128` used for `computeCreditsUsed`, `monthlyCreditBudget`, and `errorRateRollbackThresholdPct`; zero `projectId` fields in any model.
- **Accessibility & Responsive UX:** `<rapider-kanban>` on `deployments` stays 100% synchronized with its table view; ANSI terminal log viewer supports high-contrast light/dark themes and keyboard line navigation; `#ffffff !important` white text enforced on all primary/danger buttons across `390px`, `768px`, and `1440px`.

---

## 10. Deterministic Acceptance Criteria (14 Testable Criteria)

1. **AC-01 (Sidebar Minimum-3 Rule):** Visible sidebar sections (`Pipelines & Builds`: 4 items, `Quality & Artifacts`: 3 items, `Deployments & Releases`: 3 items) each contain `>= 3` `displayOnMenu: true` pages; `pipeline-run-detail`, `deployment-detail`, and `pipeline-env-drawer` set `displayOnMenu: false`.
2. **AC-02 (Rapider Kanban & Multi-View Sync):** `deployments` renders `<rapider-kanban>` synchronized with its table view; approving or rolling back a `DevDeploymentRecord` updates both views optimistically without reload.
3. **AC-03 (SWR Clean Queries & Defensive Soft-Delete):** All 10 sidebar routes load via `findSWR`/`countSWR` without client `isDeleted` where-clauses and apply in-memory `item.isDeleted !== true` filtering.
4. **AC-04 (Table Actions & Click-Through Navigation):** Zero `"View"` buttons exist in `Actions` columns; clicking a run on `pipeline-runs` navigates to `#/pipeline-run-detail?id=...` and clicking a deployment on `deployments` navigates to `#/deployment-detail?id=...` without `<base href>` 404 diversion; `Edit` uses icon+text and `Delete` uses an icon-only button with `#rapiderConfirmOkBtn` modal confirmation.
5. **AC-05 (Active Filter Tags & Clear All):** Filtering `pipeline-runs`, `test-insights`, or `deployments` displays `#activeFilterTagsBar` with HTML-escaped dismissible chips and `[Clear all]`.
6. **AC-06 (Drawer Lifecycle & `finally` Reset):** Saving or cancelling in `pipeline-env-drawer` restores button state inside `finally`, clears SWR caches, updates parent views optimistically, and closes the drawer via `closePageDrawer()`.
7. **AC-07 (Zero-Secret YAML & Log Masking):** Saving a `DevPipelineDefinition` containing hardcoded credentials fails validation (`422`), and any secret injected via `vaultSecretRefs` echoed during a job is scrubbed to `***REDACTED***` in `redactedLogFileId`.
8. **AC-08 (Exclusive Environment Concurrency Lock):** Promoting two deployments simultaneously to the same `DevDeploymentEnvironment` grants `isLockedByDeploymentId` to the first (`DEPLOYING`) and deterministically queues the second (`QUEUED_LOCK`) without parallel overwrite corruption.
9. **AC-09 (Segregation-of-Duties Prod Approval Gate):** When `blockCommitAuthorApproval === true` on `PRODUCTION`, an approval attempt by the commit author fails with `403 Self-Approval Prohibited`, while an authorized Release Approver transitions status to `DEPLOYING` and closes the Home `WorkItem`.
10. **AC-10 (Deployment Freeze Window Enforcement):** Standard `promoteArtifactToEnvironment` calls targeting `PRODUCTION` during an active `freezeWindowsJson` window are deterministically blocked (`409 Active Deployment Freeze`) unless accompanied by an audited Incident Commander break-glass token.
11. **AC-11 (Canary Continuous Verification Auto-Rollback):** When post-deploy verification error rate exceeds `Decimal128` `errorRateRollbackThresholdPct`, `cicdProgressiveCanaryVerificationWorkflow` automatically transitions the deployment to `ROLLED_BACK`, restores `previousArtifactId`, and opens an SRE Home `WorkItem`.
12. **AC-12 (Flaky Test Auto-Tagging):** A test case that fails and then passes on the exact same `commitSha` within `flakyAutoRerunMax` is automatically flagged `FLAKY` on `test-insights`.
13. **AC-13 (Back Navigation Filter Preservation):** Navigating from `pipeline-runs` (filtered by `Status: "FAILED"`) into `pipeline-run-detail` and clicking `#btnGoBack` restores `pipeline-runs` with `Status: "FAILED"` intact.
14. **AC-14 (Theme & Contrast Compliance):** All routes pass light/dark `validate_theme` checks with `#ffffff !important` white text on primary/danger buttons and zero `window.alert`/`confirm`/`prompt` dialogs.
