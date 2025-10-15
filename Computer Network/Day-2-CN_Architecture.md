# Computer Network Architecture - Day 2

## Overview

Computer Network Architecture defines how computers and devices are organized and communicate within a network. This documentation covers the two primary architectural models used in modern computing systems.

---

## Types of Computer Network Architecture

Computer Networks fall under these broad categories:

### 1. Client-Server Architecture
Client-Server Architecture is a distributed model where nodes are classified as either servers or clients. The server node manages and controls the behavior of client nodes.

### 2. Peer-to-Peer (P2P) Architecture
In P2P Architecture, there is no concept of a central server. Each device operates independently and can function as either a client or server.

---

## Client-Server Model

**Last Updated:** 27 Aug, 2025

The Client-Server Model is a distributed architecture where clients request services and servers provide them. It underpins many modern systems, including websites, email, and cloud storage platforms.

### How Does the Client-Server Model Work?

#### Client
A client is any device or software that initiates communication by requesting data or services from a server.

**Common client applications include:**
- Web browsers (e.g., Chrome, Firefox)
- Email apps (e.g., Gmail, Outlook)
- Mobile applications

#### Server
A server is a powerful system that listens for and responds to client requests by delivering data or performing tasks. Servers often handle multiple simultaneous client requests.

**Common server applications include:**
- Web Servers (e.g., Apache, Nginx)
- Email Servers (e.g., Exchange, Postfix)
- Database servers (e.g., MySQL, PostgreSQL)

### How the Browser Interacts With Servers

The process of interacting with servers through a browser involves several steps:

1. **User Enters the URL (Uniform Resource Locator)**
   - The user types a website address (e.g., www.example.com) into the browser's address bar

2. **DNS (Domain Name System) Lookup**
   - The browser contacts a DNS server to convert the domain into an IP address

3. **Establishing a Connection**
   - The browser sends an HTTP/HTTPS request to the server using the resolved IP address

4. **Server Responds**
   - The server sends back website files (HTML, CSS, JavaScript, images)

5. **Browser Renders the Webpage**
   - **DOM interpreter:** Processes HTML to structure the page
   - **CSS interpreter:** Applies styles
   - **JavaScript Engine:** Adds interactivity (using JIT compilation for performance)

### Real-World Examples of the Client-Server Model

| Use Case | Client | Server | Function |
|----------|--------|--------|----------|
| Email | Gmail, Outlook | Gmail/Yahoo Mail Servers | Send/receive email messages |
| Web Browsing | Chrome, Firefox | Apache, Nginx | Access and display websites |
| Cloud Storage | PC, Mobile App | Google Drive, Dropbox Servers | Upload/download and sync files |

### Advantages of the Client-Server Model

- **Centralized Data Management:** Easy to maintain and back up data
- **Cost Efficiency:** Clients require less processing power
- **Scalability:** Servers and clients can scale independently
- **Security:** Centralized security policies and authentication
- **Data Recovery:** Easier backup and restore from a single source

### Disadvantages of Client-Server Model

- **Client Vulnerability:** Risk of malware if servers distribute unsafe files
- **Server as a Target:** Susceptible to DDoS (Denial of Service) attacks
- **Data Spoofing:** Unprotected data can be tampered with in transit
- **MITM Attacks:** Unsecured connections can be intercepted by attackers

---

## Peer-to-Peer (P2P) Architecture

**Last Updated:** 12 Jul, 2025

### What Is a Peer-to-Peer (P2P) Service?

A peer-to-peer network is a decentralized network structure where each participant (node) acts as both a client and a server. Each computer functions as a node for file sharing within the network, eliminating the need for a central server. This architecture enables the sharing of large amounts of data with workloads equally distributed among nodes.

### History of P2P Networks

- **1979:** USENET enabled users to read and post messages without a central server
- **1980s:** First use of P2P networks after the introduction of personal computers
- **August 1988:** Internet Relay Chat (IRC) became the first P2P network for text and chat sharing
- **June 1999:** Napster developed as a file-sharing P2P software for audio files
- **June 2000:** Gnutella launched as the first decentralized P2P file-sharing network

### Types of P2P Networks

#### 1. Unstructured P2P Networks
- Each device makes an equal contribution
- Easy to build with random device connections
- Difficult to find content due to lack of structure
- **Examples:** Napster, Gnutella

#### 2. Structured P2P Networks
- Uses software to create a virtual layer organizing nodes in a specific structure
- More complex to set up but provides easier content access
- **Examples:** P-Grid, Kademlia

#### 3. Hybrid P2P Networks
- Combines features of both P2P networks and client-server architecture
- Uses central server for node discovery while maintaining P2P data transfer

### Features of P2P Network

- Typically involves fewer than 12 nodes
- All computers store their own data but make it accessible to the group
- Uses and provides resources simultaneously
- Requires specialized software
- Nodes act as both clients and servers
- Supported by almost all modern operating systems
- Constant threat of security attacks due to distributed nature

### P2P Network Architecture

- Computers connect in a workgroup to share files, internet access, and printers
- Each computer has equal responsibilities and capabilities
- Useful in residential areas, small offices, or small companies
- Each device serves as an independent workstation storing data on its hard drive
- Typically composed of workgroups with 12 or more computers

### How Does P2P Network Work?

**Example: File Download Process**

1. User installs P2P software (if not already installed)
2. Software creates a virtual network of application users
3. User initiates file download
4. File is received in bits from multiple computers in the network that have the file
5. User's computer simultaneously sends data to other computers requesting files
6. File transfer load is distributed among peer computers

### How to Use a P2P Network Efficiently

**Security Best Practices:**

1. **Share and Download Legal Files:** Verify files before sharing with others
2. **Design Sharing Strategy:** Create a strategy that suits your architecture for managing applications and data
3. **Keep Security Practices Up-to-Date:** Monitor cyber security threats and invest in quality security software
4. **Scan All Downloads:** Continuously check and scan files for viruses before downloading
5. **Proper Shutdown:** Correctly shut down P2P software after use to prevent unauthorized access

### Applications of P2P Network

- **File Sharing:** Cost-efficient method for business file sharing without intermediate servers
- **Blockchain:** Maintains complete replica of records ensuring data accuracy and security
- **Direct Messaging:** Provides secure, quick communication using encryption
- **Collaboration:** Easy file sharing builds collaboration among network peers
- **Content Distribution:** Serving capacity increases as more users access content
- **IP Telephony:** VoIP applications like Skype utilize P2P architecture

### Examples of P2P Networks

**Network Levels:**
- **Basic Level:** USB connections between two systems
- **Intermediate Level:** Copper wire connections for multiple systems
- **Advanced Level:** Software-based protocols managing numerous devices across the internet

**Popular P2P Networks:** Gnutella, BitTorrent, eDonkey, Kazaa, Napster, Skype

### Advantages of P2P Network

- **Easy to Maintain:** Each node operates independently
- **Less Costly:** No need for expensive central server
- **No Network Manager:** Each node manages its own resources
- **Adding Nodes is Easy:** Simple to add, delete, or repair nodes
- **Less Network Traffic:** Reduced traffic compared to client/server networks

### Disadvantages of P2P Network

- **Data is Vulnerable:** No backup due to lack of central server
- **Less Secure:** Difficult to secure the complete network
- **Slow Performance:** Computers accessed by other nodes can slow down performance
- **Files Hard to Locate:** Decentralized file storage makes files difficult to find

---

## Comparison: Client-Server vs P2P

| Aspect | Client-Server | Peer-to-Peer |
|--------|---------------|--------------|
| Central Authority | Required | Not Required |
| Scalability | High (server-side) | Moderate |
| Security | Centralized & Stronger | Distributed & Weaker |
| Cost | Higher (server costs) | Lower (no dedicated server) |
| Data Management | Centralized | Distributed |
| Performance | Dependent on server | Distributed load |
| Maintenance | Requires IT staff | Self-managed |

---

## Conclusion

Both Client-Server and Peer-to-Peer architectures serve distinct purposes in modern networking:

- **Client-Server** is ideal for centralized control, security, and scalability in enterprise environments
- **P2P** excels in decentralized scenarios requiring cost efficiency and distributed resource sharing

Understanding these architectures is fundamental to designing efficient, secure, and scalable network systems.

---

**Source:** geeksforgeeks.org  
