# Roadmap

Automated setup and configuration of the AAP 2.6 Automation Dashboard on the Ansible Product Demos environment (catalog.demo.redhat.com).

## Milestone 1 — Project Foundation ✅

Initial project structure, roles skeleton, and documentation.

- [x] Create GitHub repository
- [x] Define role structure (`aap_oauth_setup`, `dashboard_install`, `dashboard_configure`, `dashboard_sync`)
- [x] Master playbook (`setup_dashboard.yml`)
- [x] Variable and vault scaffolding
- [x] Jinja2 templates for `inventory` and `clusters.yaml`
- [x] README and MIT license

## Milestone 2 — Phase 1: AAP OAuth Setup

Automate creation of the OAuth2 application and access token in AAP via REST API.

- [x] [#1](https://github.com/cnorville90/aap-dashboard/issues/1) Create OAuth2 application (`automation-dashboard-sso`) via AAP Gateway API
- [x] [#1](https://github.com/cnorville90/aap-dashboard/issues/1) Capture `client_id` and `client_secret` as Ansible facts
- [x] [#2](https://github.com/cnorville90/aap-dashboard/issues/2) Generate personal access token (scope: read)
- [x] [#2](https://github.com/cnorville90/aap-dashboard/issues/2) Capture `access_token` and `refresh_token` as Ansible facts
- [ ] [#4](https://github.com/cnorville90/aap-dashboard/issues/4) Test against a live Product Demos AAP instance
- [x] [#3](https://github.com/cnorville90/aap-dashboard/issues/3) Handle idempotency (skip if app already exists)

## Milestone 3 — Phase 2: Dashboard Install

Automate extraction of the setup bundle and execution of the dashboard installer on the RHEL 9 host.

- [x] [#5](https://github.com/cnorville90/aap-dashboard/issues/5) Copy and extract bundle from `/media/` to dashboard host
- [x] [#5](https://github.com/cnorville90/aap-dashboard/issues/5) Install `ansible-core` (collections are pre-bundled — no internet required)
- [x] [#6](https://github.com/cnorville90/aap-dashboard/issues/6) Template installer `inventory` file from vault variables
- [x] [#7](https://github.com/cnorville90/aap-dashboard/issues/7) Run `ansible.containerized_installer.dashboard_install`
- [x] [#7](https://github.com/cnorville90/aap-dashboard/issues/7) Validate containers are running post-install (`podman ps`)
- [x] [#8](https://github.com/cnorville90/aap-dashboard/issues/8) Idempotency: skip install if dashboard already running

## Milestone 4 — Phase 3: Dashboard Configuration

Load cluster connection details into the running dashboard container.

- [ ] [#9](https://github.com/cnorville90/aap-dashboard/issues/9) Template `clusters.yaml` with OAuth tokens and AAP gateway FQDN
- [ ] [#9](https://github.com/cnorville90/aap-dashboard/issues/9) Copy `clusters.yaml` into container via `podman cp`
- [ ] [#9](https://github.com/cnorville90/aap-dashboard/issues/9) Run `manage.py setclusters` to load configuration
- [ ] [#10](https://github.com/cnorville90/aap-dashboard/issues/10) Verify tokens with `manage.py getclusters --decrypt`
- [ ] [#11](https://github.com/cnorville90/aap-dashboard/issues/11) Secure cleanup of `clusters.yaml` from disk after load

## Milestone 5 — Phase 4: Data Sync

Trigger the initial data synchronization from AAP to the dashboard.

- [ ] [#12](https://github.com/cnorville90/aap-dashboard/issues/12) Run `manage.py syncdata` in non-interactive mode with date range
- [ ] [#12](https://github.com/cnorville90/aap-dashboard/issues/12) Verify sync success message in output
- [ ] [#13](https://github.com/cnorville90/aap-dashboard/issues/13) Parameterize `--since` date via `initial_sync_days` variable

## Milestone 6 — End-to-End Testing

Full integration test against the Product Demos environment.

- [ ] [#14](https://github.com/cnorville90/aap-dashboard/issues/14) Provision a fresh Product Demos environment from catalog.demo.redhat.com
- [ ] [#14](https://github.com/cnorville90/aap-dashboard/issues/14) Run `setup_dashboard.yml` end-to-end with a single command
- [ ] [#14](https://github.com/cnorville90/aap-dashboard/issues/14) Verify dashboard UI accessible at `https://aap-dashboard-apd.thenorvilles.com:8447`
- [ ] [#14](https://github.com/cnorville90/aap-dashboard/issues/14) Verify metrics are visible in the dashboard
- [ ] [#15](https://github.com/cnorville90/aap-dashboard/issues/15) Document any manual pre-requisites that remain

## Milestone 7 — Hardening and Polish

- [ ] [#16](https://github.com/cnorville90/aap-dashboard/issues/16) Add `vault.yml.example` with placeholder values
- [ ] [#16](https://github.com/cnorville90/aap-dashboard/issues/16) Encrypt `vault.yml` and document vault workflow in README
- [ ] [#19](https://github.com/cnorville90/aap-dashboard/issues/19) Add error handling for expired tokens (re-run `setclusters` flow)
- [ ] [#17](https://github.com/cnorville90/aap-dashboard/issues/17) Add tag support (`--tags install`, `--tags configure`, `--tags sync`)
- [ ] [#18](https://github.com/cnorville90/aap-dashboard/issues/18) Add TLS certificate generation/copy task for self-signed certs
- [ ] Contribute back to `ansible/product-demos` or publish to Ansible Galaxy
