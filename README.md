🚀 Redis Cluster Lifecycle Tool (Ansible + CLI)

A CLI-based automation tool that provisions, manages, and performs zero-downtime rolling upgrades on a 6-node Redis Cluster using Ansible + Docker/Podman containers.

This project simulates production-like infrastructure using containers with SSH access and fully automates Redis cluster lifecycle operations.

📌 Project Overview

redis-tool is a command-line orchestration tool that wraps Ansible playbooks to manage a Redis Cluster lifecycle.

It supports:

Infrastructure provisioning (6-node Redis cluster)
Cluster setup (3 masters + 3 replicas)
Data seeding and verification
Cluster status monitoring
Zero-downtime rolling upgrades
Full post-upgrade validation

All operations are fully automated — no manual SSH or redis-cli usage.

🏗️ Infrastructure Design

The system uses 6 Ubuntu-based containers:

redis-node-1 → 10.10.0.11
redis-node-2 → 10.10.0.12
redis-node-3 → 10.10.0.13
redis-node-4 → 10.10.0.14
redis-node-5 → 10.10.0.15
redis-node-6 → 10.10.0.16
Key Features:
SSH enabled containers
Static IP networking (Docker/Podman bridge)
Passwordless SSH (key-based authentication)
Ansible control node runs from host machine
⚙️ Tech Stack
Ansible (automation engine)
Redis 7.x (cluster mode)
Docker / Podman (container runtime)
Bash / Python CLI (redis-tool)
SSH-based orchestration
🧰 CLI Tool — redis-tool
1️⃣ Provision Cluster
./redis-tool provision --version 7.0.15 --masters 3 --replicas-per-master 1

✔ Installs Redis on all nodes
✔ Configures cluster mode
✔ Forms cluster (3 masters + 3 replicas)
✔ Prints final topology

2️⃣ Seed Data
./redis-tool data seed --keys 1000

✔ Generates deterministic key-value pairs
✔ Inserts into cluster
✔ Shows distribution summary

3️⃣ Verify Data
./redis-tool data verify

✔ Reads all keys
✔ Recomputes expected values
✔ Validates integrity (PASS/FAIL)

4️⃣ Cluster Status
./redis-tool status

✔ Shows:

Node roles (master/replica)
Slot ranges
Memory usage
Cluster health
Replication status
5️⃣ Rolling Upgrade (Zero Downtime)
./redis-tool upgrade --target-version 7.2.6 --strategy rolling

✔ Upgrades replicas first
✔ Promotes replicas for masters (failover)
✔ Upgrades old masters safely
✔ Ensures cluster remains OK throughout
✔ No data loss, no downtime

6️⃣ Full Verification
./redis-tool verify --full

✔ Data integrity check
✔ Version consistency
✔ Slot coverage validation
✔ Replication health check
✔ Cluster state verification

🔄 Rolling Upgrade Strategy
Pre-checks
Cluster health validation
Version check
Data integrity baseline
Upgrade Replicas First
One node at a time
Wait for full sync before next node
Upgrade Masters via Failover
Trigger cluster failover
Promote replica to master
Upgrade old master safely
Final Validation
Verify all nodes upgraded
Run full data verification
Confirm cluster stability

✔ Ensures zero downtime + no data loss

📁 Project Structure
submission/
├── redis-tool
├── ansible/
│   ├── playbooks/
│   │   ├── provision.yml
│   │   ├── upgrade.yml
│   │   └── status.yml
│   └── roles/
├── infra/
│   ├── compose.yml
│   └── Containerfile
├── inventory/
│   └── hosts.ini
├── redis/
│   ├── tasks/
│   ├── templates/
│   └── handlers/
├── logs/
└── README.md
🧪 Prerequisites

Before running the tool:

Docker Engine OR Podman
Ansible 2.14+
SSH key setup

Check automatically:

./redis-tool check
🚀 How to Run
# Start infrastructure
docker compose up -d

# Provision cluster
./redis-tool provision --version 7.0.15

# Seed data
./redis-tool data seed --keys 1000

# Upgrade cluster
./redis-tool upgrade --target-version 7.2.6
📊 Key Features
✔ Fully automated Redis Cluster lifecycle
✔ Zero-downtime rolling upgrades
✔ Deterministic data verification
✔ Ansible-based infrastructure automation
✔ Docker/Podman compatible
✔ Production-like simulation environment
⚠️ Notes
No external Redis modules used (pure Ansible roles)
No Kubernetes or managed Redis services
Designed for learning + system design + DevOps practice
Works on Linux/macOS
📌 Author

Built as a DevOps + Distributed Systems automation project using Ansible and Redis Cluster architecture principles.
