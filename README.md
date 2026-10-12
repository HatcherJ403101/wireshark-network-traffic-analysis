# Wireshark Network Traffic Analysis

## Project Overview

This project demonstrates foundational network traffic analysis and security investigation skills using Wireshark and publicly available sample packet captures.

The investigations cover DNS analysis, TCP traffic inspection, packet filtering and the examination of a suspicious network conversation. Findings are documented in structured reports and supported by screenshots.

## Objectives

- Analyse DNS queries and responses.
- Use Wireshark display filters to narrow down network traffic.
- Identify unsuccessful DNS lookups and interpret response codes.
- Investigate TCP traffic using packet details and stream reconstruction.
- Examine application-level data within captured network conversations.
- Document evidence and explain the limitations of investigative conclusions.

## Investigations

### Investigation 01 — DNS Traffic Analysis

**Status:** Completed

Analysed a sample DNS capture to distinguish successful responses from unsuccessful lookups and investigate repeated DNS failures.

Key observations:
- 38 packets in the capture.
- 19 DNS queries.
- 13 successful DNS responses.
- 6 `NXDOMAIN` response packets.
- Repeated unsuccessful lookups for a hostname.

The report explains why unsuccessful DNS responses are not, by themselves, proof of malicious activity.

**Report:** [Read the DNS analysis](investigations/01-dns-traffic-analysis.md)

**Evidence:** [Browse investigation screenshots](evidence/)

### Investigation 02 — DNS Remote Shell Analysis

**Status:** Completed

Analysed a publicly available Wireshark sample capture documented as an example of remote-shell activity involving a port commonly associated with DNS.

Key observations:
- 131 packets in the capture.
- 24 packets matched the filter for TCP traffic involving port 53 that Wireshark did not decode as DNS.
- 10 packets matched the same filter while carrying TCP payload data.
- Packet 31 contained 1,011 bytes of TCP payload.
- A reconstructed TCP conversation contained readable Windows XP command-shell output, including directory-listing information.

The report distinguishes packet-level observations from interpretations and explains why port numbers alone do not prove malicious activity.

**Report:** [Read the remote-shell investigation](investigations/02-malicious-traffic-analysis.md)

**Evidence:**
- [Windows command-shell stream](evidence/02-windows-shell-stream.png)
- [TCP payload on port 53](evidence/03-tcp-port-53-payload.png)

## Tools and Techniques

- **Wireshark** — packet inspection and protocol analysis
- DNS query and response analysis
- TCP port and payload analysis
- Display-filter construction
- TCP stream reconstruction
- Application-level traffic inspection
- Evidence collection and investigation reporting

## Scope and Safety

This project uses existing sample packet captures for educational purposes. The investigations were conducted offline; no malware was executed and no external systems were contacted as part of the analysis.

Findings relate to the traffic present in each supplied capture. The results
