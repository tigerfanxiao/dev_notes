
### Security Group VS Network ACL 
- Network ACLs are considered stateless. By default, they allow all traffic in and out of the subnet. In contrast, security groups are stateful. The default configurations of security groups block all inbound traffic and allow all outbound traffic.
- **Scope**: Security groups apply at the instance level, while network ACLs apply at the subnet level.
- **Rules**: Security groups only support allow rules, default reject all, while network ACLs support both allow and deny rules.
- **Evaluation**: Security groups evaluate all rules collectively, whereas network ACLs evaluate rules in numerical order, when hit then return

SG chain
3-tier app 每一层都有的组件都有自己的 SG. 下一层允许上一层的 SG, 就是是 SG Chain

