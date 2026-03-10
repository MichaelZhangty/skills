---
name: ip-lookup
description: Investigate any IP address or hostname — geolocation, ASN/ISP, reverse DNS (PTR), RDAP/WHOIS network block, and optional AbuseIPDB reputation check. No API keys needed for core features. Use when the user asks about an IP address, wants to geolocate an IP, look up who owns a network block, find the ISP or ASN for an IP, check abuse reputation, or do a reverse DNS lookup. Trigger phrases include "who owns this IP", "where is this IP located", "look up IP", "check if IP is malicious", "reverse DNS for", "what ASN is", "whois for IP".
metadata: {"openclaw":{"emoji":"🔍","requires":{"bins":["python3"]}}}
---

# IP Lookup

Zero-dependency network intelligence for any IP or hostname. Uses only Python stdlib — **no pip install required**.

## Quick Start

```bash
python3 {baseDir}/scripts/ip_lookup.py <ip_or_hostname>
```

**Examples:**

```bash
python3 {baseDir}/scripts/ip_lookup.py 8.8.8.8
python3 {baseDir}/scripts/ip_lookup.py github.com
python3 {baseDir}/scripts/ip_lookup.py 1.1.1.1 --no-rdap        # skip WHOIS (faster)
python3 {baseDir}/scripts/ip_lookup.py 185.220.101.1 --abuse     # + AbuseIPDB check
python3 {baseDir}/scripts/ip_lookup.py 8.8.8.8 --json            # machine-readable JSON
```

## Output Panels

| Panel | Data | API | Auth |
|---|---|---|---|
| 🌍 Geolocation | Country, city, coords, timezone, ISP | ip-api.com (ipwho.is fallback) | None |
| 🔄 Reverse DNS | PTR record | dns.google | None |
| 📋 RDAP / WHOIS | Network name, CIDR block, abuse contact, registration date | rdap.arin.net (RIPE fallback) | None |
| 🛡 Abuse (opt) | Confidence score, report count, last seen | api.abuseipdb.com | Free key |

## Flags

| Flag | Effect |
|---|---|
| `--json` | Raw JSON output (pipe-friendly) |
| `--abuse` | Enable AbuseIPDB check (set `ABUSEIPDB_KEY` env var) |
| `--no-rdap` | Skip RDAP/WHOIS (faster for simple geo queries) |
| `--no-ptr` | Skip reverse DNS lookup |

## AbuseIPDB Setup (optional)

1. Create a free account at https://www.abuseipdb.com
2. Get API key from the dashboard
3. `export ABUSEIPDB_KEY=your_key_here`
4. Run with `--abuse` flag

## Notes

- Hostnames are auto-resolved to IP before lookup
- RDAP uses ARIN first, falls back to RIPE for European addresses
- ip-api.com free tier: 45 requests/minute
- IPv6 supported for geo/RDAP; PTR lookup is IPv4-only
