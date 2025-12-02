# Networking (TCP/IP, load balancing, etc)

### **OSI Model (7 layers)**

* **L1 – Physical:** Cables, NICs, bits.
* **L2 – Data Link:** MAC addresses, switches, VLANs, STP.
* **L3 – Network:** IP, routing, subnets, CIDR, ARP.
* **L4 – Transport:** TCP/UDP, ports, handshake, retransmission.
* **L5 – Session:** Connection management. Rare in practical ops.
* **L6 – Presentation:** Encryption, compression, TLS framing.
* **L7 – Application:** HTTP, DNS, SMTP, gRPC.

#### Key Interview Tip

* OSI layers help **troubleshoot**: “Which layer is failing?”
* Map protocols to layers (HTTP → L7, TLS → L6, IP → L3, etc).

***

### **TCP/IP Model (4 layers)**

* **Link (L1–L2):** Ethernet, ARP.
* **Internet (L3):** IP, ICMP, routing.
* **Transport (L4):** TCP/UDP.
* **Application (L5–L7):** HTTP, DNS, SSH.

#### Important Concepts

* **TCP**: reliable, handshake, congestion control.
* **UDP**: fast, no guarantee, used for DNS, streaming, VoIP.
* **ICMP**: used for ping/traceroute, path discovery.

***

### **IP Addressing**

* **CIDR:** /24 = 256 hosts, /16 = 65k hosts.
* **Subnetting:** dividing network segments for isolation/performance.
* **Private IP ranges:** 10/8, 172.16/12, 192.168/16.
* **VPC (cloud):** routed private networks; no L2 broadcast.

***

### **Routing & Switching**

#### Switching (L2)

* Forwards frames by MAC table.
* VLAN segments networks.
* STP prevents loops (root bridge, blocked ports).

#### Routing (L3)

* Routers forward packets using routing tables.
* **Static routing**: manual.
* **Dynamic routing**: BGP, OSPF.
* **NAT:** translates internal IP → public IP (SNAT, DNAT).

***

### **DNS Essentials**

* Converts names → IP.
* Record types: A, AAAA, CNAME, MX, TXT, NS, SRV.
* **TTL** controls caching.
* Common issue: stale DNS caches.
* Use `dig`, `nslookup`, `host`.

***

### **HTTP/HTTPS Essentials**

* Stateless, request/response.
* Methods: GET, POST, PUT, PATCH, DELETE.
* Status codes: 2xx OK, 3xx redirect, 4xx client error, 5xx server error.
* HTTPS adds TLS handshake + encryption.

#### TLS

* Uses asymmetric cryptography for handshake.
* Certificates: chain, CA, trust store.

***

### **Load Balancing**

#### **OSI Layers**

* **L4 LB** (Transport): TCP/UDP based. HAProxy, NLB, LVS/IPVS.
* **L7 LB** (Application): HTTP-aware. Nginx, Envoy, ALB, Traefik.

#### Key Algorithms

* Round-robin.
* Least connections.
* Weighted round-robin.
* IP-hash (sticky).
* Random with two-choices (modern default).

#### Key Features

* Health checks (HTTP/TCP).
* Session persistence.
* TLS termination/offloading.
* Rate limiting, WAF (L7 only).
* Connection draining.

***

### **Reverse & Forward Proxies**

#### Reverse Proxy

* Client → proxy → backend service.
* Used for load balancing, caching, TLS termination, auth.
* Examples: Nginx, Envoy, Traefik, HAProxy.

#### Forward Proxy

* Client → proxy → arbitrary internet.
* Used for egress control, filtering, anonymity.
* Examples: Squid, corporate proxies.

***

### **NAT & Firewall Basics**

#### NAT Types

* SNAT: internal → external.
* DNAT: external → internal.
* PAT/Masquerade: many → one IP.

#### Firewalls

* L3/L4 filtering: IP, port, protocol.
* Stateful vs stateless.
* Tools: iptables, nftables, ufw, AWS SG/NACL.

***

### **TCP Essentials**

* **3-way handshake:** SYN → SYN/ACK → ACK.
* **Flags:** SYN, ACK, FIN, RST, PSH.
* **Slow start:** congestion control.
* **Retransmissions** when packets drop.
* **Keepalive** prevents dead connections.

#### Common Issues

* SYN backlog full → connection timeouts.
* High RTT → slow performance.
* MTU mismatch → packet fragmentation/blackholes.

***

### **UDP Essentials**

* Unreliable, unordered, no retransmits.
* Used for DNS, video, VoIP, QUIC.
* No handshake → low latency.

***

### **MTU & Packet Fragmentation**

* Default Ethernet MTU: **1500 bytes**.
* Tunnel/VPN reduces MTU.
* PMTUD helps discover correct MTU.
* Wrong MTU → “can’t load some websites”, “stalled connections”.

***

### **VPN & Tunneling**

* Encapsulate traffic across networks.
* Types: IPSec, WireGuard, OpenVPN.
* Overhead reduces MTU.
* Often used for inter-VPC or DC-to-DC secure links.

***

### **CDN Basics**

* Edge caching.
* Reduces latency and origin load.
* Key concepts: cache hit ratio, TTL, purge, invalidation.

***

### **Caching**

* Local caching: DNS, HTTP, OS caches.
* Proxy caching: Nginx/Envoy/Cloudflare.
* Cache-control headers: max-age, no-cache, etag.

***

### **Service Discovery**

* DNS-based (Kubernetes).
* Consul, etcd, Eureka.
* K8s: kube-dns / CoreDNS.

***

### **Common DevOps Networking Tools**

* `ping`: reachability.
* `traceroute`, `mtr`: routing path.
* `ss`, `netstat`: sockets and ports.
* `tcpdump`, `wireshark`: packet capture.
* `curl`, `wget`: HTTP debug.
* `dig`, `nslookup`: DNS.
* `nmap`: port scanning.
* `nc` (netcat): TCP/UDP testing.
* `iperf`: bandwidth testing.

***

### **Cloud Networking Basics**

* **VPC:** isolated private network.
* **Subnets:** routing units.
* **Security Groups:** stateful firewalls.
* **NACLs:** stateless L3/L4 ACLs.
* **Internet Gateway:** outbound internet.
* **NAT Gateway:** private → internet.
* **VPC Peering:** L3 connection between VPCs.
* **Transit Gateway:** central hub for multi-VPC.

***

### **Kubernetes Networking (very common in interviews)**

* Nodes must be able to reach each Pod (L3).
* Pod gets separate IP.
* No NAT between Pods in cluster.
* CNI plugins: Calico, Cilium, Flannel, Weave.
* Services expose stable virtual IP (iptables or IPVS).
* Ingress manages L7 HTTP routing.
* Kube-proxy handles L4 load balancing.
* Cilium/Envoy handle advanced L7 policy & mesh.
