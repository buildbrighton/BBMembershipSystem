# BBMS modernisation and manual releases

## Aim and scope

Bring BBMS onto maintained runtimes, libraries and tooling in small, reviewable steps while preserving its existing behaviour. Keep membership rules, payment calculations, access decisions, integrations and user journeys stable. Any unavoidable behaviour change must be identified explicitly and reviewed separately.

GitHub and the live server are separate. A merged PR is not evidence of a deployed change. This document starts the upgrade record; it does not certify the existing application as secure.

## Repository baseline

Inspected on 1 November 2026, on `master` at `ed1fb6e95ae37ef9e690ff46fce29b07da4faa94`.

| Area | Repository evidence |
| --- | --- |
| Backend | `composer.json` requests Laravel ~5.1.0; `composer.lock` pins v5.1.46 |
| Payments | Lockfile pins GoCardless 1.2.0, Stripe v1.18.0 and PayPal merchant SDK v3.8.107 |
| Frontend | `package.json` requests React ^0.13.3, jQuery ^2.1.4, Bootstrap ^3.3.4, Gulp ~3.9.1 and Laravel Elixir ~2.1.1 |
| Build records | Composer and Yarn lockfiles are present |
| Historical CI | `.travis.yml` selects PHP 7.1; this is not proof of the live PHP version or a successful current test run |
| Tests | PHP tests, Codeception suites and frontend tests are present; they have not been run as part of this initial documentation change |

No live server comparison, dependency vulnerability audit or runtime compatibility assessment has yet been completed. Version declarations alone do not establish an exploitable vulnerability or compatibility with a replacement version.

## Establish the live baseline before code deployment

Record the following in a private operations record, keeping credentials, member information and server backups out of this public repository:

- Server OS, web server, PHP version and extensions, database engine/version, Composer version, and any Node/Yarn versions used to build assets.
- Application directory, web root, upload method, file ownership and permissions.
- Scheduler/cron entries, queue workers and process supervision.
- Enabled integrations, webhook endpoints, email transport and file storage.
- Current application files, installed dependency versions, built assets and database migration state.
- A comparison of the live source against the repository baseline, excluding secrets and generated data. Review and preserve server-only fixes before replacing files.
- Verified backups and a rehearsed restore process covering application code, dependencies, database and uploaded files.

Preserve the existing encryption/application key. Do not generate a replacement key during an upgrade. The README warns that it cannot be changed or lost once set.

## PR sequence

Each stage may require multiple PRs. Use a separate branch and one coherent purpose per PR.

1. **Baseline and documentation:** capture the repository and server state, release records and rollback procedure.
2. **Reproducible development and checks:** establish an isolated environment matching the baseline, run existing tests, document failures and add meaningful coverage where critical behaviour is unprotected. Keep test credentials separate and prevent real payments, messages and access-control actions.
3. **Security and compatibility fixes:** assess exact locked dependencies and application code. Make narrowly scoped fixes with regression evidence. Describe any intentional change to unsafe behaviour explicitly.
4. **Backend modernisation:** select maintained PHP/Laravel targets after checking official support policies and hosting compatibility. Upgrade through compatible stages, including SDKs and test tools. Verify existing database, session, authentication, encryption and date handling.
5. **Frontend and build modernisation:** update the build pipeline and browser dependencies in controlled steps. Compare generated assets and existing user journeys.
6. **Release rehearsal:** test the complete package on a representative staging environment before a separately scheduled manual production release.

Do not perform an unrestricted dependency update on production. Resolve and review dependency changes outside production, commit the appropriate lockfile changes and deploy the tested versions.

## Evidence required for code PRs

Record the reason, exact version changes, changed files, expected behaviour, test results and known limitations. Include manual deployment and rollback notes even when no server action is required.

Choose checks relevant to the change. Critical regression areas include signup/login, permissions, member status transitions, subscriptions and balances, payment/webhook handling including repeated deliveries, RFID door/equipment access, scheduled billing, uploads and email. Existing behaviour is the reference; any discrepancy needs investigation.

External integrations must use isolated test credentials or controlled substitutes. Do not connect a development or staging environment to live payment processing, mail delivery or physical access devices.

## Manual release procedure

These are release requirements, not executable server commands. Add environment-specific paths and commands only after the server baseline is known.

1. Select the reviewed commit(s), record their PR links and define the release scope. Confirm all prerequisites and test evidence.
2. Produce a release package from the reviewed commit using the reviewed lockfiles and build process. Record its commit and checksum, included dependencies/assets, file manifest, removals and exclusions.
3. Compare the target server against the recorded baseline. Resolve unexpected differences before deploying.
4. Verify backups and the rollback plan. Coordinate any maintenance window, background jobs and webhook delivery handling so payment events are not lost or processed twice.
5. Upload to a separate release location where hosting permits, or follow a rehearsed file replacement procedure. Preserve environment settings, the existing key, uploads, logs and other persistent data; never overwrite these from a development copy.
6. Run only the migrations, cache operations and worker restarts explicitly reviewed for this release. Database changes require a specific recovery plan; restoring code alone may be insufficient.
7. Verify the deployed file manifest/commit record, relevant user journeys, logs, integration status and background jobs. Check payment behaviour through controlled checks; do not trigger unplanned real transactions.
8. Record the outcome and actual deployed version. If checks fail, follow the reviewed rollback plan and reconcile any writes or external events since the backup before considering a database restore.

## Release record template

Keep the completed operational record in the agreed private location. Store a sanitised summary with the PR where useful.

- Release ID and date/time (include timezone):
- Operator:
- PR(s) and reviewed commit(s):
- Previously deployed release / observed server baseline:
- Package checksum and file manifest (including deleted files):
- Runtime/dependency changes and prerequisites:
- Configuration changes (names only; no secret values):
- Database migrations and recovery implications:
- Backup location and restore verification (private record):
- Exact upload/replacement steps:
- Cache/scheduler/worker actions:
- Checks performed and results:
- Rollback steps and decision point:
- Actual deployed version, outcome and outstanding issues:

Track **proposed**, **merged**, **deployed** and **verified** separately. Do not mark a release deployed or verified solely because its PR was merged.

## This first documentation change

Adds this process and a PR template. No runtime, dependency, configuration, database or asset changes are included. No server upload or command is needed. Rollback consists of reverting the documentation change.
