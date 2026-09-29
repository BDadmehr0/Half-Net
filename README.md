# Half-Net

## Iranian Half-Price Traffic VPN

A VPN architecture designed for users in Iran that combines **access to foreign internet services** with a potential **domestic/half-price traffic path** through an Iranian gateway.

> **Important:** VPN software itself cannot make traffic half-price. Traffic pricing is controlled by the user's ISP and its traffic classification policies. The Iranian server must already qualify for the relevant domestic/half-price traffic policy.

## Overview

The basic architecture is:

```text
                    Iran
┌─────────────────────────────────────────────┐
│                                             │
│   User Device                               │
│       │                                     │
│       │ VPN Tunnel                          │
│       ▼                                     │
│   Iranian Gateway                           │
│   VPN / Routing Server                      │
│       │                                     │
└───────┼─────────────────────────────────────┘
        │
        │ Encrypted Tunnel
        ▼
┌──────────────────────┐
│   Foreign VPS        │
│   Internet Gateway   │
└──────────┬───────────┘
           │
           ▼
    Foreign Services
```

## How It Works

1. The user connects to an **Iran-based VPN gateway**.
2. The ISP sees the user's connection primarily as traffic to the Iranian gateway.
3. If that gateway/IP is classified by the ISP as domestic or eligible for a reduced traffic tariff, the user-side traffic may receive that tariff.
4. The Iranian gateway forwards the traffic through an encrypted tunnel to a **foreign server**.
5. The foreign server accesses external services on behalf of the user.

## Components

### 1. Iranian Gateway

The Iranian gateway acts as the user's VPN entry point.

Possible technologies:

- WireGuard
- Linux routing/NAT
- nftables
- Policy routing
- Connection management
- Traffic accounting

### 2. Foreign Gateway

The foreign server provides access to services outside Iran.

Possible technologies:

- WireGuard
- Linux
- NAT
- IPv4/IPv6 forwarding
- Multiple exit nodes
- Load balancing

### 3. Client

Supported clients could include:

- Linux
- Windows
- Android
- iOS
- macOS

## Example Network

```text
Client
  │
  │ WireGuard
  ▼
Iran VPS
  │
  │ WireGuard / Encrypted Tunnel
  ▼
EU VPS
  │
  ▼
Internet
```

## Traffic Model

```text
User
 │
 │ Connection to Iranian Gateway
 ▼
Iranian Gateway
 │
 │ Encrypted tunnel
 ▼
Foreign Gateway
 │
 ▼
External Service
```

The important distinction is that **traffic pricing and traffic routing are separate concepts**.

Routing determines where packets go.

The ISP determines how those packets are classified and billed.

## Half-Price Traffic Requirement

The project does **not** attempt to manipulate or falsify ISP billing systems.

For half-price traffic to apply legitimately, the Iranian gateway must be covered by the applicable ISP/datacenter traffic policy.

This may depend on:

- ISP
- Datacenter
- IP address
- IP range
- ASN
- Domestic traffic classification
- Current operator policies
- Peering arrangements

Therefore, simply deploying a VPN on an Iranian VPS does **not** guarantee half-price traffic.

## Goals

- Provide a personal VPN gateway in Iran.
- Provide access to external internet services through a foreign exit node.
- Minimize user-side bandwidth costs where legitimate domestic/discounted traffic policies apply.
- Keep traffic encrypted between the Iranian and foreign gateways.
- Support multiple client platforms.
- Keep the infrastructure simple and auditable.

## Non-Goals

This project does not attempt to:

- Forge ISP traffic classifications.
- Manipulate ISP billing systems.
- Falsify destination information for billing purposes.
- Exploit vulnerabilities in ISP accounting systems.
- Guarantee half-price traffic on every ISP.

## Security Considerations

The Iranian gateway should not be treated as a trusted endpoint for sensitive data unless the infrastructure is controlled and properly secured.

Recommended practices:

- Use WireGuard with strong keys.
- Disable unnecessary services.
- Configure a restrictive firewall.
- Keep Linux packages updated.
- Do not expose administrative interfaces publicly.
- Rotate compromised keys immediately.
- Monitor bandwidth and connection activity.
- Separate management access from VPN traffic.

## Future Architecture

A larger deployment could use multiple gateways:

```text
                         ┌── Iran Gateway #1 ──┐
                         │                     │
Client ── Load Balancer ─┼── Iran Gateway #2 ──┼── Foreign Exit
                         │                     │
                         └── Iran Gateway #3 ──┘
```

Potential future features:

- Automatic gateway selection
- Multiple foreign exit locations
- Failover
- Bandwidth monitoring
- Client management
- Usage statistics
- API for provisioning WireGuard configurations
- Automatic health checks

## Disclaimer

Traffic pricing is determined by network operators and ISPs. This project cannot guarantee a specific tariff.

Any deployment should comply with the applicable laws, ISP terms, datacenter policies, and network usage rules.

## Status

**Concept / Architecture Design**

The next development step is to validate the Iranian gateway's traffic classification with the intended ISP/datacenter before implementing the complete VPN infrastructure.
