### Azure Load Balancing Services

Azure provides multiple load balancing options to distribute traffic efficiently, enhance application availability, and ensure high performance across your infrastructure.

#### 1. Azure Load Balancer (Layer 4 - TCP/UDP)
Azure Load Balancer is a Layer 4 (TCP, UDP) load balancer that distributes inbound and outbound traffic to virtual machines (VMs) and other compute resources.
| Load Balancer Type               | Description                                   | Use Case                                    |
| -------------------------------- | --------------------------------------------- | ------------------------------------------- |
| **Public Load Balancer**         | Routes internet traffic to Azure resources    | Web apps, APIs accessible from the internet |
| **Internal Load Balancer (ILB)** | Balances traffic within Azure virtual network | Backend services, private apps              |

### Key Features:
* Inbound and outbound rules
* Health probes (HTTP, TCP, HTTPS)
* NAT rules for port forwarding
* High throughput (millions of flows)
* Zonal or zone-redundant deployment

### Use Cases:
* Load balancing web servers in a VM scale set
* Distributing database read replicas
* Enabling highly available backend APIs

#### 2. Azure Application Gateway (Layer 7 - HTTP/HTTPS)
A Layer 7 (Application layer) load balancer that can route and inspect HTTP/HTTPS traffic using advanced features like SSL termination, URL-based routing, and WAF (Web Application Firewall).

### Key Features:
* URL path-based routing
* Host-based routing
* SSL offloading and end-to-end encryption
* Integrated Web Application Firewall (WAF)
* Autoscaling and zone redundancy

### Use Cases:
* Load balancing microservices based on URL (e.g., /api, /login)
* Secure web apps using WAF
* Multi-site hosting

#### 3. Azure Front Door (Global HTTP/HTTPS Load Balancer)
Azure Front Door is a global Layer 7 load balancer and CDN, best for serving global applications with low latency, smart routing, and built-in failover.

### Key Features:
* Smart routing based on latency and health
* SSL offloading
* Web Application Firewall (WAF)
* URL redirection and rewrite
* Integration with Azure CDN

### Use Cases:
* Global websites with traffic from multiple regions
* High availability with geo-failover
* API gateway with global reach

#### 4. Traffic Manager (DNS-based Load Balancer)
Traffic Manager uses DNS to route users to endpoints based on routing policies such as:
  * Priority
  * Performance (latency)
  * Geographic location
  * Weighted round-robin
  * Multi-value routing
    
### Use Cases:
  * Redirecting users to the nearest regional endpoint
  * Geo-redundant application deployments
  * Hybrid cloud DNS routing
** Note:** Traffic Manager only directs traffic; it doesn't handle the traffic directly like Load Balancer or App Gateway.


| Service             | Layer | Traffic Type | Global | Protocols      | Intelligent Routing   | WAF Support |
| ------------------- | ----- | ------------ | ------ | -------------- | --------------------- | ----------- |
| Azure Load Balancer | L4    | TCP/UDP      | ❌      | TCP, UDP       | ❌                     | ❌           |
| Application Gateway | L7    | HTTP/HTTPS   | ❌      | HTTP, HTTPS    | ✅ (URL/path/host)     | ✅           |
| Azure Front Door    | L7    | HTTP/HTTPS   | ✅      | HTTP, HTTPS    | ✅ (Latency, failover) | ✅           |
| Traffic Manager     | DNS   | DNS-based    | ✅      | All (indirect) | ✅                     | ❌           |

#### Hands-On Lab Suggestion

**Goal:** Set up a Load Balanced Web App using Azure Load Balancer

**Tasks:**
* Create a VNet and 2 subnets
* Deploy 2 VMs with IIS installed
* Place both VMs in an availability set
* Create a Load Balancer with health probes
* Configure load balancing rule for port 80
* Test with browser and simulate failure

  <img width="3052" height="2404" alt="image" src="https://github.com/user-attachments/assets/dd4eadb4-a12e-4904-bed6-9a0434b1f712" />

