# Project #1 & #2: Multi-VPC Isolation & Bidirectional Network Peering Pipeline

## 📋 Real-World Operational Scenario
* **The Business Challenge:** Your organization operates two separate internal application frameworks (Alpha and Omega) that reside in completely isolated custom network environments. To facilitate direct, secure data transfers between them using internal IP paths, you must link the architectures without sending traffic over the vulnerable public internet and without creating a single shared network point of failure.
* **The Technical Resolution:** Provisioned two distinct custom-mode VPC networks (`vpc-alpha` and `vpc-omega`) with independent subnet configurations. Implemented a bidirectional VPC Network Peering mesh (`alpha-to-omega` and `omega-to-alpha`) to bridge the security boundaries and enable low-latency internal routing.

## ⚡ Flattened One-Liner Execution Commands

```bash
# 1. Initialize active environment project context
export MY_PROJ="project-c1a05de0-ba4b-4764-93a"

# 2. Create independent custom-mode VPC network configurations
gcloud compute networks create vpc-alpha --subnet-mode=custom --project=\$MY_PROJ
gcloud compute networks create vpc-omega --subnet-mode=custom --project=\$MY_PROJ

# 3. Allocate localized subnetworks with non-overlapping IP spaces
gcloud compute networks subnets create subnet-alpha --network=vpc-alpha --region=us-central1 --range=10.10.0.0/24 --project=\$MY_PROJ
gcloud compute networks subnets create subnet-omega --network=vpc-omega --region=us-central1 --range=10.20.0.0/24 --project=\$MY_PROJ

# 4. Provision compute resources mapped to individual subnets
gcloud compute instances create vm-alpha --zone=us-central1-a --machine-type=e2-micro --subnet=subnet-alpha --tags=alpha-nodes --project=\$MY_PROJ
gcloud compute instances create vm-omega --zone=us-central1-a --machine-type=e2-micro --subnet=subnet-omega --tags=omega-nodes --project=\$MY_PROJ

# 5. Establish bidirectional VPC Network Peering pathways
gcloud compute networks peerings create alpha-to-omega --network=vpc-alpha --peer-network=vpc-omega --project=\$MY_PROJ
gcloud compute networks peerings create omega-to-alpha --network=vpc-omega --peer-network=vpc-alpha --project=\$MY_PROJ
```

## 🔍 Validation Protocol
Verify that both paths show an `ACTIVE` status message using these evaluation queries:
```bash
gcloud compute networks peerings list --network=vpc-alpha --project=\$MY_PROJ
gcloud compute networks peerings list --network=vpc-omega --project=\$MY_PROJ
```
