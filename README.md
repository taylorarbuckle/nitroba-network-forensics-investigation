# Nitroba Network Forensics Investigation

## Overview

This project documents a network-forensics investigation of the `nitroba.pcap` packet capture using Wireshark. The goal was to identify a workstation associated with suspicious web activity, reconstruct relevant HTTP traffic, and document evidence surrounding threatening messages observed in the capture.

> **Scope note:** This report states only what was directly supported by the packet-capture evidence reviewed during the investigation. Account names and network activity are treated as technical artifacts; they do not, by themselves, establish the identity of a real-world person.

## Tools

- Wireshark
- Display filters and packet-byte searches
- HTTP/TCP stream reconstruction
- HTTP form-data analysis
- Timeline correlation

## Key Findings

The investigated workstation used the private IPv4 address **192.168.15.4**. Ethernet/ARP evidence associated that host with MAC address **00:17:f2:e2:c0:ce**, displayed by Wireshark with an Apple vendor label.

Traffic from `192.168.15.4` contained Gmail-related HTTP activity in which the string **`jcoachj@gmail.com`** was visible. This establishes that the account string appeared in network traffic associated with the investigated host, but it is not sufficient on its own to attribute the activity to a particular real-world person.

The same source IP also connected over HTTP to **`www.sendanonymousemail.net`**. Packet **80614** contains a `POST /send.php HTTP/1.1` request with URL-encoded form data. The reconstructed request exposes the following fields:

| Field | Observed value |
| --- | --- |
| Destination email | `lilytuckrige@yahoo.com` |
| Sender | `the_whole_world_is_watching@nitroba.org` |
| Subject | `Your class stinks` |
| Message | Begins with `Why do you persist in teaching a boring class?` |
| Security code | `xkpmkb` |
| HTTP response | `200 OK` |

The packet capture also contains another HTTP form submission with the subject **`you can't find us`** and message text containing **`and you can't hide from us.`**, **`Stop teaching.`**, and **`Start running.`**

## Timeline

### Gmail-related activity

Packet searches for `jcoachj@gmail.com` returned HTTP/TCP traffic involving the workstation `192.168.15.4`. One reconstructed Gmail HTTP stream displayed `jcoachj@gmail.com` in the returned page content.

### Anonymous-email website access

The workstation accessed resources at `sendanonymousemail.net` and subsequently submitted an HTTP POST to `/send.php`.

### Anonymous email submission

**Packet 80614** was captured at:

- **Local capture time:** July 22, 2008, 02:02:57.548149 EDT
- **UTC:** July 22, 2008, 06:02:57.548149 UTC

The request originated from **192.168.15.4** and was sent to **69.80.225.91** over HTTP. Following the TCP/HTTP stream exposed the form fields and message contents in plaintext.

## Investigation Methodology

### 1. Identify the workstation

Traffic was narrowed to the internal host:

```text
ip.src == 192.168.15.4
```

ARP/Ethernet inspection showed the associated MAC address `00:17:f2:e2:c0:ce`.

### 2. Examine HTTP POST activity

Because unencrypted HTTP can expose submitted form data, POST requests from the host were reviewed. A useful filter was:

```text
ip.src == 192.168.15.4 && http.request.method == "POST"
```

URL-encoded form submissions were then inspected in the packet-details pane.

### 3. Search for Gmail artifacts

Packet content was searched for the account string:

```text
jcoachj@gmail.com
```

Relevant Gmail traffic was isolated and HTTP/TCP streams were followed to examine surrounding application data.

### 4. Isolate the anonymous-email service

Traffic to the anonymous-email site was narrowed with:

```text
ip.src == 192.168.15.4 && http.host contains "sendanonymousemail"
```

This revealed the browsing requests and the later `POST /send.php` submission.

### 5. Reconstruct the POST request

Following the stream containing packet 80614 revealed the request headers and URL-encoded body. Because the transaction used HTTP rather than HTTPS, the form contents were readable directly from the capture.

### 6. Correlate timestamps and artifacts

The source IP, HTTP host, form contents, Gmail-related artifacts, and packet timestamps were compared to build a defensible evidence timeline without extending attribution beyond what the capture supports.

## Evidence

The investigation captured screenshots demonstrating:

1. The anonymous-email HTTP stream and submitted form contents.
2. A packet-content search locating `jcoachj` in Gmail-related traffic.
3. A threatening-message form submission showing the subject and message content.
4. Filtering of traffic containing `jcoachj@gmail.com` from the investigated workstation.
5. Packet 80614's exact arrival timestamp.

Screenshots are intentionally treated as supporting exhibits rather than proof of real-world identity. The strongest technical conclusion is that the observed activities occurred in traffic associated with the same investigated internal host.

## Conclusion

Analysis of `nitroba.pcap` identified **192.168.15.4** as a workstation of interest. The capture shows Gmail-related traffic containing `jcoachj@gmail.com`, access to an anonymous-email service, and an HTTP POST containing a hostile message. A separate captured form submission contains additional threatening language.

The investigation demonstrates how plaintext application-layer traffic can be reconstructed with Wireshark and correlated through IP addresses, packet contents, HTTP requests, streams, and timestamps. The evidence supports correlation of these network artifacts to the investigated host; additional endpoint, account-provider, or organizational evidence would be required for stronger attribution to a specific individual.

## Skills Demonstrated

- Packet capture analysis
- Wireshark display filtering
- TCP/HTTP stream reconstruction
- HTTP request and form-data analysis
- Network artifact correlation
- Timeline development
- Evidence documentation
- Careful forensic attribution

## Dataset

This project analyzes the `nitroba.pcap` training capture. The packet capture itself is not included in this repository.
