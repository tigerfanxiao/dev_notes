
### Security Group

### SR-IOV

SR-IOV, or Single Root I/O Virtualization, is a feature of Enhanced Networking used to provide higher networking performance. On a normal EC2 instance, multiple EC2 instances may share a single physical network interface on the EC2 Host. SR-IOV effectively dedicates the interface to a single instance, and bypasses parts of the Hypervisor, allowing for better performance


### Egress internet gateway
用于IPv6出口
### VPC flow logs


### ENI vs ENA vs EFA

**ENI**: Elastic Network Interface - essentially a virtual network card.

**EN**: Enhanced Networking. Uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types.

**Elastic Fabric Adapter**: A network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications.