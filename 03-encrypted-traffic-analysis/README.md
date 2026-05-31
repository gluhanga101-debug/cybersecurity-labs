# 03 — Encrypted Traffic Analysis and Contextual Risk Evaluation

## Overview
Captured and analysed live browser traffic using Wireshark on an Ubuntu VM to demonstrate how defenders can evaluate encrypted outbound connections without reading packet content. Established a normal traffic baseline, identified an outbound TLS connection to a file-sharing service (we.tl), and performed a full session-level investigation using IP-based filtering and lifecycle review.

## Tools Used
- Ubuntu VM (log01) — monitored system and packet capture host
- Wireshark — traffic capture and analysis
- Firefox — traffic generation
- Stellar Cyber — referenced for correlation context

## Environment
- Ubuntu VM running in Oracle VirtualBox
- Wireshark capturing on NAT-facing interface (enp0s3)
- IP address: 10.0.2.15

## What I Did
1. Captured live browser traffic on enp0s3 and applied an http filter to establish a baseline
2. Identified expected traffic including Firefox connectivity checks and routine web requests
3. Used TLS Server Name Indication (SNI) filter to detect outbound communication to we.tl
4. Extracted the destination IP (3.169.202.120) and filtered the full session
5. Reviewed the complete connection lifecycle: TCP handshake, TLSv1.3 Client Hello, encrypted application data, keep-alive behaviour, and FIN/ACK termination

## Key Finding
A technically normal TLS session can still require analyst attention depending on where it goes and what business context surrounds it. The we.tl file-sharing service presents a data exfiltration risk even when the session is well-formed and properly terminated. Detection in encrypted environments depends on metadata — domains revealed during TLS negotiation and session behavioural patterns — rather than content inspection.

## Challenges
- Expected HTTP-style content inspection could not reveal the destination directly because we.tl traffic appeared as encrypted TLS
- Solving this required switching from HTTP filtering to TLS SNI filtering, then pivoting to IP-based session investigation

## Report
[View Full Lab Report (PDF)](./Encrypted_Traffic_Analysis_Lab_Report.pdf)
