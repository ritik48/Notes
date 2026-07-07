### IAM (Identity and Access Management)

1. IAM Users
  - Represent an individual person(developer, tester)
  - Can have username + password(console access)
  - Can have access keys(CLI access)
  - With specific permissions

2. IAM groups
  - A collection of users with shared permissions.
  - Example:
      - Group: Developers -> Policy: PowerUserAccess
      - Group: Viewers -> Policy: ReadOnlyAccess

3. IAM Roles
  - Temporary identities assumed by users, apps or AWS services.
  - No permanent credentials (keys are short-lived)
  - Best practice for EC2, Lambda, EKS, CI/CD

4. IAM Policies:
  - JSON documents defining permissions.

#### EC2 (Elastic compute cloud)
- Used for hosting apps + api
- batch jobs, scheduled tasks, background workers

#### S3 (Simple Storage Service)

- Used to store any files here.
- Durable

Buckets: 
- A kind of folder where all files are stored. Bucket names must be unique across all AWS Buckets globally.
- Region based
- We can host statuc websites

#### IP (Internet Protocol)

1. Private IP
    - Accessible only within a private network.
    - Not accessible directly from the public internet.
2. Public IP
    - Accessible from the public internet.
    - Used to communicate with devices outside your private network.
3. Elstic IP
    - A static (fixed) public IPv4 address provided by AWS.
    - Remains the same even if you stop and start an EC2 instance (when associated correctly).
    - Can be reassociated with another EC2 instance if needed.


#### VPC (Virtual Private Cloud)

A VPC (Virtual Private Cloud) is a isolated private network that you create inside AWS to host your cloud resources, such as EC2 instances, databases, and load balancers.

A VPC has a range of IPs

Think of it like this:

- AWS = A city
- VPC = Your private plot of land in that city
- Inside your plot, you decide:
  - The IP address range
  - Which servers can communicate
  - Which servers are public or private
  - What traffic is allowed in and out

A VPC gives you:
- Network isolation
- Better security
- Full control over networking
- Ability to create public and private networks

#### Subnets

A Subnet is a smaller network inside a VPC.
Its of two type:
- Public Subnet
  - Can access the internet.
  - Typically contains:
    - Web servers
    - Load balancers
- Private Subnets
  - No direct internet access.
  - Typically contains:
    - Databases
    - Backend servers
    - Internal services

Within a VPC, there can be both public and private subnet.


```
                    AWS Region
                         │
         ┌──────────────────────────┐
         │          VPC             │
         │ 10.0.0.0/16              │
         │                          │
         │  ┌──────────────┐        │
Internet │  │ Public       │        │
──────────▶ │ Subnet        │        │
Gateway  │  │ 10.0.1.0/24   │        │
         │  │ EC2           │        │
         │  └──────────────┘        │
         │                          │
         │  ┌──────────────┐        │
         │  │ Private      │        │
         │  │ Subnet        │        │
         │  │ 10.0.2.0/24   │        │
         │  │ Database      │        │
         │  └──────────────┘        │
         └──────────────────────────┘
```

#### NAT Gateway

NAT Gateway allows instances with no public IPs to acces the internet. It is a one way connection. The resources inside a private subnet **initiate outbound internet requests**, while **blocking inbound traffic** from the outside world.

#### IGW (Internet Gateway)

It connects VPC to the public internet, allowing two way traffic.

Without an Internet Gateway:

- No internet access.
- Even if an EC2 has a public IP, it cannot be reached from the internet.

#### Route Tables

A route table tells traffic where to go.

#### Security Groups

It acts as a firewall for an EC2 Instance.

Example:

Allow:

- SSH (22)
- HTTP (80)
- HTTPS (443)

Block everything else.

#### NACL (Netwrork ACL/ Network Access Control List)

A Network ACL is a firewall for an entire subnet.
| Security Group   | Network ACL          |
| ---------------- | -------------------- |
| Instance-level   | Subnet-level         |
| Stateful         | Stateless            |
| Only allow rules | Allow and deny rules |

#### CIDR (Classless Inter-Domain Routing)

