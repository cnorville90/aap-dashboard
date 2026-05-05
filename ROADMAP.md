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

- [ ] Create OAuth2 application (`automation-dashboard-sso`) via AAP Gateway API
- [ ] Capture `client_id` and `client_secret` as Ansible facts
- [ ] Generate personal access token (scope: read)
- [ ] Capture `access_token` and `refresh_token` as Ansible facts
- [ ] Test against a live Product Demos AAP instance
- [ ] Handle idempotency (skip if app already exists)

## Milestone 3 — Phase 2: Dashboard Install

Automate extraction of the setup bundle and execution of the dashboard installer on the RHEL 9 host.

- [ ] Copy and extract bundle from `/media/` to dashboard host
- [ ] Install `ansible-core` and required collections
- [ ] Template installer `inventory` file from vault variables
- [ ] Run `ansible.containerized_installer.dashboard_install`
- [ ] Validate containers are running post-install (`podman ps`)
- [ ] Idempotency: skip install if dashboard already running

## Milestone 4 — Phase 3: Dashboard Configuration

Load cluster connection details into the running dashboard container.

- [ ] Template `clusters.yaml` with OAuth tokens and AAP gateway FQDN
- [ ] Copy `clusters.yaml` into container via `podman cp`
- [ ] Run `manage.py setclusters` to load configuration
- [ ] Verify tokens with `manage.py getclusters --decrypt`
- [ ] Secure cleanup of `clusters.yaml` from disk after load

## Milestone 5 — Phase 4: Data Sync

Trigger the initial data synchronization from AAP to the dashboard.

- [ ] Run `manage.py syncdata` in non-interactive mode with date range
- [ ] Verify sync success message in output
- [ ] Parameterize `--since` date via `initial_sync_days` variable

## Milestone 6 — End-to-End Testing

Full integration test against the Product Demos environment.

- [ ] Provision a fresh Product Demos environment from catalog.demo.redhat.com
- [ ] Run `setup_dashboard.yml` end-to-end with a single command
- [ ] Verify dashboard UI accessible at `https://aap-dashboard-apd.thenorvilles.com:8447`
- [ ] Verify metrics are visible in the dashboard
- [ ] Document any manual pre-requisites that remain

## Milestone 7 — Hardening and Polish

- [ ] Encrypt `vault.yml` and document vault workflow in README
- [ ] Add `vault.yml.example` with placeholder values
- [ ] Add error handling for expired tokens (re-run `setclusters` flow)
- [ ] Add tag support (`--tags install`, `--tags configure`, `--tags sync`)
- [ ] Add TLS certificate generation/copy task for self-signed certs
- [ ] Contribute back to `ansible/product-demos` or publish to Ansible Galaxy
