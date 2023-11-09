---
layout: post
title:  "Load Balancing Without a Floating IP: Achieving HA with HAProxy, Keepalived, and Dynamic DNS"
date:  2023-11-09
description: ""
img: lb.jpeg
tags: [kubernetes, loadbalancer, haproxy, keepalived, dynamicdns]
---
# 

High availability (HA) is a critical component of modern IT infrastructure, ensuring minimal service disruption and maintaining business continuity. In this post, we'll unveil a robust HA setup using HAProxy and Keepalived, with the assistance of Dynamic DNS for seamless failover, all without the need for a floating IP.

## The Python Script for Dynamic DNS Update

When the active load balancer fails, updating the DNS record to reflect the secondary LB's IP is crucial. Here is the Python script that automates the update:

```python
import sys
import requests

# Configuration parameters
api_endpoint = "https://dynamicdns.provider.com/update"
api_token = "your_api_token"
record_name = "lb.example.com"
record_type = "A"
new_ip = "secondary_lb_ip_address"

# Function to update the DNS record
def update_dns_record():
    data = {
        "token": api_token,
        "record": record_name,
        "type": record_type,
        "value": new_ip
    }
    try:
        response = requests.post(api_endpoint, data=data)
        response.raise_for_status()
        print(f"DNS record updated successfully: {record_name} IN {record_type} {new_ip}")
    except requests.exceptions.RequestException as e:
        print(f"Failed to update DNS record: {e}")

if __name__ == "__main__":
    if len(sys.argv) >= 2 and sys.argv[1] == "MASTER":
        update_dns_record()
```

## Keepalived Configuration

Keepalived manages failover and ensures high availability of services. Here's a snippet of both multicast and unicast configurations in the `keepalived.conf` file.

**Multicast Configuration on LB1:**

```conf
vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    virtual_ipaddress {
        192.168.0.100
    }
}
```

**Unicast Configuration on LB1:**

```conf
vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    unicast_src_ip LB1_PRIVATE_IP
    unicast_peer {
        LB2_PRIVATE_IP
    }
    virtual_ipaddress {
        192.168.0.100
    }
}
```

Replace `LB1_PRIVATE_IP` and `LB2_PRIVATE_IP` with the appropriate IP addresses.

## Multicast vs. Unicast

**Multicast:**
Used in L2 broadcast domains; easier setup; less configuration.

**Unicast:**
Can operate across subnets and over the internet; requires explicit peer IPs; more complex setup but greater flexibility.

## Summary

Achieving high availability for load balancers without a floating IP is entirely feasible. By combining HAProxy with Keepalived and Dynamic DNS, we create a resilient environment that can quickly adapt to outages by dynamically updating DNS records. The choice between multicast and unicast depends on your network infrastructure and specific needs, but both can be effectively used to maintain continuous service availability.