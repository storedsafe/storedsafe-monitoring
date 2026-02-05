# StoredSafe Monitoring (Prometheus / Grafana)

This repository provides a **reference monitoring stack** for StoredSafe using
**SNMPv3**, **Prometheus**, and **Grafana**.

It is intended as an example and starting point for customers, partners, and
integrators who want to monitor StoredSafe appliances using standard
open-source monitoring tools.

## Overview

StoredSafe exposes health and operational metrics via **SNMPv3** using
Net-SNMP extend (`NET-SNMP-EXTEND-MIB`).

This repository contains:

- Example **Prometheus** configuration
- Example **snmp_exporter** configuration
- Pre-built **Grafana dashboards** for:
  - Operational health
  - Security and policy compliance
  - Licensing and administrative state
- Example **Prometheus alert rules**

All monitoring components run **outside** the StoredSafe appliance.

## What this repository is (and is not)

### This repository **is**:

- A **reference implementation**
- A **working example** of how to monitor StoredSafe
- A convenient way to get started with Prometheus and Grafana

### This repository **is not**:

- Required to operate StoredSafe
- A supported monitoring product
- A replacement for your own monitoring standards or policies

StoredSafe does **not** depend on Prometheus, Grafana, or any specific monitoring
stack.

## Requirements

- StoredSafe **version 4.1.0 or later** (build 7120+)
- **SNMPv3** access configured in the StoredSafe management console
- SNMPv3 security level: **authPriv** (authentication + encryption)

SNMPv1, SNMPv2c, and SNMPv3 without privacy (`authNoPriv`) are not supported.

## Repository structure

```text
.
├── docker-compose.yml        # Prometheus + Grafana stack
├── prometheus.yml            # Prometheus configuration
├── snmp.yml                  # snmp_exporter configuration
├── alerts.yml                # Example Prometheus alerts
├── rules.yml                 # Recording / alert rules
├── dashboards/               # Grafana dashboards
│   ├── storedsafe-dashboard.json
│   ├── storedsafe-dashboard-compliance-licensing.json
│   └── storedsafe-dashboard-security-policy.json
└── provisioning/
    ├── datasources/          # Grafana data sources
    └── dashboards/           # Grafana dashboard provisioning
```

## Quick start (example)

1. Clone the repository:

```bash
git clone https://github.com/storedsafe/storedsafe-monitoring.git
cd storedsafe-monitoring
```

2. Configure SNMPv3 credentials in the StoredSafe management console.

3. Update `prometheus.yml` with the IP-adress or hostname of your StoredSafe instance.

4. Update `snmp.yml` with your SNMPv3 credentials.

5. Start the stack:

```bash
docker-compose up -d
```

6. Open Grafana (default):

```bash
http://localhost:3000
```

## Dashboards

Dashboards are intentionally separated to reflect different operational concerns:

- **Operational health**
- **Security and policy compliance**
- **Licensing and administrative state**

The `ok` metric provides a high-level health indicator, while dashboards and
alerts are designed to help identify the underlying cause.

## Alerts

Prometheus alert rules included in this repository are **examples**.

Alert thresholds and routing should be adapted to your local operational and
security policies.

## Security considerations

- SNMP access is **read-only**
- SNMPv3 with authentication and encryption is mandatory
- No secrets, credentials, or sensitive payload data are exposed via metrics
- StoredSafe does not initiate outbound connections for monitoring

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**.

See the LICENSE file for details.

## Disclaimer

This repository is provided **as-is** and without warranty.

It is intended as a reference and example only. Customers are responsible for
validating and adapting configurations to their own environments.
