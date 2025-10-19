# Distributed Zabbix  
_A modular automation framework for deploying and managing distributed Zabbix monitoring infrastructure_

---

## Table of Contents  
1. [Project Overview](#project-overview)  
2. [Why Use This Framework](#why-use-this-framework)  
3. [Features & Scope](#features-and-scope)  
4. [Repository Structure](#repository-structure)  
5. [Getting Started](#getting-started)  
   1. [Prerequisites](#prerequisites)  
   2. [Installation](#installation)  
   3. [Quick Start Usage](#quick-start-usage)  
6. [Use Cases & Scenarios](#use-cases-and-scenarios)  
7. [Customisation & Extending the Framework](#customisation-and-extending-the-framework)  
8. [Best Practices and Operational Tips](#best-practices-and-operational-tips)  
9. [CI / QA / Testing](#ci-qa-testing)  
10. [Security Considerations](#security-considerations)  
11. [Contributing](#contributing)  
12. [License](#license)  
13. [Support & Contact](#support-and-contact)  

---

## Project Overview  
Distributed Zabbix is designed to automate the deployment, configuration, and ongoing management of an enterprise-scale, geographically distributed Zabbix monitoring platform.  
It supports multiple nodes (proxies, servers), central management, and standardised monitoring configuration across environments (e.g., production, staging, DR sites).  
With this framework you can:  
- Roll out Zabbix server(s), proxy(es) and agents in a repeatable way  
- Ensure configuration consistency across multiple sites and instances  
- Easily expand the monitoring infrastructure as your environment grows  
- Automate the operational lifecycle: installation, upgrades, configuration, backup, failover  

---

## Why Use This Framework  
Monitoring distributed systems at scale presents unique challenges:  
- Manual setup of each node is error-prone and inconsistent  
- Documentation often diverges from actual deployments  
- Upgrades and configuration changes drift across sites  
- Ensuring uniform monitoring policies and templates across multiple Zabbix instances is hard  

This framework addresses those challenges by:  
- Providing declarative infrastructure-as-code for Zabbix components  
- Treating monitoring nodes (servers, proxies, agents) as managed assets  
- Ensuring configuration becomes version-controlled, peer-reviewed and reproducible  
- Allowing you to scale out monitoring with confidence and consistency  

---

## Features & Scope  
### Included by default  
- Infrastructure provisioning playbooks/roles for Zabbix **Server**, **Proxy**, **Agent**  
- Role parameterisation for OS variants (e.g., RHEL, CentOS, Ubuntu)  
- Templates and variables for core Zabbix components (database, web UI, services)  
- Inventory and group structure for multi-site/multi-instance setups  
- Configuration management for Zabbix proxies to relay data back to central server  
- Backup and retention policies for Zabbix database and configuration  
- Tagging support to selectively deploy components (–tags “proxy”, “server”, “agent”)  

### Scope (Expansion)  
- Add custom templates and host-group logic per environment  
- Integrate Zabbix with other infrastructure services (e.g., cloud, container platforms)  
- Automated upgrade paths (minor/major version)  
- Multi-region failover, clustering of Zabbix servers, scaling proxies dynamically  

---

## Repository Structure  


> Note: Adjust the filenames, folders and names if they differ in your actual repo.

---

## Getting Started  

### Prerequisites  
- A control node running a supported OS (e.g., RHEL 8/9, Ubuntu 20.04+) with Ansible installed (version 2.10+ or 2.12+ recommended).  
- Target hosts (servers, proxies, agents) reachable via SSH and configured with appropriate privileges (sudo or root).  
- For database backing the Zabbix server: MySQL/MariaDB or PostgreSQL as per your environment.  
- Inventory up-to-date with host groups (e.g., `zabbix_servers`, `zabbix_proxies`, `zabbix_agents`).  
- If using encrypted secrets (DB passwords, API keys), ensure you use `ansible-vault` and keep vault key separate from this codebase.  
- Familiarity with Ansible concepts: playbooks, roles, inventory, variables, tags.

### Installation  
```bash
# Clone the repository
git clone https://github.com/majevince/Distributed_Zabbix.git
cd Distributed_Zabbix

# (Optional) Create virtual environment and install Ansible
python3 -m venv .env
source .env/bin/activate
pip install ansible

# Review ansible.cfg, inventory/ and vars/ files.
# Update inventory/production.yml with your site/host definitions.
# Dry-run deploying a Zabbix server
ansible-playbook -i inventory/production.yml playbooks/server.yml --check

# If dry-run looks good, deploy for real
ansible-playbook -i inventory/production.yml playbooks/server.yml

# Deploy a Zabbix proxy
ansible-playbook -i inventory/production.yml playbooks/proxy.yml

# Deploy Zabbix agents to hosts in group ‘monitored_hosts’
ansible-playbook -i inventory/production.yml playbooks/agent.yml --limit monitored_hosts
