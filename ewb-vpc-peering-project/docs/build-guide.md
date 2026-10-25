# Build guide — beginner steps

This guide is based on the uploaded EWB workbook. Build manually in the AWS Console first so you understand each resource. Console labels can change.

## Before you start
1. Sign in to your AWS account using the official AWS console.
2. Check whether your account is permitted to create VPC and EC2 resources.
3. Check AWS pricing, free-tier eligibility and billing alerts. This lab can incur EC2, EBS and public IPv4 charges.
4. Select **Asia Pacific (Mumbai), `ap-south-1`** in the Region selector.
5. Never share your password, access keys, session tokens or private `.pem` key.

## Step 1 — Create VPC A
1. Open **VPC → Your VPCs → Create VPC**.
2. Choose **VPC only**.
3. Name: `ewb-vpc-a`; IPv4 CIDR: `10.10.0.0/16`; tenancy: Default.
4. Create it.

## Step 2 — Create VPC A subnets
1. Open **Subnets → Create subnet**, choose `ewb-vpc-a`.
2. Create `ewb-a-public`, CIDR `10.10.1.0/24`.
3. Create `ewb-a-private`, CIDR `10.10.2.0/24`.
4. Use one Availability Zone for the lab if possible.
5. Keep public IPv4 auto-assignment disabled on the private subnet.

## Step 3 — Internet Gateway and VPC A route tables
1. Create Internet Gateway `ewb-igw-a` and attach it to `ewb-vpc-a`.
2. Create route table `ewb-rt-a-public` in VPC A; associate `ewb-a-public`.
3. Add `0.0.0.0/0` → `ewb-igw-a`; keep the local route.
4. Create `ewb-rt-a-private`; associate `ewb-a-private`.
5. Keep only its local route until peering is configured.

## Step 4 — Create VPC B
1. Create VPC `ewb-vpc-b` with CIDR `10.20.0.0/16`.
2. Create subnet `ewb-b-private` with CIDR `10.20.1.0/24`.
3. Create route table `ewb-rt-b-private` and associate the subnet.
4. Keep the local route. Do not create an Internet Gateway or NAT Gateway for VPC B.

## Step 5 — Security groups
Create these groups in the corresponding VPC. Keep default outbound rules for this beginner lab and default network ACLs.

- `ewb-sg-bastion` in VPC A: inbound SSH TCP 22 from **your current public IPv4 address /32 only**.
- `ewb-sg-app-a` in VPC A: inbound SSH TCP 22 from `10.10.1.10/32`; inbound ICMP IPv4 Echo Request from `10.20.1.10/32`.
- `ewb-sg-app-b` in VPC B: inbound SSH TCP 22 from `10.10.1.10/32`; inbound ICMP IPv4 Echo Request from `10.10.2.10/32`.

Do not expose SSH or ICMP to `0.0.0.0/0`. For ICMP Echo Request, select IPv4 ICMP type 8, code 0 if the console asks.

## Step 6 — Create the SSH key pair
1. Open **EC2 → Key pairs → Create key pair**.
2. Name: `ewb-lab`; choose RSA and `.pem`.
3. Download it once and keep it safely on your own computer.
4. Never upload it to GitHub or paste it into chat.

## Step 7 — Launch three EC2 instances
Use Amazon Linux 2023 and an instructor-approved small instance type. Check the price before launch. Use a small root volume with delete-on-termination.

- `ewb-bastion`: VPC A, public subnet, private IP `10.10.1.10`, public IPv4 enabled, group `ewb-sg-bastion`.
- `ewb-app-a`: VPC A, private subnet, private IP `10.10.2.10`, public IPv4 disabled, group `ewb-sg-app-a`.
- `ewb-app-b`: VPC B, private subnet, private IP `10.20.1.10`, public IPv4 disabled, group `ewb-sg-app-b`.

Select the correct VPC/subnet and set the primary private IPv4 address in advanced network settings. Record instance IDs and the bastion public IPv4.

## Step 8 — Baseline test before peering routes
Connect from your laptop to the bastion using your local SSH client, then SSH to App-A using ProxyJump or the SSH configuration described in the workbook. From App-A run:

`ping -c 4 10.20.1.10`

Save the actual output. Before peering and peer routes, no replies are expected. Do not claim a test passed unless you ran it.

## Step 9 — Create VPC Peering
1. In VPC Console, choose **Peering connections → Create peering connection**.
2. Name `ewb-a-b-peer`; requester `ewb-vpc-a`; accepter `ewb-vpc-b`; same account and Region.
3. Create the request, select it, then **Actions → Accept request**.
4. Wait until status is **Active**. Record the `pcx-...` ID.

## Step 10 — Add routes to all three route tables
Keep each existing local route and add these routes, using the actual peering ID as the target:

- `ewb-rt-a-private`: destination `10.20.0.0/16` → VPC peering connection.
- `ewb-rt-b-private`: destination `10.10.0.0/16` → VPC peering connection.
- `ewb-rt-a-public`: destination `10.20.0.0/16` → VPC peering connection.

The public route table needs the peer route so the bastion can open an SSH connection to App-B. The more specific VPC B route is selected instead of the default Internet Gateway route.

## Step 11 — Verify
Follow `testing-and-submission.md`. Capture your own screenshots and real command output.

## Step 12 — Clean up
After saving evidence, terminate all three EC2 instances; verify lab EBS volumes and public IPv4 allocations are removed/released; delete the peering connection; delete security groups after interfaces disappear; delete subnets, custom route tables and Internet Gateway; then delete both VPCs. Check the Billing page for remaining resources. Usage reporting can lag.
