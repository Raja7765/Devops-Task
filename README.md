 DevOps Engineer Assignment - Redis Cluster Zero-Downtime Upgrade

Author: Raja
Role: Candidate, DevOps Engineer

##  Project Overview

This repository contains the complete infrastructure, configuration management, and automation tooling required to deploy a highly available 6-node Redis cluster and perform a **Zero-Downtime Rolling Upgrade** from version `7.0.15` to `7.2.6`. 

The architecture simulates a bare-metal environment using Docker containers, where Redis is compiled entirely from source via Ansible to ensure absolute control over the binary and configuration. A custom Python-based CLI orchestrator (`redis-tool`) was engineered to manage the cluster state, handle hitless failovers, and verify data integrity.

---

##  Architecture & Tech Stack

* **Infrastructure:** Docker & Docker Compose (Simulating 6 bare-metal Ubuntu servers on a dedicated subnet).
* **Configuration Management:** Ansible (Automated C-compiler installation, source downloading, compilation, and `redis.conf` generation).
* **Database:** Redis (Configured as a highly available cluster with 3 Masters and 3 Replicas covering 16,384 hash slots).
* **Automation & Orchestration:** Custom Python CLI (`redis-tool`) utilizing `redis-py` for state management and cluster topology mapping.

---

##  Core Objectives Achieved

1. **Phase 1: Provisioning:** Successfully built a 6-node Redis cluster (v7.0.15) from scratch using Ansible.
2. **Phase 2: State Injection:** Developed tooling to seed the cluster with exactly 1,000 keys to simulate a live production load.
3. **Phase 3: Zero-Downtime Upgrade:** Engineered a hitless rolling upgrade script. The script takes replicas offline, upgrades them, forces a safe `CLUSTER FAILOVER` to promote them, and then upgrades the demoted masters, ensuring 0% packet loss.
4. **Phase 4: Post-Flight Verification:** Automated a comprehensive 5-step health check verifying data integrity (1000/1000 keys preserved), version consistency across all nodes, and replication link health.

---

##  Stretch Goals Implemented

* **S1 (Scale Out) & S2 (Scale In):** Successfully expanded the cluster from 6 nodes to 8 nodes, executed a live dynamic resharding to transfer 4,096 hash slots without data loss, and safely drained and removed the nodes to return to the baseline topology.
* **S4 (Idempotency):** The orchestrator is state-aware. The `upgrade` command explicitly sweeps node versions before execution. If the cluster is already at the target version, it exits cleanly without attempting an upgrade.
* **S5 (Structured Logging):** The system writes structured JSON audit logs containing timestamps, actions, target nodes, and status codes for every executed command to ensure a clear operational paper trail.

---

##  Repository Structure

```text
├── ansible/
│   ├── ansible.cfg          # Ansible configuration
│   ├── inventory/           # Node mappings and IPs
│   └── playbooks/           # Provisioning and compilation playbooks
├── infra/
│   └── docker-compose.yml   # Infrastructure baseline
├── output/                  # Terminal proofs of successful execution
│   ├── provision_output.txt
│   ├── data_seed_output.txt
│   ├── upgrade_output.txt
│   ├── verify_output.txt
│   └── scale_in_out_proof.txt
├── redis-tool               # The custom Python CLI orchestrator
└── README.md
