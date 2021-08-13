# AWS-SAA-C02 Notes

My notes while studying for the AWS SAA-C02 exam using the course at <https://learn.cantrill.io> by Adrian Cantrill.

**The official AWS certification site:** <https://aws.amazon.com/certification/certified-solutions-architect-associate/>

**The official AWS SAA-C02 Exam Guide:** <https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf>

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
- Breaking 1 subnet into two smaller subnets is typical. Using odd numbers is not as common but will provide you with 3 smaller subnets.

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

TLS 
1. Cipher Suites - set of protocols used by TLS 
    - e.g key exchange algorithm, bulk encryption algorithm, and message authentication code (MAC) algorithm
     