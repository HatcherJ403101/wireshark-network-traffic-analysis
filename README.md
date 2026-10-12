# Wireshark Network Traffic Analysis

## Project Overview

This project demonstrates foundational network traffic analysis skills using Wireshark and publicly available sample packet captures. The investigations focus on understanding network protocols, filtering packet data, identifying unusual activity, and documenting evidence-based findings.

## Objectives

- Analyse DNS queries and responses.
- Use Wireshark display filters to investigate network traffic.
- Identify successful and unsuccessful DNS lookups.
- Examine repeated DNS lookup failures.
- Develop structured investigation reports suitable for a SOC portfolio.
- Distinguish observable evidence from assumptions about malicious activity.

## Investigations

### Investigation 01 — DNS Traffic Analysis

**Status:** Completed

Analysed a sample DNS packet capture to identify successful DNS responses, `NXDOMAIN` responses, and repeated failed hostname lookups.

Key observations included:
- 38 packets in the capture.
- 19 DNS queries.
- 13 successful DNS responses.
- 6 `NXDOMAIN` response packets.
- Repeated unsuccessful lookups for a hostname in the sample.

The findings were documented without assuming the failed lookups were malicious.

**Report:** [01-dns-traffic-analysis.md](investigations/01-dns-traffic-analysis.md)

**Evidence:** See the `evidence` folder.

### Investigation 02 — Suspicious Network Traffic

**Status:** Planned

The next investigation will examine a publicly available capture associated with suspicious or malware-related network activity. The aim is to identify relevant network indicators, interpret the packet-level evidence, and document the limits of the analysis.

The investigation will use offline packet analysis. No malware will be executed.

## Tools and Techniques

- **Wireshark** — packet inspection and protocol analysis
- DNS query and response analysis
- Display filters
- DNS response-code analysis
- Evidence collection and reporting
- Security investigation and triage

## Scope and Safety

This project uses sample packet captures for educational purposes. Findings relate only to the traffic contained in each capture and are not assumed to represent current network activity.

Potentially suspicious traffic will be assessed using the available evidence. A single unusual packet or failed DNS lookup is not sufficient to confirm a security incident.

## Skills Demonstrated

- Network traffic analysis
- DNS fundamentals
- Investigative reasoning
- Evidence-based documentation
- Security alert triage
- Recognition of analytical limitations
