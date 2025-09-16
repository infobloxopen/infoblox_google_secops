# Infoblox Google SecOps Parser and Integrations

This repository contains SEIM parser, sample playbooks and integration guides for leveraging Infoblox solutions within the Google SecOps platform. Each integration helps automate enrichment, response, threat intelligence, and asset management workflows to enhance your security operations.

## Parser

### 1. Infoblox Parser
Google SecOps SIEM custom parser for Infoblox Cloud logs.
- **Documentation:** [Infoblox Log Forwarding to Google SecOps SIEM](https://docs.infoblox.com/space/DeploymentGuideLogtoGoogleSecOps)

## Integrations

### 1. Infoblox NIOS (On-Prem)
Automate network and security operations using Infoblox NIOS data within Google SecOps SOAR.

- **Playbooks:** Lookups and indicator blocking/unblocking
- **Documentation:** [Infoblox NIOS README](./Infoblox%20NIOS/README.md)

### 2. Infoblox Threat Defense with DDI
Leverage Infoblox Threat Intelligence and DDI for advanced threat detection and response.

- **Playbooks:** Threat intelligence gathering, indicator blocking/unblocking, vulnerability management, and more.
- **Documentation:** [Infoblox Threat Defense with DDI README](./Infoblox%20Threat%20Defense%20with%20DDI/README.md)

## Getting Started

1. **Choose your integration:** Review the descriptions above and select the integration that fits your needs.
2. **Import Playbooks:** Follow the instructions in the respective integration’s README to import playbooks into your Google SecOps SOAR instance.
3. **Customize Workflows:** Use the provided playbooks as references to build or adapt workflows for your environment.

---

*For detailed instructions, examples, and advanced customization, please refer to the README files within each integration folder.*
