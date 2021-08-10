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
- When a device sends data through the network, it encapsulates the packet containing data into a frame. Each hop removes 
the frame to inspect the packet destination IP and MAC address, then reencapsulates the packet in a new frame and sends 
it on it's way to the next hop, using ARP to find the next MAC address along the way.