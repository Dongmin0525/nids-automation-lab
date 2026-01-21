# NIDS Automation Lab

This repository provides an automated capstone lab for a Network Intrusion Detection System (NIDS) built with Suricata integrated into the Elastic Stack (Elasticsearch, Kibana, Filebeat). It uses infrastructure as code and configuration management to deploy a complete lab environment.

## Contents

- `ansible/` – Ansible playbook and roles to install and configure Suricata and the Elastic Stack on the lab hosts.
- `terraform/` – Terraform configuration to build a small cloud environment for the lab (VPC, instances).
- `suricata/` – Example Suricata configuration (`suricata.yaml`) tuned for sending JSON logs to Elastic.
- `elastic/` – Saved Kibana dashboards (ndjson) to visualize Suricata alerts, including top attacker IPs and rule hit rankings.
- `RUNBOOK.md` – Step-by-step runbook to validate the lab environment and test detection capabilities.
- `LAB_SETUP.md` – Guide describing how to set up and use the lab.

## Overview

Suricata monitors network traffic for suspicious patterns. Its `eve.json` log file is parsed by Filebeat and sent to Elasticsearch, where events are stored and indexed for search. Kibana dashboards allow you to explore the alerts and metrics.

The deployment scripts in this repository automate the installation of these components, configure Suricata with appropriate rulesets, and stand up an Elastic Stack with authentication enabled. Terraform scripts provision the underlying compute resources and networking.

Refer to the lab setup guide for detailed instructions and to the runbook for validation scenarios.
