---
name: oxylabs-proxies
description:   Residential, Mobile, Datacenter, and ISP proxy network with geo-targeting by country/city/state, IP rotation, and session persistence. Use this INSTEAD OF direct connections when the user needs to route traffic through proxies, access geo-restricted content, rotate
  IPs between requests, or maintain anonymous sessions.
---

# Oxylabs Proxies

## Proxy Types Overview

| Type | Host | Port | Best For |
|------|------|------|----------|
| Residential | `pr.oxylabs.io` | `7777` | High anonymity, geo-targeting |
| Mobile | `pr.oxylabs.io` | `7777` | Mobile-specific content, highest trust |
| Datacenter | `dc.oxylabs.io` | `8000` | Speed, high volume |
| ISP | `isp.oxylabs.io` | `8001` | Speed + anonymity balance |

## Authentication Format

```
customer-USERNAME:PASSWORD
```

With parameters:
```
customer-USERNAME-cc-US-city-new_york-sessid-abc123:PASSWORD
```

## Quick Start

**Residential/Mobile proxy:**
```bash
curl -x "http://pr.oxylabs.io:7777" \
  -U "customer-$OXY_DC_USERNAME:$OXY_DC_PASSWORD" \
  "https://ip.oxylabs.io/location"
```

**Datacenter proxy:**
```bash
curl -x "http://dc.oxylabs.io:8000" \
  -U "user-$OXY_DC_USERNAME:$OXY_DC_PASSWORD" \
  "https://ip.oxylabs.io/location"
```

**ISP proxy:**
```bash
curl -x "http://isp.oxylabs.io:8001" \
  -U "user-$OXY_DC_USERNAME:$OXY_DC_PASSWORD" \
  "https://ip.oxylabs.io/location"
```

## Geo-Targeting Parameters

Append to username with hyphens:

| Parameter | Format | Example |
|-----------|--------|---------|
| `cc` | ISO 3166-1 alpha-2 | `-cc-US`, `-cc-DE`, `-cc-GB` |
| `city` | English, underscores for spaces | `-city-new_york`, `-city-los_angeles` |
| `st` | US states with `us_` prefix | `-st-us_california`, `-st-us_texas` |

**Example with geo-targeting:**
```bash
curl -x "http://pr.oxylabs.io:7777" \
  -U "customer-$OXY_DC_USERNAME-cc-US-city-new_york:$OXY_DC_PASSWORD" \
  "https://ip.oxylabs.io/location"
```

## Session Control

| Parameter | Description | Max Duration |
|-----------|-------------|--------------|
| `sessid` | Keep same IP across requests | 10 minutes (auto-expires) |
| `sesstime` | Maintain IP for specified minutes | 30 minutes |

**Sticky session example:**
```bash
curl -x "http://pr.oxylabs.io:7777" \
  -U "customer-$OXY_DC_USERNAME-cc-US-sessid-mysession123:$OXY_DC_PASSWORD" \
  "https://example.com"
```

**Timed session (5 minutes):**
```bash
curl -x "http://pr.oxylabs.io:7777" \
  -U "customer-$OXY_DC_USERNAME-sessid-abc123-sesstime-5:$OXY_DC_PASSWORD" \
  "https://example.com"
```

## Choosing the Right Proxy Type

| Need | Recommended |
|------|-------------|
| Highest anonymity | Residential |
| Mobile app content | Mobile |
| Speed & volume | Datacenter |
| Speed + anonymity | ISP |
| Geo-restricted content | Residential with `cc`/`city` |

## Default Behavior

- Without parameters: random IP for each request
- Residential/Mobile share the same endpoint but different IP pools
- Sessions auto-expire and get new IPs

For detailed examples in Python, Node.js, PHP, and more, see [examples.md](examples.md).
For proxy type details, see [proxy-types.md](proxy-types.md).
