# AWS resource inventory template

Fill this in after you create resources. Do not put passwords, private keys or access tokens here.

| Resource | Name | Actual ID | CIDR / IP | Notes |
|---|---|---|---|---|
| Region | Mumbai | `ap-south-1` | — | |
| VPC A | `ewb-vpc-a` | To fill | `10.10.0.0/16` | |
| Public subnet A | `ewb-a-public` | To fill | `10.10.1.0/24` | |
| Private subnet A | `ewb-a-private` | To fill | `10.10.2.0/24` | |
| VPC B | `ewb-vpc-b` | To fill | `10.20.0.0/16` | |
| Private subnet B | `ewb-b-private` | To fill | `10.20.1.0/24` | |
| Internet Gateway | `ewb-igw-a` | To fill | — | |
| Public route table | `ewb-rt-a-public` | To fill | — | |
| Private route table A | `ewb-rt-a-private` | To fill | — | |
| Private route table B | `ewb-rt-b-private` | To fill | — | |
| Bastion | `ewb-bastion` | To fill | `10.10.1.10` + public IP | |
| App-A | `ewb-app-a` | To fill | `10.10.2.10` | No public IP |
| App-B | `ewb-app-b` | To fill | `10.20.1.10` | No public IP |
| Peering | `ewb-a-b-peer` | `pcx-...` | Active? | |
| Key pair | `ewb-lab` | To fill | Never upload key | |

Tag resources with `Project = EWB-VPC-Peering` and `Student = your name`, as required by the workbook.
