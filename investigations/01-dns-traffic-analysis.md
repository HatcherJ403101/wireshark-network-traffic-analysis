# Investigation 01 — DNS Traffic Analysis

## 1. Objective

The objective of this investigation was to analyse a DNS packet capture using Wireshark, identify successful and unsuccessful DNS lookups, and assess what the observed traffic could indicate from a security monitoring perspective.

## 2. Tools and Data Source

- **Tool:** Wireshark
- **Capture file:** `dns.cap`
- **Data source:** Public Wireshark sample capture
- **Analysis type:** Offline DNS traffic analysis

This investigation used an existing sample capture. It did not involve monitoring live user traffic or conducting tests against external systems.

## 3. Methodology

The capture was opened in Wireshark and inspected using the packet list, DNS protocol details and display filters.

The following filters were used:

```text
dns.flags.response == 0
```

To identify DNS queries.

```text
dns.flags.response == 1 && dns.flags.rcode == 0
```

To identify successful DNS responses.

```text
dns.flags.rcode == 3
```

To identify DNS responses with the `NXDOMAIN` response code.

```text
dns.flags.response == 1 && dns.flags.rcode != 0 && dns.flags.rcode != 3
```

To check for other unsuccessful DNS response codes.

The hostname-specific filter below was also used to investigate repeated failed lookups:

```text
dns.qry.name == "(Blurred).local"
```

## 4. Findings

### Finding 1 — DNS traffic overview

The capture contained 38 packets in total. The analysis identified:

| Observation | Packets |
|---|---:|
| DNS queries | 19 |
| Successful DNS responses (`NOERROR`) | 13 |
| Unsuccessful responses (`NXDOMAIN`) | 6 |
| Other non-zero response codes, excluding `NXDOMAIN` | 0 |

The query and response counts account for all 38 packets in the capture.

### Finding 2 — Successful DNS lookups

Successful responses included an A-record lookup for `www.netbsd.org`, which returned an IPv4 address in the sample capture.

A TXT-record lookup for `google.com` also returned a response containing SPF-related text.

These observations demonstrate how DNS can return different types of records depending on the request. The results reflect the contents of this sample capture and should not be treated as verification of current DNS records.

### Finding 3 — Unsuccessful DNS lookups

Six response packets used the `NXDOMAIN` response code. This means the DNS resolver reported that the requested domain name did not exist.

The unsuccessful lookups included an attempted hostname lookup and several SRV-record queries associated with an internal-looking `.local` domain.

An `NXDOMAIN` response is not, by itself, evidence of an attack. It can result from a mistyped hostname, an unavailable or obsolete name, or an expected lookup that cannot be resolved in the captured environment.

### Finding 4 — Repeated lookup for a hostname

Filtering for `(Blurred).local` revealed two DNS queries and their corresponding `NXDOMAIN` responses. Both queries originated from the same source address in the sample, and the unsuccessful responses occurred approximately 7.4 seconds apart.

This is consistent with a name being looked up more than once without a successful resolution. The capture does not establish why the lookup was repeated or whether the behaviour was caused by an application, a user action, or another process.

## 5. Security Assessment

The analysis demonstrated several useful DNS monitoring techniques:

- Separating DNS queries from responses.
- Identifying successful and unsuccessful name resolutions.
- Filtering for specific DNS response codes.
- Investigating repeated lookups for an individual hostname.
- Distinguishing an observable event from a confirmed security incident.

Repeated failed lookups can be worth investigating in a real environment when supported by additional context, such as endpoint logs, process activity, DNS server logs or evidence of other suspicious behaviour.

However, this sample does not provide sufficient evidence to classify the observed failures as malicious activity.

## 6. Limitations

- The investigation used a sample capture rather than live network traffic.
- The capture represents only the traffic contained in the supplied file.
- The cause of the unsuccessful lookups could not be established.
- The results cannot be used to determine whether the same behaviour is occurring on a current network.
- No conclusion about compromise or malicious intent can be drawn from the `NXDOMAIN` responses alone.

## 7. Conclusion

The investigation successfully used Wireshark to examine DNS traffic, distinguish successful responses from unsuccessful ones, and investigate repeated failed hostname lookups.

The main finding was that the sample included six `NXDOMAIN` response packets, including repeated unsuccessful lookups for one hostname. This provided an opportunity to practise filtering, protocol analysis, evidence-based assessment and investigation documentation.

The investigation reinforced the importance of examining context before escalating a potential security event as malicious.

## Skills Demonstrated

- Wireshark packet analysis
- DNS protocol fundamentals
- Display-filter use
- DNS response-code analysis
- Basic security investigation and triage
- Evidence-based reporting
- Recognition of investigative limitations
