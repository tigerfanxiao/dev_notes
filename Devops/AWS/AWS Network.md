
Security Group 和 Network ACL 的区别
- Network ACLs are considered stateless. By default, they allow all traffic in and out of the subnet. In contrast, security groups are stateful. The default configurations of security groups block all inbound traffic and allow all outbound traffic.
- **Scope**: Security groups apply at the instance level, while network ACLs apply at the subnet level.
- **Statefulness**: Security groups are stateful, making them easier to manage for instance-level traffic, whereas network ACLs are stateless and require explicit rules for both inbound and outbound traffic.
- **Rules**: Security groups only support allow rules, while network ACLs support both allow and deny rules.
- **Evaluation**: Security groups evaluate all rules collectively, whereas network ACLs evaluate rules in numerical order.
