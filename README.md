# AWS VPC Peering Lab

## Project Overview

This project demonstrates how to establish private communication between two Amazon Virtual Private Clouds (VPCs) using **AWS VPC Peering**.

The lab was completed as part of the EWB AWS VPC Peering Student Project. It covers VPC creation, subnet configuration, route tables, security groups, connectivity testing, and secure access to private EC2 instances through a bastion host.

## Objectives

- Create and configure two separate VPCs.
- Configure public and private subnets.
- Establish a VPC peering connection.
- Configure route tables for communication between VPCs.
- Configure security groups to control network traffic.
- Test connectivity using ICMP ping.
- Access private EC2 instances through a bastion host.
- Understand troubleshooting, failure testing, and recovery.
- Learn how to clean up AWS resources after completing a lab.

## AWS Architecture

The project uses the following architecture:

```text
                    Internet
                       |
                 Internet Gateway
                       |
              VPC A: 10.10.0.0/16
                       |
             Public Subnet
             10.10.1.0/24
                       |
                 Bastion Host
                 10.10.1.39
                       |
             Private Subnet
             10.10.2.0/24
                       |
                    App-A
                10.10.2.192
                       |
                VPC Peering
                       |
              VPC B: 10.20.0.0/16
                       |
             Private Subnet
             10.20.1.0/24
                       |
                    App-B
                10.20.1.96
```

**Note:** The bastion host provides SSH access to private instances. App-A and App-B do not require public IPv4 addresses for private communication over the VPC peering connection.

## AWS Resources

| Resource | Configuration |
|---|---|
| AWS Region | Mumbai (`ap-south-1`) |
| VPC A CIDR | `10.10.0.0/16` |
| VPC A Public Subnet | `10.10.1.0/24` |
| VPC A Private Subnet | `10.10.2.0/24` |
| VPC B CIDR | `10.20.0.0/16` |
| VPC B Private Subnet | `10.20.1.0/24` |
| Bastion Host | `10.10.1.39` |
| App-A Private IP | `10.10.2.192` |
| App-B Private IP | `10.20.1.96` |
| VPC Peering Connection | `pcx-0e2a0f1d635f24a36` |

## Route Table Configuration

The route tables were configured to enable communication between the two VPCs.

### VPC A Public Route Table

- Local route: `10.10.0.0/16`
- Internet route: `0.0.0.0/0` through the Internet Gateway
- Peering route: `10.20.0.0/16` through the VPC peering connection

### VPC A Private Route Table

- Local route: `10.10.0.0/16`
- Peering route: `10.20.0.0/16` through the VPC peering connection

### VPC B Private Route Table

- Local route: `10.20.0.0/16`
- Peering route: `10.10.0.0/16` through the VPC peering connection

## Security Configuration

Security groups were configured to permit only the required traffic.

- SSH access to the bastion host was restricted to the administrator's public IP address.
- SSH access to private instances was permitted from the bastion host.
- ICMP traffic was allowed between App-A and App-B for connectivity testing.
- Private instances were accessed through the bastion host rather than by assigning them public IP addresses.

## Testing and Verification

The following tests were performed during the lab:

### 1. SSH Access to Bastion Host

Verified SSH access from the local computer to the bastion host.

### 2. SSH Access to App-A

Connected to App-A through the bastion host.

### 3. App-A to App-B Connectivity

Executed the following command on App-A:

```bash
ping -c 4 10.20.1.96
```

**Result:** Four packets were transmitted and received successfully, with zero packet loss.

### 4. App-B to App-A Connectivity

Executed the following command on App-B:

```bash
ping -c 4 10.10.2.192
```

**Result:** Four packets were transmitted and received successfully, with zero packet loss.

### 5. SSH Access to App-B

Verified SSH access to App-B through the bastion host without assigning App-B a public IPv4 address.

### 6. Controlled Failure and Recovery

Temporarily removed the relevant ICMP inbound rule to test connectivity failure, then restored the rule to verify recovery.

### 7. Baseline Test

The pre-peering connectivity test was not captured during the initial configuration. It is therefore not presented as verified evidence.

## Key Learnings

Through this project, I learned:

- How AWS VPCs and subnets organize cloud networks.
- How VPC peering enables private communication between VPCs.
- How route tables determine the path of network traffic.
- How security groups control inbound and outbound traffic.
- How to access private EC2 instances securely through a bastion host.
- How to troubleshoot connectivity issues involving routes and security rules.
- How to validate network communication through ping and SSH tests.
- Why AWS lab resources should be cleaned up to prevent unnecessary charges.

## Repository Structure

The repository can be organized as follows:

```text
aws-vpc-peering-lab/
├── README.md
├── report/
│   └── Dinesh_Baidya_EWB_VPC_Peering.pdf
├── diagrams/
│   └── vpc-peering-architecture.png
└── screenshots/
    ├── vpc-overview.png
    ├── peering-connection.png
    ├── route-tables.png
    ├── security-groups.png
    ├── forward-ping.png
    ├── reverse-ping.png
    ├── ssh-via-bastion.png
    └── cleanup-evidence.png
```

Only include files and screenshots that you have actually created.

## Cleanup

After collecting the required evidence and completing the report, verify that the lab's EC2 instances, VPC peering connection, and other lab networking resources have been removed if the lab is no longer needed.

Check for remaining EBS volumes, snapshots, Elastic IP addresses, and other billable resources. Delete only resources belonging to this lab.

**Cleanup status:** Update this section after verifying the resources in the AWS Console.

## Conclusion

This project demonstrates private communication between two AWS VPCs using VPC peering, appropriate route tables, and security group rules. Successful bidirectional connectivity tests and SSH access through a bastion host demonstrate the practical application of AWS networking concepts.

---

**Project:** AWS VPC Peering Lab  
**Author:** Dinesh Baidya  
**Cloud Platform:** Amazon Web Services (AWS)  
**Region:** Asia Pacific (Mumbai)
