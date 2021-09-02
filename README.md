# AWS-SAA-C02 Notes

My notes while studying for the AWS SAA-C02 exam using the course at <https://learn.cantrill.io> by Adrian Cantrill.

**The official AWS certification site:** 
<https://aws.amazon.com/certification/certified-solutions-architect-associate/>

**The official AWS SAA-C02 Exam Guide:** 
<https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf>

## Course Fundamentals and AWS Accounts
- Root accounts have full control and can access billing details.
- IAM and the AWS account have full trust between each other which gives IAM all the permissions of the AWS account, 
with restrictions around things like billing controls and account closure.
- IAM users are created with limited permissions respective for each role. 
    - Least privilege is the goal with IAM accounts.

- 3 types of IAM accounts: 
1. Users
2. Groups
3. Roles - used by AWS Services, or for granting external access to your account
- IAM Policies: Allow or Deny access to AWS services ONLY when attached to users, groups, or roles. 

- 3 main jobs of IAM: 
1. Identity Provider - it lets you manage identities.
2. Authentication - proof of identity
3. Authorization - allow or deny access to resources 
- Use IAM accounts whenever possible. Rule of least privilege should apply. Always utilize multi-factor authentication (MFA), 
especially for high level accounts. 

#### IAM Access Keys:
- IAM Users are the only accounts that use access keys. IAM Roles do not use access keys.
- Access keys are used to sign requests made to AWS API's so that AWS can validate the identity of the sender.
- You can only have 2 sets of access keys per identity. Active or inactive, it doesn't matter. You'll generally rotate between 
keys, using one at a time. 

#### AWS CLI
- The CLI allows you to configure profiles. 
- To configure the Default AWS profile, type: `aws configure`
- To configure Named profiles, type:   `aws configure --profile NAME_OF_PROFILE` e.g. `aws configure --profile iamadmin-general`

## Cloud Computing Fundamentals
#### What is cloud computing? 
Taken from NIST Special Publication 800-145.
1. On-Demand Self-Service - Able to provision service capabilities without human interaction. "Provision and Terminate 
using a UI/CLI 
without human interaction." (acantrill)
2. Broad Network Access - Service capabilities are available over the network through typical connection methods. "Access 
services over any networks, on any devices, using standard protocols and connections." (acantrill)
3. Resource Pooling - Service capabilites of the provider are shared amongst collections of their customers. Limited control 
and knowledge of resource locations. "Economies of scale. Cheaper service." (acantrill)
4. Rapid Elasticity - Service capabilities allow resources to rapidly scale up or down with demand and appear to be unlimited.
"Scale UP and DOWN automatically in response to system load." (acantrill)
5. Measured Service - Service can be monitored, controlled, and reported on for billing. "Usage is measured. Pay for what 
you consume." 
(acantrill)

In the SAA-C02 exam, questions are often less about the product in an answer and more about how the architecture, the 
answer uses, makes sense.

#### Public vs Private vs Multi vs Hybrid
- Public Cloud
    - Using 1 public cloud.
- Multi-cloud uses more than one cloud provider. e.g. AWS + Azure + Google Cloud
    - Using more than 1 public cloud.
    - 3rd party tools, offering a "single pane management panel", typically will be limited in their feature set because 
    they're restricted to compatibility of offerings between the various cloud computing providers.  
    - Preferred architecture for multi-cloud environments is to avoid full mirroring of everything. Instead, separate portions 
    of your product or service and distribute them across your chosen cloud computing providers. You're doing this with HA 
    (high availability) in mind. 
- Private Cloud brings the 5 characteristics is cloud computing as an on-premise solution. 
    - Using on-prem **real** cloud.
    - This is different than most offerings from VMware, Citrix, etc. Those solutions may have some cloud-like features, but 
    usually are missing one or more of the 5 characteristics. 
    - AWS Outposts, Azure stack, and Google Anthos are examples of private clouds. 
- Hybrid Cloud combines a private cloud and public cloud as a single environment. 
    - Using a combination of public and private clouds.
    - e.g. A company uses AWS for most of their services but depends on AWS Outposts for a specific portion of their product 
    or service.
    - The term can be confused with using public cloud in combination with on-prem infrastructure. These are not the same 
    thing.
        - acantrill will use the terms Hybrid Environment and/or Hybrid Networking when referring to public clouds that are
        connected to local, typically private, on-prem infrastructure. 
    -Hybrid Cloud is **not** connecting a public cloud to a legacy on-prem environment. 

#### Cloud Service Models
e.g. X as a Service
- On-premises: Owner pays for everything, including equipment, facilities, and IT support teams.
- DC Hosted: Renting rack space in a facility to place your own equipment. Facility pays for electricity, security, HVAC, etc.
- IaaS (Infrastructure as a Service): Paying to rent a VM (virtual machine) in a data center. Typically charged by the 
second, minute, or hour of usage.
    - Some restrictions of the resources and capabilities by the provider. 
    - EC2 uses the IaaS service model.   
    - Unit of consumption is the O/S layer.
- PaaS (Platform as a Service): 
    - Aimed more at developers who just want to run their application. Not worry about infrastructure. 
    - Unit of consumption is the Runtime environment.
    - e.g. Heroku
- SaaS (Software as a Service):
    - Unit of consumption is the Application layer.
    - e.g. Netflix, Dropbox, Gmail, etc. 

## Tech Fundamentals
#### YAML101 - YAML* ain't markup language (* it's recursive)
- YAML: human readable data serialization language
    - language designed to make data and configurations more easily readable
    - unordered collection of key:value pairs, where each key has an associated value
    - YAML supports strings, boolean, numbers, floating point numbers, and null
    - YAML also supports lists (arrays) - an ordered set of values
        - comma separated set of values inside square brackets or in a vertical list with each value placed after a hyphen
        - values can be placed in `" "`, ` ' '`, or without either, however, using quotation marks improves accuracy. 
        - for vertical lists, indentation before the hyphen matters. All indentation must be the same. 
    - lists can be made up of dictionaries, each having keys and key:value pairs

#### JSON101 - Javascript Object Notation
- Where YAML is generally only used for CloudFormation. JSON is used for CloudFormation and other things such as policy 
documents in AWS for permissions.
- JSON doesn't care about spacing, positioning, and indentation because everything is enclosed in something, e.g. braces, 
brackets, or quotations
    - because of this, it appears less readable
- Objects in JSON are unordered collections of key:value pairs enclosed in `{ }`
- Lists in JSON are ordered arrays separated by commas and enclosed in `[ ]`
- Values can be strings, objects, numbers, arrays, boolean, and null
- JSON objects can contain lists of other JSON objects

> Try to insert image of the OSI model here.

#### Network Starter Pack - 1 - Physical
- defines the transmission and reception of raw data streams between a device and a shared physical medium. It defines 
things like voltage levels, timing, rates, distances, modulation, and connectors.
- physical medium is copper, fiber, WiFi
- voltage fluctuations determine binary output to be shared between devices
    - e.g. Binary 1 = +1 volt, Binary 0 = -1 volt
- HUBS
    - Anything received on one port is sent to **all** other ports, including errors and collisions.
    - Transmissions cannot be directed to a specific device. It's broadcasted to, and processed by, every device on the 
    network.
    - If multiple devices transmit at once, a collision occurs, rendering all the data useless. 
    - L1 has no media access control and no collision detection

#### Network Starter Pack - 2 - Data Link - Part 1
- Frames are a format for sending information over a layer 2 network. 
    - Frames can be addressed to a destination or broadcast (ALL F's)
    - Preamble (56 bits, SFD is 8 bits), Destination MAC, Source MAC, EtherType (16 bits), Payload (46-1500 bytes), 
    Frame Check Sequence (FCS, 32 bits)
    > Try to insert image of the breakdown of a Data Link frame
- Devices at L2 have a unique MAC address. 48 bits, in hex. 24 bits for the manufacturer (O.U.I.) and 24 bits for the 
specific NIC hardware.

#### Network Starter Pack - 2 - Data Link - Part 2
- Device 1, using L2, checks L1 for carrier presence before attempting to send a frame to device 2, by MAC address. If 
carrier is present, meaning another device is already transmitting on the L1 physical medium, device 1 waits for the 
transmission to complete. 
If no carrier is present, device 1 begins transmission. 
- The data being transmitted is encapsulated inside a frame. 
    - As a data packet is being built, working it's way down through the OSI model from L7 to L1, each layer adds it's portion
    to the encapsulated frame. The device receiving the transmission must de-encapsulate the frame to process each segment. 
- Collisions can still happen with L2. 
    - If a collision occurs, both devices stop and wait for a random amount of time before trying again. This is called 
    Collision Detection.
        - CSMA/CD (carrier sense multiple access / collision detection)
    - On HUBs, one collision affects all devices on the network.
- Switches store and forward frames to the device with the specific destination MAC address in the frame.
    - Switches keep a table of device MAC addresses to assoicate with each switch port.
    - Collisions are contained to the port. They are not forwarded to other devices. Every other device does not have to 
    process them.
    
#### Decimal to Binary Conversion (IP Addressing) 
- 32 bits 
- 4 x 8 bits
- 4 x Bytes
- 4 x Octets
- Make a table with 3 rows:
	- Row 1: each column is numbered 1 through 8 representing each bit in the octet
	- Row 2: contains the values for each bit in each bit from left to right. 128, 64, 32, 16, 8, 4, 2, 1
	- Row 3: left blank for you to write either 1 or 0 as you're adding up (or subtracting from) the octect value
- Method to use for Decimal to Binary: 
	- Move through the binary table, left to right. 
	1. Compare decimal number to Binary position value, if smaller write 0 => move on to next table position, go to #1
	2. IF IT IS EQUAL OR LARGER - Minus the binary position value from your decimal number, add 1 in the binary value column
	3. Move on to next position, go to #1 (with the new decimal value)

#### Network Starter Pack - 3 - Part 1
Layer 3 - Network
- Layer 3 connects layer 2 networks together and moves data between them. 
	- At one location with directly cabled/wired connections, ethernet is typically the best protocol to use. Across 
	geographically spaced layer 2 networks that ARE directly connected by fiber, cable, wireless bridge, etc., layer 3 
	will typically use suitable protocols such as PPP, MPLS, or ATM. 
	- When data needs to traverse geographically spaced layer 2 networks that are NOT directly connected by fiber or cable, 
	layer 3 will typically use another protocol such as Internet Protocol (IP)
	- Routers will encapsulate a packet inside an ethernet frame for it's journey over a local network. When that packet 
	needs to move over a different network, on it's way to its final destination, the router will remove the packet from 
	that frame and re-encapsulate it in a new frame for the next network.
- IPv6 has bigger, than IPv4, Source and Destination IP address fields for larger addresses.
- TTL in IPv4 is called Hop Limit in IPv6.
- L3 does not provide any method for channels of communications for the receiving device to distinguish which application 
on the operating system the packet data belongs. Source IP to Destination IP only. 
- L3 has no flow control. 

#### Network Starter Pack - 3 - Part 2
IPv4 - IP Addressing
- 133.33.3.7 is called Dotted-decimal Notation; 4 x 0-255
- First 2 octets are the "network" part and the last 2 octets are the "host" part
- An IPv4 address is made up of 32 bits (4 bytes) - 4 x 8 bits (Octets)

Subnet masks
- 255.255.0.0 is the same as /16 prefix
- Binary for 255 is 11111111
- Therefore, 255.255.0.0 in binary is 11111111.11111111.00000000.00000000
	- 255.255.255.255 in binary would be 11111111.11111111.11111111.11111111

Routing tables and Routes
- /24 subnet mask means the first 24 bits of the IP address are for the network and the last 8 bits are for the host
- Border Gateway Protocol (BGP) allows routers to communicate with each other to exchange information containing which 
networks they know about.
- A packet traveling across networks is directed to the proper NETWORK based on information in the routing table. It 
gets directed to the proper HOST based on the MAC address listed in the packet by using Address Resolution Protocol (ARP).

#### Network Starter Pack - 3 - Part 3
Address Resolution Protocol (ARP)
- ARP finds the MAC address for any given IP address.
- ARP is used when you have a L3 packet that you want to encapsulate in a frame and send to a MAC address. You don't know 
the MAC address and you need a protocol that can find the MAC address for an IP address. 
- ARP broadcasts on layer 2. It ask's all devices on the network, "Who has IP address xxx.xxx.xxx.xxx?"

IP Routing
- When a device sends data through the network, it encapsulates the packet containing data into a frame on layer 2. Each hop removes 
the frame to inspect the packet destination IP and MAC address, then reencapsulates the packet in a new frame and sends 
it on it's way to the next hop, using ARP to find the next MAC address along the way.
- Packets can be delivered out of order.
    - When packets travel across the internet, they may take different routes to reach the destination. Because some routes 
    will be slower than others, some packets may take longer than others sent after them. Layer 4 assists in sorting this out. 
    
#### Network Starter Pack - 4 - Transport - Part 1
#### *and maybe layer 5
Problems with Layer 3
- L3 provides to ordering mechanism for packets.
- No communication channels. Source to Destination IP address only. No method of splitting packet data by application or 
channel.
- No flow control. If the source sends packets faster than the destination can receive and process them, saturation occurs
causing packet loss.

Layer 4 - TCP and UDP
- Transmission Control Protocol (TCP)
    - slower but reliable
    - HTTP, HTTPS, SSH, etc.
    - connection oriented protocol. Establishing a connection between two devices it creates a bi-directions channel of 
    communication.
- User Datagram Protocol (UDP)
    - faster but less reliable
- TCP and UDP, both, run on top of IP, using IP as transit.
- TCP is used for most of the important upper-layer protocols

TCP Segments
- Just another container like packets and frames but are specific to TCP.
- TCP segments are encapsulated inside IP packets. 
    - Because they're inside a packet that's already inside a frame, TCP segments don't contain source or destination IP 
    addresses but they do contain source and destination PORT info.
- Sequence number defines the order of the packets.
- Acknowledgement confirms receipt of all sequenced packets.
- Flags (9 bits) are used to control the TCP segments and various parts of the wider connection. e.g. close the connection, 
synchronize sequence numbers, etc. 
- TCP Window is the number of bytes you're willing to receive between acknowledgements. 
    - This is how flow control is implemented. It lets the receiver control the rate of data being sent by the sender.
- Checksum is used for error checking so retransmissions can be requested.
- Urgent Pointer allows coordination of the data transfer between the client and server. 
    - Useful for latency sensitive applications like telnet or FTP. 
    
#### Network Starter Pack - 4 - Transport - Part 2
- Ephemeral port is a temporary port on the client device used as a source port to send data to the server. 
    - Ephemeral ports tend to be a higher number range. e.g. 23060
- TCP Connection 3 way Handshake
    - It's important that the client and server both agree on a sequence so data transmission occurs as smoothly and reliably
    as possible.
    - The sequence number they both agree on is how they acknowledge receipt of data send from the other device.
    - The client will send an initial sequence number "CS" and the server will respond with it's own
    sequence start number "SS" plus 1. The client will acknowledge the server's starting sequence by returning
    that response with the next number in the server's sequence number.

Network ACL (AWS)
- Stateless firewalls see two transmissions: 
1. Outbound from client to server (e.g. tcp/23060 to tcp/443)
2. Response from server to client (e.g. tcp/443 to tcp/23060)
- Stateful firewalls see 1 thing
1. Outbound from client to server (e.g. tcp/23060 to tcp/443) and allowing the outbound response 
implicitly allows the inbound response. 
- This is how security rules work in AWS.
- This is technically a combination of layers 4 and 5. 

#### Network Starter Pack - EXTRA - NAT - Part 1
Network Address Translation (NAT) is the process of adjusting packets source and destination addresses to allow transit 
across different networks. The main types you will encounter are Static NAT, Dynamic NAT and Port Address Translation (PAT). 
NAT is most commonly experience in home or office networks where private IPv4 addresses are translated to a single public 
address, allowing outgoing internet access. 
- NAT designed to overcome IPv4 shortcomings. 
- Translates private IPv4 addresses to public addresses. 
- Provides some security benefits.
- Static NAT is 1 private IP to 1 public (fixed) IP address (IGW)
- Dynamic NAT is 1 private IP to 1st available public IP from a designated pool
- Port Address Translation (PAT) is many private addresses to one public address (NATGW)
    - This is the method the NAT gateway instances use in AWS
- NAT is for IPv4 only. IPv6 has plenty of addresses. 

Static NAT
- The router (NAT device) stores a NAT table, mapping private IP to public IP (1:1)
- As packets flow out from internal devices with private IP addresses through the device performing the NAT, the source 
IP of the packets are translated to a public IP address of the NAT device's network. This is done so that the receiving 
server across the internet has a valid public IP address of the source network and knows where to return it's response. 
- This is how the AWS Internet Gateway works.

Dynamic NAT
- Similar to Static NAT, except devices are not allocated a permanent public IP address. Instead, they're allocated a 
temporary public IP address from a pool of public IP addresses. 
- Dynamic NAT is used when you have fewer public IP addresses available than you have private devices and all private devices 
require bi-directional communication.     
- Public IP addresses can be shared by multiple private devices only if there's no overlap, meaning the private devices are 
communicating with different public internet servers. 
- If the public IP address pool you own is exhausted, other private devices on your network will fail to access the internet. 

Port Address Translation (PAT)
- Common with home routers
- In AWS, this is how the NATGateway (NATGW) functions. Many private IP's to 1 public IP (MANY:1) architecture.
- Private devices determines the source port, which must be unique among all other private network devices. 
- With PAT, you can't initiate traffic inbound to a private device because the NAT table won't know which private device 
to send the packets to. 

#### Network Starter Pack - Subnetting - Part 1
IPv4 Addressing and Subnetting
- 0.0.0.0 - 255.255.255.255 = 4,294,967,296 addresses
    - less than 1 address per person on the planet and many/most people have more than one device
- Originally managed by IANA but now managed by regional authorities
- All IPv4 addresses are "allocated". So, you can't just pick your own public IP address and expect it to work properly.
- Part of the address space is private and can be used freely for LANs.

IPv4 Address Space
- Class A
    - 0.0.0.0 - 127.255.255.255
    - 128 networks
    - 16,777,216 IPs each
    - Huge, early internet businesses and military
- Class B 
    - 128.0.0.0 - 191.255.255.255
    - 16,384 networks
    - 65,536 IPs each
- Class C
    - 192.0.0.0 - 223.255.255.255
    - 2,097, 152 networks
    - 256 IPs each (starts counting at the last octet)

Private IP addresses 
- are defined in standard RFC1918
- all are generally broken up into smaller networks
- you want to use the same one multiple times
- 10.0.0.0 - 10.255.255.255 (1 x class A network)
    - 16,777,216 IPv4 addresses
- 172.16.0.0 - 172.31.255.255 (16 x class B networks)
    - 16 x 65,536 IPv4 addresses
- 192.168.0.0 - 192.168.255.255 ( 256 x class C networks)
    - 256 x 256 IPv4 addresses

IPv4/6 Address Space
- There are 4,294,967,296 IPv4 addresses but there are 340,282,366,920,938,463,463,374,607,431,770,000,000 IPv6 addresses. 
    - 50 Octillion per living human! 79,228,163,000,000,000,000,000,000,000 IPv4 Internets! 670 Quadrillion addresses 
    per square millimeter of earth!
    
IP Subnetting
- /## is called the prefix. e.g. 10.16.0.0/16
- Classless Inter-Domain Routing (CIDR) is what allows us to take networks and break them down.
- 10.16.0.0/8 would be a bigger network. The smaller the prefix, the larger the network space.
- Breaking 1 subnet into two smaller subnets is typical. Using odd numbers is not as common but will provide you with 3 
smaller subnets.

#### Network Starter Pack - Distributed Denial of Service (DDOS)
- Attacks designed to overload a service
- Creates competition for resources amongst legitimate connections
- Distributed means they're difficult to block because the DDOS traffic isn't coming from individual IP address or ranges
- 3 Categories of attacks
    - Application layer - HTTP flood
        - Takes advantage of an inbalance of processing between the client and server. Client requests for a webpage are simple 
    and easy but the delivery of the webpage by the server is complex. DDOS attacks amplify the number of client requests
    exponentially causing the server resources to overload while trying to meet demand. 
    - Protocol attacks - SYN flood
        - takes advantage of the connection-based nature of requests by spoofing a source IP address when initiating 
        the 3 way handshake. The server tries to respond, in step 2 of the handshake, but can't because the source IP is 
        spoofed. The server sits waiting for a response, consuming network resources while it waits. 
    - Volumetric / Amplification attacks - DNS Amplification
        - DNS requests are small but responses are more complex. Typically botnet armies will generate a multitude of 
        independent requests using a spoofed IP of their target webpage or webservice so that all responses to the requests 
        are directed back to the spoofed IP of the targeted webpage. 
- AWS has products and services to help combat DDOS attacks.

#### Secure Sockets Layer (SSL) and Transport Layer Security (TLS)
- SSL and TLS provide Privacy and Data Integrity between client and server
- Privacy - communications are encrypted
    - asymmetric then symmetric
- Identity (server or client/server) verified
    - e.g. Netflix.com proving that it is indeed actually Netflix
- Reliable connection - protection against alteration
- See screenshot of process 
[insert screenshot here for SSL and TLS]

## AWS Fundamentals
#### AWS Public vs Private Services
- By default, there is no permissions access to public or private services. The terms public and private only refer 
to networking. 
    - Connectiviy vs Permissions
- AWS Public is not the same as the public internet. 
- AWS Private network is secured away from the public internet and AWS Public networks by default. A service that runs 
inside an AWS private network must explicitly be made available to the AWS Public network, where connections can be made 
to it from the public internet.  
[insert screenshot here]

#### AWS Global Infrastructure
- https://infrastructure.aws has a 3D model of regions and edge locations
- Geographic Separation - separated Fault Domain
- Geopolitical Separation - Different Governance
- Location Control - Performance

Regions and AZs
- Region codes and Region names
    - Region code: ap-southeast-2
    - Region name: Asia Pacific (Sydney)
    - Availability Zones: ap-southeast-2a, ap-southeast-2b, ap-southeast-2c
        - Isolated infrastructure inside a region (compute, storage, networking, power, and facilities)
        - Could be made up of one or more data centers
        - Connected together with high speed, resilient networking
        - Services can be spread across AZs, e.g. your VPC
- Service Resilience
    - Globally Resilient
        - e.g. IAM, Route 53. You never select a region for these services because they exist globally. 
        - Several regions, if not all regions, would need to fail to impact availability of global services.
    - Regionally Resilient
        - Services that operate in a single region with one set of data per region. 
        - e.g. RDS databases in different regions would be different
        - An entire region would need to fail to impact availability of regional services.
        - Regional services generally replicate data between it's AZs.
    - AZ Resilient
        - Even though there is some resiliency built into the components making up an AZ, AZ services are very prone to 
        failure if there are problems inside the AZ.
        
#### AWS Default Virtual Private Cloud (VPC)
- VPC Basics
    - VPC - virtual network inside AWS
    - Exists within 1 AWS account and within 1 region
    - Private and isolated unless you decide otherwise
    - 2 types: Default VPC and Custom VPC
        - You can only have 1 default VPC per region!
    - VPCs are regionally resilient
        - VPC CIDR of the default VPC is always 172.31.0.0/16
        - Subnets can be created inside a VPC for network segregation.
        - The default VPC is pretty inflexible. It can be remove and recreated, if necessary.
        - /20 subnet is created in each AZ in the region
            - plenty of network space
        - Included are an Internet Gateway (IGW), Security Group (SG) and Network Access Control List (NACL)
        - Subnets assign public IPv4 addresses
    - DEMO at 11:30
        - Recreating the default VPC is under the Action drop down menu, not "Create VPC". 
        - "Create VPC" button only creates custom VPCs

#### Elastic Compute Cloud (EC2) Basics
EC2 Key Facts and Features
- Provides access to virtual machines, known as "Instances"
- IAAS (Infrastructure as a Service) - Provides virtual machines => Instances
    - The Instance is the unit of consumption
    - An Instance is just an OS configured with a certain set of resources
    - Since EC2 is IaaS, you control the OS and up while AWS controls everything from the virtualization of the VM, the 
    hardware it's running on, and everything else required down to the needs of the building the server is located. 
- Private service by-default which means it uses VPC networking
    - If you want your EC2 instance to have public access, your VPC needs to support that access. 
- AZ resilient - meaning, if the AZ fails, the instance fails      
- On-Demand billing - per second
- Local on-host storage and Elastic Block Store (EBS)

Instance Lifecycle
- 3 states: Running, Stopped, Terminated
    - you can switch from Running to Stopped and back again without problem. 
    - Terminating an instance is a permanent, one way change. 
    - Running instances are billed per second for all components of the instance, e.g. CPU, RAM, DISK, and NETWORK
        - Changing an instance to the Stopped state when not needed saves you money on CPU, RAM, and NETWORK but the storage
        allocated to the instance is still being held. *Stopped instances still incur storage charges!*
        
Amazon Machine Image (AMI)
- Image of an EC2 instance
- AMI can be use to create, or be created from, an EC2 instance.
- Similar to a USB used to install an OS, or perhaps a VM template used to launch a preconfigured version of a new VM
- AMI contains:
    - permissions - controls which accounts have access to the AMI
        - can be set to Public - everyone allowed. This is how the Windows and Linux images that AWS provides, are set to. 
        - Owner is implicitly allowed to create EC2 instances from the AMI
        - Owner explicitly allows specific AWS accounts access
    - Root volume - the drive that boots the OS. e.g. C:\ in Windows or Root volume in Linux
    - Block Device Mapping - configuration that links the volumes of the AMI to the device ID when presenting to the OS

Connecting to EC2
- EC2 instances can run various linux distros or Windows
    - use RDP:3389 to remote into Windows instances
        - When connecting to Windows, a private key is created and stays on the client device. The public key goes to the 
        EC2 instance. The private key is used to reach the EC2 instance but access is granted using the Windows local admin 
        password. The private key should be safe backed up and secured. 
    - use SSH:22 to connect to Linux instances 
        - When connecting to Linux instances, the private and public key are used to establish the SSH connection.

#### DEMO - My First EC2 Instance
- A Security Group can be applied to many network interfaces on many EC2 instances. It doesn't have to be 1 per instance
or network interface. 

#### Simple Storage Service (S3) Basics
- Global storage platform - Region based / resilient
    - Can withstand failure of an AZ
- Public service, unlimited data and multi-user
- Good for movies, audio, photos, text, and large data sets
- Economical and accessible via the UI, CLI, API, and HTTP
- Delivers objects and buckets
    - buckets are containers for objects
    
S3 Objects
- An object in S3 is made up of 2 main components, and some associated metadata. 
    - **Object Key** is similar to a filename. 
        - So if you have the object key and bucket, assuming you have the proper permissions, you can access the object.
    - **Object Value** is the content, or data, being stored. 
        - **Object size can range from 0 bytes to 5 TB**
    - Other attributes of an S3 object
        - Version ID, Metadata, Access Control, Subresources

S3 Buckets
- Buckets have primary home regions, and never leave that region, unless you configure it to leave the region. 
    - By keeping a bucket in a region, you can control the laws and rules that apply to that data. 
    - Blast radius is the region. If a major failure occurs (natural disaster or data corruption), the effects will be 
    contained within the region. 
    - **Bucket names must be globally unique.**
        - 3-63 characters, all lower case, no underscores, start with lowercase letter or number, can't be formatted like 
        an IP address (e.g. 1.1.1.1)
    - Buckets can hold an unlimited number of objects.   
    - Bucket structure is flat. All objects are stored at the same level. There is no nesting. 
        - There are no **real** folders in S3. S3 will present objects as being contained in folders when the object name 
        includes a prefix, e.g. /old/koala1.jpg, but in reality, no folder actually exists. 
    - Bucket limits per account: 
        - **100 soft limit** 
        - **1000 hard limit** - making limit increase requests through support along the way 
    
S3 Patterns and Anti-patterns
- S3 is an **object** store, not *file* or *block*
    - Not good for Windows server using S3 as a network file storage location. That should be  *file* storage.
    - You can't mount an S3 bucket. e.g. K:/ or /images
    - Great for large scale data storage, distribution, or upload
    - Great for offloading as it's cheaper to get old content off storage of more expensive server instances. e.g. old 
    blog posts from your website and point your users to the S3 bucket for that content.
    - Should be your default storage location for INPUT and/or OUTPUT to MANY AWS services. 
    
#### DEMO - My First S3 Bucket
- Untick checkbox for "Block *all* public access" only means that you're removing the safety mechaism that allows you to 
take further steps to make objects in your bucket publicly accessible. 
- Amazon Resource Name (ARN)
    - Every resource in AWS has a unique ARN. 
    - Format is always the same: arn:PARTITION:SERVICE NAME:SERVICES:SERVICES:RESOURCE NAME
- Clicking the link under Object URL attempts to view the object as an unauthenticated user which is blocked by default. 
To view the object with your own AWS permissions, select Open from the Object Actions menu. 

#### CloudFormation (CFN) Basics
- Uses templates to create, update, and delete AWS infrastructure in a consistent and repeatable way.
    - Templates are in JSON or YAML. Both do the same thing. Both have their own pros and cons. 
- Resources section of the  template is the only *mandatory* part of a template. 
- If you use "AWSTemplateFormatVersion" and "Description", the Description must immediately follow the Template Format Version. 
- "Metadata" is a way to force how the AWS Console UI presents the template. 
- "Parameters" contains fields that prompt the user for more information. 
- "Mappings" is optional but allows you to create lookup tables. 
- "Conditions" allow decision making in the template. 
- "Outputs" can be presented based on what's being created, updated, or deleted. 
    - e.g. Returning the Instance ID of something that's been created. 
    
- "Resources" inside a CloudFormation template are called "Logical Resources".
- A "stack" is a living representation of a template. It's created from all the "Instances" inside a template. 
- For every Logical Resource in a Stack, AWS makes a corresponding physical resource in your AWS account.
    - This is the *whole point* of CloudFormation. 
    
#### DEMO - Simple Automation with CFN
- Create a stack and EC2 instance from a YAML template.
- Connect to the EC2 instance using Session Manager. 
    - Session Manager role was already active because it was included in the YAML template.
    - You can type "bash" at the prompt once connected in Session Manager.  
- Monitor progress of the stack creation and deletion from CloudFormation.

#### CloudWatch (CW) Basics
- Collects and manages operational data, e.g. how the resource it's running, performing, or logging data
- 3 main jobs:
    1. Metrics - AWS products, apps, on-premise collection of metrics, monitoring of metrics, and actions of metrics. 
    e.g. cpu utilization, disk space consumption, etc.
        - some metrics require the CloudWatch agent e.g. metrics outside AWS, processes running inside an EC2 instance
    2. CloudWatch Logs - AWS products, app, on-premise
    3. CloudWatch Events - AWS Services and Schedules
- Namespace - like a container for monitoring data. Keeps data organized.
    - required to have a name but AWS/[service] is reserved. e.g. AWS/EC2 is reserved for EC2 data.
- Metric - Time Ordered set of data points. e.g. cpu utilization, network in/out, disk IO, etc. 
- Datapoint - unit of data inside a Metric. 
    - For example, several EC2 instances report their cpu usage to the same CPU Usage metric. Inside that metric are many 
    datapoints of cpu usage data from each of the instances. 
    - Datapoints consist of 2 things: Timestamps and Values. 
        - Timestamps contain the exact date and time of the value when the datapoint was created.
        - Value contains the measurement being taken. e.g. % of CPU used. 
    - Dimensions are how we determine which instance the datapoint came from or is referring to. They separate datapoints 
    for different perspectives within the same metric.
        - InstanceID and InstanceType are also included. 
- Alarms - takes an action based on a metric. Tied to a specific metric. 

#### DEMO - Simple Monitoring with CloudWatch
- Created a CPU usage alarm and test the alarm state change using "stress" application on the EC2 instance.

####  Shared Responsibility Model
[Insert screenshot here]
- Keep shared responsibility model in mind through the remainder of the course.
- Good info to understand at the beginning of learning AWS.

#### High-Availability vs Fault Tolerance vs Disaster Recovery
High-Availability (HA)
- Minimize any outages
- aims to ensure an agreed level of operational performance, usually uptime, for a higher than normal period.
- HA is not about user experience or preventing user disruption. It's not a guarantee that customers won't experience outages.
 **It's about maximizing a system's uptime.** HA is about keeping a system operational. It's about fast, or automatic, 
 recovery of issues. 
- 99.9% (Three 9's) = 8.77 hours/year 
- 99.999%(Five 9's) = 5.26 minutes/year
- e.g. 4 wheel drive on a vehicle traveling through muddy terrain provides HA

Fault Tolerance (FT)
- Operate through faults
- the property that enables a system to continue operating properly in the event of the failure of some (one or more faults 
within) of it components. 
- e.g. airplanes must continue operating through system failure so they're built with more engines than they need and duplicate
systems. 
- FT is more difficult to design, implement and costs much more. 

Disaster Recovery (DR)
- Used when HA and FT don't work.
- a set of policies, tools, and procedures to enable the recovery or continuation of vital technology infrastructure and
systems following a natural or human-induced disaster. 
- DR is designed to keep the crucial and non-replaceable parts of your system safe so that when a disaster occurs, you don't 
lose anything irreplaceable and can rebuild after the disaster. 
- e.g. pilot or passenger ejection systems
- DR was previously a very manual process but the Cloud and automation has reduced recovery time and potential for errors.

#### Domain Name System (DNS) Fundamentals - Part 1
- DNS is a discovery service
- Translates machine into human and vice-versa
- It's huge and has to be distributed
- The root of a domain is the invisible "." that comes after the TLD. e.g. .com., .org., .co.uk., etc.  
- Zone file for a website exists in one place on the internet and has within it a DNS record that points the public IP of 
the server hosting the website. The DNS records is hosted on a DNS Nameserver (NS).
- DNS Client: your laptop, phone, tablet, PC
- Resolver: software on your device, or a server which queries DNS on your behalf
- Zone: part of the DNS database (e.g. amazon.com, netflix.com)
- Zonefile: physical database for a zone
- Nameserver: where zonefiles are hosted
- DNS Root Servers (13 of them) are operated by 12 different large global companies or organizations manage the root zone 
servers but not the database.
    - list of root servers can be found on iana.org
        - Verisign manages 2 of the servers
    - each manager is assigned a letter, A-M  
    - each server name is actually a pool of servers
    - These root servers are the entry point. Understand how your laptop finds these root servers. *It's the root hints file 
    provided by your computer's O.S.!*
- Your laptop generally will use a DNS resolver server typically from your internet router or your ISP. The resolver for 
a Windows device might be Microsoft or for Linux it might your OS' maintaining group. They supply the root hints, or root 
hints file. The root hints file is essentially just a list of the root servers. The contents of the root zone are managed
 by IANA, essentially placing them in charge of the global DNS system. DNS is built on trust.  
- IANA (Internet Assigned Numbers Authority) : https://www.iana.org
  
  Root hints : https://www.internic.net/domain/named.root
  
  Root Servers : https://www.iana.org/domains/root/servers
  
  Root Zone Database : https://www.iana.org/domains/root/db
  
  Root Zone File : https://www.internic.net/domain/root.zone
  
  Delegation Record for .com : https://www.iana.org/domains/root/db/com.html 
  
  #### Domain Name System (DNS) Fundamentals - Part 2
  DNS Hierarchy
  - "authority" or "authoritative" indicates Trust
  - the Root Zone can delegate part of itself to another entity, or another zone. That someone else becomes *authoritative* 
  just for that part that's been delegated. 
  
  DNS Flow - client to server
  1. A client makes a dns request asking for a specific URL, e.g. amazon.com
  2. The root zone database looks at the .com and points the request to Verisign's nameserver because Verisign is responsible 
  for the .com TLD. The .com zone is now authoritative because it's been delegated by the Root Zone.
  3. The amazon zone, for amazon.com, in the .com TLD nameserver has a DNS record pointing to the amazon.com zone that is 
  managed by Amazon, the organization that owns the amazon.com domain name. This amazon.com zone is trust by the .com zone 
  which is trusted by the Root Zone. It contains a DNS record for www. which maps to an IP address 104.98.34.131, which is 
  what your client needs to communicate with the www.amazon.com server.
  
  DNS Resolution
  "Walking the tree" [insert screenshot] 
  It's called a recursive resolver because it handles the process for you. 
  
  Remember these!
  - Root hints - config points at the root server's IPs and addresses
  - Root Server - hosts the DNS root zone
  - Root Zone - points at the TLD authoritative servers
  - gTLD - generic top level domain (.com, .org)
  - ccTLD - country-code top level domain (.uk, .eu, etc.)
  
  #### Route53 (R53) Fundamentals
  1. Registers domains 
  2. Hosts zones .. managed nameservers 
  - Global service ... single database. Don't need to pick a region in the UI. 
  - Global resilience
  
  Hosted Zones
  - zone files in AWS
  - hosted on 4 managed nameservers 
  - can be public
  - or private .. linked to VPC(s)
  - stores records (recordsets)
  
  #### DEMO - Registering a domain
  - PIR controls getting your newly registered domain updated on the TLD database
  
  
 #### DNS Record Types
 Nameservers (NS) provide the route to each zone along the way until the proper info is collected to send back to the 
 requesting client.
 
 A and AAAA Records
 - these records map host names to IP addresses
    - A records map to an IPv4 address
    - AAAA records map to an IPv6 address
    - Create each when building so the requesting client can decide which one it wants to use, IPv6 if capable.
    
CNAME Records
- Host to Host records
- CNAME records **can not** point to an IP address, only host names

MX Records 
- MX records are how a server can find the mail server (SMTP) for a specific domain.
- MX records have 2 main parts: Priority and Value
- Values: 
    - MX 10 mail (host)
        - because it doesn't have a trailing ".", it's assumed to be a part of the same zone it's in.
    - MX 20 mail.other.domain.  
        - the trailing "." means it's a fully qualified domain name (FQDN)
- Priority: 
    - the lower value is tried first. If it doesn't work, move on to the next highest value and try again. 
    - if both records have the same value, e.g. both have 10, either can be used.
    
TXT Records
- allow you to add arbitrary text to a domain
    - one common use is to prove domain ownership
    
TTL - Time to Live
- numeric value, in seconds, that can be set on DNS records
- responses provided by the domain zone, e.g. amazon.com zone, are authoritative. They are accepted, and cached by the 
resolver servers. Responses provided by resolver servers are non-authoritative.
- TTL can be set low but this increases the number of requests on your DNS servers. If you change the values of records 
in your domain zone, delays may occur for requests getting responses from resolver servers who have not yet checked back 
with the domain zone servers because the previous TTL has not yet expired. 
    - When making planned changes to DNS records, seriously consider lowering the TTL values of those records days or weeks 
    ahead of time to reduce caching issues when you finally change the records. 