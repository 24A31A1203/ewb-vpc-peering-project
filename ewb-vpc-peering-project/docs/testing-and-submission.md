# Testing and college submission

## Required tests
Record the date, instance name, command, actual output and screenshot for each test.

1. **Baseline failure:** Before peering routes, run from App-A: `ping -c 4 10.20.1.10`. Expected: no replies.
2. **Forward path:** After peering is Active, both routes are installed and App-B's ICMP rule is correct, run the same command from App-A. Expected: replies.
3. **Reverse path:** From App-B run `ping -c 4 10.10.2.10`. Expected: replies.
4. **Private SSH:** From your laptop use SSH ProxyJump to connect to App-B through the bastion.
5. **Isolation:** Confirm App-A and App-B have no public IPv4 addresses.
6. **Controlled failure:** Remove only App-B's inbound ICMP Echo Request rule from `10.10.2.10/32`. Repeat forward ping; it should fail. Restore the exact rule and repeat; it should work again. Do not remove the SSH rule.

Expected results are acceptance criteria, not proof that your deployment has passed. Keep your actual output.

## Suggested report structure
1. Cover page: student name, batch, title, Region and completion date.
2. Objective and learning outcomes.
3. Architecture diagram and CIDR plan.
4. Resource inventory with actual IDs.
5. Subnet associations and route table screenshots.
6. Peering status and security group screenshots.
7. Baseline failure and successful forward/reverse tests.
8. App-B SSH-through-bastion proof.
9. Controlled failure and recovery.
10. Troubleshooting and lessons learned.
11. Cleanup evidence.

Do not include private keys, passwords, AWS access keys or session tokens in the report or screenshots.

## Troubleshooting order
- Verify instances are running and status checks pass.
- Verify the destination IP is correct.
- Verify the route table is associated with the intended subnet.
- Verify peering is Active and both VPC CIDRs do not overlap.
- Verify forward and return routes point to the same peering connection.
- Verify destination security group allows the required inbound traffic.
- For SSH, verify username `ec2-user`, key pair, bastion public IP and source IP `/32`.
