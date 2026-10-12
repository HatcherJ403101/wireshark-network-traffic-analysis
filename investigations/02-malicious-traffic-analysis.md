# Investigation 02 — DNS Remote Shell Analysis

## 1. Objective

The objective of this investigation was to analyse a publicly available packet capture containing suspicious network activity, identify evidence of interactive shell communication, and assess the significance of traffic using TCP port 53.

The investigation focused on protocol analysis, TCP conversations, application-level content and evidence-based security assessment.

## 2. Tools and Data Source

- **Tool:** Wireshark
- **Capture file:** `dns-remoteshell.pcap`
- **Data source:** Wireshark Sample Captures
- **Analysis type:** Offline network traffic investigation

The capture is listed in the [official Wireshark Sample Captures collection](https://wiki.wireshark.org/SampleCaptures) under "Viruses and worms." Its description identifies it as an example of a remote shell operating over a port associated with DNS.

The investigation was conducted against the existing capture. No malware was executed and no external systems were contacted.

## 3. Methodology

The capture was opened in Wireshark and initially examined using the Protocol Hierarchy view. Display filters were then used to narrow the investigation to DNS, Telnet, HTTP and TCP traffic.

The following filters were used:

### Identify TCP traffic involving port 53 that was not decoded as DNS

```text
tcp.port == 53 && !dns
```

### Identify packets from that traffic containing TCP payload data

## 8. Evidence

The following screenshots support the observations documented in this investigation.

- **Windows shell conversation:** [View shell output](../evidence/02-windows-shell-stream.png) — shows the Windows XP command prompt and directory listing observed in the TCP stream.
- **TCP payload analysis:** [View packet evidence](../evidence/03-tcp-port-53-payload.png) — shows the filtered TCP traffic involving port 53 and the selected packet with a 1,011-byte payload.

The screenshots provide supporting evidence for the analysis; they should be interpreted alongside the methodology, findings and limitations described above.

```text
tcp.port == 53 && tcp.len > 0 && !dns
```

### Isolate the HTTP traffic

```text
http
```

### Examine a specific packet

```text
frame.number == 22
```

TCP conversations were inspected using Wireshark's Follow TCP Stream feature to examine readable application-level content.

## 4. Findings

### Finding 1 — Capture overview

The capture contained 131 packets.

The Protocol Hierarchy view identified TCP as the predominant transport protocol, with additional traffic associated with DNS, Telnet, HTTP and TLS.

Protocol Hierarchy counts can overlap because packets may be counted at multiple protocol layers.

### Finding 2 — TCP traffic using port 53

The display filter:

```text
tcp.port == 53 && !dns
```

returned 24 packets.

This indicated that Wireshark had not decoded these TCP packets as DNS, despite their use of port 53.

TCP port 53 is commonly associated with DNS, although DNS is not the only possible use of that port. The use of the port alone was therefore treated as an investigative lead rather than proof of malicious activity.

### Finding 3 — TCP payload data

Adding the condition `tcp.len > 0` reduced the filtered results to 10 packets carrying TCP payload data.

Packet 31 was examined in detail.

| Field | Observed value |
|---|---|
| Packet number | 31 |
| Source address | `192.168.1.2` |
| Source port | `53` |
| Destination address | `192.168.1.3` |
| Destination port | `1396` |
| TCP flags | `PSH, ACK` |
| TCP payload length | 1,011 bytes |

This packet carried 1,011 bytes of TCP payload from the endpoint using source port 53 to the endpoint using destination port 1396.

The TCP flags describe transport-level behaviour. They do not, by themselves, establish whether the transmitted content was malicious.

### Finding 4 — Interactive Windows command-shell output

Following the TCP conversation revealed readable command-shell output identifying Microsoft Windows XP, version 5.1.2600.

The conversation included the Windows `dir` command and a directory listing for the `C:\` drive. The output included file and directory names, file sizes and available disk space.

The observed interaction demonstrated that command input and system information were present in the network conversation.

A separate attempted `ls -la` command was also observed in the stream and was not recognised by the Windows command environment.

The presence of readable command-shell interaction, combined with the TCP traffic using port 53, supports the assessment that this capture contains remote-shell activity over a port commonly associated with DNS.

### Finding 5 — HTTP request and response

The HTTP conversation contained the following request:

```http
GET /images/empty.gif HTTP/1.1
Host: www.goals365.com
Referer: http://www.goals365.com/livescore.html
```

The server responded with:

```http
HTTP/1.1 304 Not Modified
```

This response indicates that the server considered the requested resource unchanged for the relevant conditional request.

The HTTP exchange provides additional context about the captured traffic, but it does not independently establish malicious activity or a connection to the remote-shell behaviour.

## 5. Security Assessment

The principal finding was the presence of readable command-shell interaction within a TCP conversation involving port 53.

The investigation identified three mutually supporting observations:

- TCP traffic involving port 53 that Wireshark did not decode as DNS.
- TCP payload data travelling between the observed endpoints.
- Readable Windows command-shell output in the TCP conversation.

Together, these observations are consistent with the remote-shell behaviour described by the sample's publisher.

In a live SOC investigation, this combination would warrant further investigation. Relevant follow-up work could include correlating endpoint process logs, network connection records, DNS server logs and other activity from the same time period.

The capture alone does not establish who initiated the activity, the original infection method, the identity of an operator, or the complete impact on the affected system.

## 6. Limitations

- The investigation used a historical sample capture rather than live network telemetry.
- The displayed IP addresses belong to the private address range used within the captured environment.
- A port number alone does not identify the application or prove malicious intent.
- The command-shell transcript provides evidence of interactive command execution or simulation within the captured conversation, but not the identity of the operator.
- The precise relationship between the HTTP activity and the remote-shell conversation was not established.
- The investigation did not execute the commands or attempt to connect to any endpoints.

## 7. Conclusion

This investigation demonstrated how Wireshark can be used to identify traffic that does not match the expected protocol associated with a port, narrow down packets containing TCP payload data, and reconstruct an application-level conversation.

The most significant finding was readable Windows command-shell output within a TCP conversation involving port 53. This provided stronger evidence for the sample's documented remote-shell behaviour than port numbers or packet sizes alone.

The investigation reinforced the importance of correlating transport-level observations with application-level evidence before reaching a security conclusion.

## Skills Demonstrated

- Wireshark protocol analysis
- TCP and port analysis
- Display-filter construction
- TCP stream reconstruction
- Recognition of command-shell activity
- Investigative reasoning and triage
- Evidence-based reporting
- Assessment of findings and limitations
