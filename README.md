# aap-dashboard

Automates the setup and configuration of the AAP 2.6 Automation Dashboard on the Ansible Product Demos environment (catalog.demo.redhat.com).

## Prerequisites

- RHEL 9 dashboard host (`aap-dashboard-apd.thenorvilles.com`) provisioned and accessible
- Setup bundle at `/media/ansible-automation-dashboard-containerized-setup-bundle-0.1-x86_64.tar.gz`
- Ansible Product Demos environment provisioned from the Red Hat Demo Catalog
- Ansible Vault password

## Usage

```bash
ansible-playbook setup_dashboard.yml \
  -e "aap_gateway_fqdn=aap-aap.apps.cluster-XXXXX.dynamic.redhatworkshops.io" \
  --ask-vault-pass
```

## What it does

| Phase | Role | Description |
|---|---|---|
| 1 | `aap_oauth_setup` | Creates OAuth2 app and access token in AAP via REST API |
| 2 | `dashboard_install` | Extracts bundle, templates inventory, runs installer (idempotent) |
| 3 | `dashboard_configure` | Templates `clusters.yaml` and loads it into the dashboard container |
| 4 | `dashboard_sync` | Runs initial data sync from AAP to the dashboard |

## Vault setup

Copy and populate the vault template, then encrypt it:

```bash
cp group_vars/all/vault.yml.example group_vars/all/vault.yml
# Edit vault.yml with real values
ansible-vault encrypt group_vars/all/vault.yml
```

## Variables

| Variable | Description |
|---|---|
| `aap_gateway_fqdn` | AAP gateway FQDN (changes each demo provisioning) |
| `verify_ssl` | TLS verification — `false` for self-signed certs |
| `initial_sync_days` | Days of historical data to sync on first run |
