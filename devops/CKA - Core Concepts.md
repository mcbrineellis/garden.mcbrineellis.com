---
created: 2024-02-28T18:06
updated: 2024-02-28T18:48
---
## Cluster Architecture
**Worker Nodes**
- Host containers
**Master Nodes** aka the control plane
- Manage, plan, schedule, monitor nodes
	- etcd - key/value store (db)
	- scheduler - where to run stuff, taints, etc
	- 