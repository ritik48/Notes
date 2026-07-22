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

#### Regions and Availability zones (Azs)

**Regions:** A Region is a separate geographic location where AWS operates.
Example:
```
ap-south-1 → Mumbai
ap-south-2 → Hyderabad
us-east-1 → N. Virginia
eu-west-1 → Ireland
```

Each region has multiple Availability zones.

**Availability zones:** An Availability Zone is one or more physically separate data centers inside a region.

Example:
```
Region: Mumbai (ap-south-1)

├── ap-south-1a
├── ap-south-1b
└── ap-south-1c
```

Why multiple AZs?

Suppose you launch an EC2 instance only in ap-south-1a.
```
ap-south-1

AZ A
 EC2

AZ B

AZ C
```
If AZ A has a power failure, then the application goes down.

Insteade, launch instances in multiple AZs:
```
ap-south-1

AZ A
 EC2

AZ B
 EC2

AZ C
 Load Balancer
```

Now if AZ A fails:

- Traffic automatically goes to AZ B.
- Application stays available.

This is called high availability.

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

### IP (Internet Protocol)

IPv4 address is made of 32 bits.

eg: 192.168.0.10

And each part is repesented by 8 bits.

This IP is made up of:
- **Network bits:**
  Identifies which network the device belongs to,
- **Host bits**
  Identifies the device within the network.

Example in this IP: 192.168.1.10

192.158.1.x: Its the network address
x=10, identifies the device

So within this network, all the devices wil have the same network bits and only the host bits will change.

![alt text](/images/aws1.png)
---


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

It acts as a firewall for an EC2 Instance. With this, we can define only what all traffic is "allowed". we cannot add any "deny" rule.

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

In NACL we defines rules with **Rule Number**, and the smalest rule number takes the priority.

Example of rules in NACL:
| Rule # | Protocol | Source            | Port | Action |
| -----: | -------- | ----------------- | ---- | ------ |
|    100 | TCP      | `0.0.0.0/0`       | 80   | ALLOW  |
|    200 | TCP      | `192.168.1.10/32` | 80   | DENY   |
|      * | All      | All               | All  | DENY   |

A request comes from 192.168.1.10 to port 80.
AWS checks:

1. Rule 100 → Does it match?
    - Source 0.0.0.0/0 includes 192.168.1.10 ✅
    - Port 80 matches ✅
    - Action = ALLOW
2. AWS stops here.
    - Rule 200 is never checked, even though it is more specific.

So the request is allowed.

---

#### CIDR (Classless Inter-Domain Routing)

Its is a way of representing IP addresses and network ranges.

It's used to define network ranges for things like:

- Virtual Private Clouds (VPCs) in AWS, Azure, or GCP
- Firewalls and security groups
- Routing tables
- VPN configurations
- Kubernetes networking
- IP allowlists

A CIDR notation looks like this:
```
192.168.1.0/24
```
It is made up of two parts:
```
<Ip-Address>/<Prefix-length>
```

1. 192.168.1.0 -> network address
2. /24 -> the prefix length, it tells us how many bits are used for the network portion of the IP address.

###### How the /24 works?

An IPv4 address has 32 bits.
```
192.168.1.0
11000000.10101000.00000001.00000000
```
/24 means:

- First 24 bits = network
- Remaining 8 bits = hosts

```
Network bits                 Host bits
11000000.10101000.00000001 | 00000000
```

Since there are 8 host bits, so total posible address is calculated as:
```
2^8 = 256 addresses
```

That means:

- Network: 192.168.1.0
- First usable: 192.168.1.1
- Last usable: 192.168.1.254
- Broadcast: 192.168.1.255

So, total device that can cxonnect to this network is 254. And if message is snet to the ip 192.168.0.255 then all the device connected to the network receives it as it is the broadcast address.

*Note:*

>/24: Its also called Netmask


**Question.** 

```
How many device is possible in this CIDR 10.0.0.0/16 ?

Total bits allowed for host is 16. Which mean:

Network bits: 16

Hotst bits: 16

that means: 10.0.x.x, 3rd and 4th octet are free to vary

In host bits:

Minimum:
00000000.00000000
=
10.0.0.0


Maximum:
11111111.11111111
=
10.0.255.255


So the range of ip address is: 

10.0.0.0
↓
10.0.255.255

Total addresses possible = 2^16 => 65,536
Total device possible = 2^16 - 2 => 65,534
```

Question.

```
Given IP address 192.168.1.70/27.
What is the ramge of IP address, and how many devices are supported ?

/27: It means networkm bits are made of 27 bits and host bits is made of 32-27=5 bits

Comvert the IP to binary:

192.168.1.70
↓
11000000.10101000.00000001.01000110

Now divide after 27 bits.

11000000.10101000.00000001.010 | 00110
                                ↑
                           split here

See, the last octet is split into: 010 | 00110
The first 3 bts belong to the network. The remaioning 5 belongs to the host.

To get the the network address:
Keep the network bits the same, but set all the host bits to 0.

So it becomes: 11000000.10101000.00000001.01000000

01000000 => 64

So network address is: 192.168.1.64

To get the broadcast address:
Keep the network bits the same, but set all the host bits to 1.

So it becomes: 11000000.10101000.00000001.01011111
01011111 => 95

So broadcast address is: 192.168.1.95

So total address possible for this subnet is: 64 to 95 = 32 address
Total device possible: 32-2 = 30
```


#### Subnet mask?

A subnet mask is another way of representing the network portion of an IP address. It is equivalent to CIDR notation.

The subnet mask tells the computer: **"These bits belong to the network, and these bits belong to the host."**


```
/24

can be written as 

255.255.255.0
```
It is becasue, 255 = 11111111

So, 255.255.255.0

becomes

```
11111111
11111111
11111111
00000000


24 1s and 8 0s

The 1s mean:
This bit belongs to the network.

The 0s mean:
This bit belongs to the host.


So, 
255.255.255.0 is exactly the same as /24
```

Question.

```
What is the subnet mask for /27

27: network bits 27, host bits 5

So 27 1s and 5 0s

11111111
11111111
11111111
11100000

11100000 => 224

So subnet mask = 255.255.255.224
```

#### Creating a VPC with Subnets

Lets say we create a VPC with CIDR block: 10.0.0.0/16

Now within this vpc we can huve multiple subnets. So for those subnets too we have to assign CIDR block.

Let's say we create three subnets:
```
Subnet 1: 10.0.1.0/24
Subnet 2: 10.0.10.0/24
Subnet 3: 10.0.120.0/24


Each subnet here having 32 addresses.
```

Can we create subnet with CIDR 10.1.1.0/24?

No. Because 10.1.x.x is outside of the VPC range which is 10.0.x.x

We also cannot create overlapping subnet CIDRs


![alt text](/images/aws2.png)

### Practice: Create this network in AWS

![alt text](/images/aws3.png)

---

#### Load Balancer(ALB)

A Load Balancer distributes incoming traffic across multiple backend servers (EC2 instances) to improve availability, fault tolerance, and scalability.

Without a Load Balancer:

- One EC2 handles all traffic.
- If the EC2 fails → application goes down.
- Heavy traffic can overload the server.

With a Load Balancer:

- Traffic is distributed across multiple EC2 instances.
- If one EC2 fails, requests are sent to healthy instances.
- Applications can scale horizontally.

Load balancer lets us do different Routings like:
- Path based (/api, /admin)
- host based (api.example.com, shop.example.com)


Flow
```
Users
   │
   ▼
Application Load Balancer
   │
   ▼
Target Group
   ├── EC2-1
   ├── EC2-2
   └── EC2-3
```

Features:
- Distributes traffic across multiple Availability Zones.
- Supports SSL/TLS termination.
- Performs health checks.
- Supports path-based routing (/api, /admin).
- Supports host-based routing (api.example.com, shop.example.com).

#### Target Group

A Target Group is a logical group of backend resources that receives traffic from a Load Balancer.

Targets can be:

- EC2 instances
- IP addresses
- Lambda functions

Why is it needed?

The Load Balancer does not directly send traffic to EC2 instances.

Instead:
```
Load Balancer
      │
      ▼
Target Group
      │
      ├── EC2-1
      ├── EC2-2
      └── EC2-3
```

The Target Group keeps track of:

- Registered targets
- Health status
- Port number
- Protocol

Suppose:
```
EC2-1 ✓ Healthy

EC2-2 ❌ Crashed

EC2-3 ✓ Healthy
```
The Target Group reports this to the Load Balancer.

Now the Load Balancer sends traffic only to:
```
EC2-1

EC2-3
```


#### Auto Scaling Groups (ASG)

An Auto Scaling Group (ASG) automatically launches or terminates EC2 instances based on demand or health.

With ASG we can define:
1. Scaling Policies
    ```
    CPU > 70%
    Launch 2 EC2 instances
    ```
    ```
    CPU < 20%
    Terminate 1 EC2 instance
    ```

2. Desired, Minimum & Maximum Capacity
    - Minimum: ASG never goes below this number.
    - Desired: Number of instances ASG tries to maintain.
    - Maximum: Upper limit of instances.
3. Health Monitoring
    - If an EC2 instance becomes unhealthy:
    ```
    EC2 crashes
      │
      ▼
    ASG terminates it
      │
      ▼
    Launches a new EC2 automatically
    ```

**Integration with Target Group**

When ASG launches a new EC2:
```
ASG
 │
 ▼
Creates EC2
 │
 ▼
Registers with Target Group
```
When EC2 is terminated:
```
ASG
 │
 ▼
Removes EC2
 │
 ▼
Removed from Target Group
```

#### Bastion Host

A Bastion Host is a **public EC2 instance** used as a secure **entry point** to access EC2 instances located in **private subnets**.

Why is it needed?

Private EC2 instances:

- Have no public IP.
- Cannot be accessed directly from the internet.

Instead, connect through a Bastion Host.

Architectuire
```
Internet
    │
    ▼
Bastion Host
(Public Subnet)
    │
SSH
    ▼
Private EC2
```

Benefits
- Only one EC2 is exposed to the internet.
- Private EC2 instances remain inaccessible from the internet.
- Improves security.

We already have a Load Balancer which takes care of the requests received from users, then why we need Bastion hosts?

- A Load Balancer is for **application traffic.**

- A Bastion Host is for **administrative access.**

Now imagine you need to:

- Check logs
- Restart a service
- Debug an issue
- Install software

You need SSH access.

The Load Balancer cannot help with this.

A Load Balancer only forwards application traffic.

It does not let you SSH into an EC2 instance.


---

### Route 53

Route 53 in AWS provides DNS as a Service.
Why is it called Route 53?
- Route → Routes users to the correct application or server.
- 53 → DNS uses port 53 (UDP and TCP).

What can Route 53 do?
1. Domain Registration
    - You can buy domains directly from AWS

2. DNS Hosting
    - Even if you bought the domain elsewhere (GoDaddy, Namecheap, etc.), you can host its DNS records in Route 53.
3. Health Checks
    - Route 53 can continuously check whether an endpoint is healthy.

#### Hosted Zone

A Hosted Zone is a container for all DNS records of a domain.
Example:
example.com

```
Hosted Zone (for example.com)
├── A Record
├── AAAA Record
├── CNAME
├── MX
├── TXT
└── NS
```

Common DNS Record Types

1. **A Record:** Maps a domain to an IPv4 address.
    ```
      example.com
            │
            ▼
      54.23.11.8
    ```
2. **AAAA Record:** Maps a domain to an IPv6 address.
3. **CNAME:** Makes one domain point to another domain.
    ```
      blog.example.com
          │
          ▼
      myblog.wordpress.com
    ```
4. **TXT Record:**

    Stores text data, commonly used for Domain verification.


#### Name Server

An NS record tells the internet:
>"Which DNS server is responsible for answering questions about this domain?"
Think of it like this:
- Domain name = example.com
- NS record = "Go ask these servers for information about example.com.

Example:
Suppose you own: `example.com`

Its NS records might be:
```
example.com
NS ns-123.awsdns-45.com
NS ns-456.awsdns-78.net
NS ns-789.awsdns-90.org
NS ns-321.awsdns-12.co.uk
```

These are AWS Route 53 name servers.

When someone types `example.com`, the DNS resolver eventually learns:
>"The DNS records for example.com are managed by these four AWS name servers."
It then asks one of those servers:
>"What's the IP address for example.com?"

The Route 53 name server replies with the appropriate DNS record (such as an A record or Alias record).

So flow is roughly like this:

```
Browser
   │
   ▼
DNS Resolver
   │
   ▼
Root DNS Server
   │
   ▼
".com" TLD Server
   │
   │ "Who manages example.com?"
   ▼
Returns NS records:
ns-123.awsdns-45.com
ns-456.awsdns-78.net
   │
   ▼
Route 53 Name Server
   │
   │ "What's the IP for www.example.com?"
   ▼
Returns:
54.23.10.5
```

