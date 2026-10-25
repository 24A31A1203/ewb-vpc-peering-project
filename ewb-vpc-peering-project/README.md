# EWB AWS VPC Peering Project

Beginner-friendly student project based on the EWB Cloud Labs workbook.

## Goal
Connect two non-overlapping VPCs with VPC Peering, keep App-A and App-B private, administer them through a bastion host, and verify connectivity with positive and controlled-negative tests.

## Architecture plan
- Region: `ap-south-1` (Mumbai)
- VPC A: `10.10.0.0/16`
- VPC B: `10.20.0.0/16`
- Public subnet A: `10.10.1.0/24` (bastion)
- Private subnet A: `10.10.2.0/24` (App-A)
- Private subnet B: `10.20.1.0/24` (App-B)
- Instances: 1 bastion + 2 private instances
- VPC B has no Internet Gateway; App-A and App-B have no public IPv4 addresses.

## Important
This repository contains project documentation and a starter GitHub Pages website. It does **not** prove that AWS resources have been created. Add your own AWS resource IDs, screenshots, and real test output only after running the lab.

Never commit AWS passwords, access keys, session tokens, or `.pem` private keys.

## Repository structure
- `index.html`: project overview website (GitHub Pages)
- `architecture/architecture.svg`: conceptual network diagram
- `docs/build-guide.md`: console-based build checklist
- `docs/testing-and-submission.md`: verification and report checklist
- `docs/resource-inventory-template.md`: record actual AWS IDs safely
- `scripts/`: reserved for optional automation after the manual build is understood

## Publish the website
1. Create a GitHub repository named `ewb-vpc-peering-project`.
2. Upload the files from this folder to the repository's root.
3. In GitHub, open **Settings → Pages**.
4. Under Build and deployment, select **Deploy from a branch**.
5. Choose branch `main` and folder `/ (root)`, then Save.
6. Wait for GitHub Pages to publish. The URL normally looks like:
   `https://YOUR-GITHUB-USERNAME.github.io/ewb-vpc-peering-project/`

Replace `YOUR-GITHUB-USERNAME` with your own username. This is a documentation site, not the AWS network itself.

## Next step
Follow `docs/build-guide.md` and build AWS resources in the console. Check current AWS pricing before launching EC2 or allocating public IPv4 addresses. Delete lab resources when finished.
