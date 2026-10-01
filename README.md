# Service-Discovery
A hands-on microservices project demonstrating dynamic service discovery, registration, and load balancing using HashiCorp Consul, Node.js (Express), and an API Gateway.
# Microservices Service Discovery with HashiCorp Consul

A hands-on implementation of a microservices architecture demonstrating how services dynamically register themselves and how an API Gateway routes traffic using HashiCorp Consul for service discovery.

## 🏗️ Architecture Overview

In a traditional setup, services communicate using hardcoded IP addresses and ports, which makes scaling and load balancing difficult. This project solves that by introducing a **Service Registry** (Consul). 

1. **Dummy Services (A, B, C):** Built with Node.js and Express. On startup, they automatically register their IP, port, and health check endpoints to the Consul registry.
2. **Consul Registry:** Acts as the central directory. It continuously pings the services (health checks) to ensure they are alive and removes them from the routing pool if they crash.
3. **API Gateway:** The single point of entry for clients. Instead of hardcoding the IPs of Services A, B, or C, the Gateway queries Consul's HTTP API to discover the live port/IP of the requested service and proxies the request to the first healthy node.

```text
[ Client ] 
    │
    ▼
[ API Gateway ] ◄───── Queries ────► [ Consul Registry ]
    │                                      ▲
    │ Dynamically routes                   │ Registers on startup
    │                                      │
    ├──► [ Service A (Port 3001) ] ────────┤
    ├──► [ Service B (Port 3002) ] ────────┤
    └──► [ Service C (Port 3003) ] ────────┘
