# Common Network Security Threats

## Executive Summary

This report surveys three major network security threats — **Denial of Service (DoS/DDoS)**, **Man-in-the-Middle (MITM)** attacks, and **IP-based spoofing** — explaining how they work, their real-world impacts, and practical mitigations. It draws on recent academic surveys and incident reports to provide defendable recommendations for network operators and system administrators.

## Table of Contents

1. Introduction
2. Denial of Service (DoS / DDoS)

   * 2.1 How it works
   * 2.2 Types & techniques
   * 2.3 Real-world examples
   * 2.4 Mitigations & best practices
3. Man-in-the-Middle (MITM) Attacks

   * 3.1 How it works
   * 3.2 Common MITM variants
   * 3.3 Detection & mitigation
4. IP Spoofing and Related Impersonation Attacks

   * 4.1 How IP spoofing works
   * 4.2 Role in DDoS and other attacks
   * 4.3 Detection & defenses
5. Recommendations & Best Practices
6. Conclusion
7. References & Suggested Reading

## 1. Introduction:

Network-facing services are exposed to a range of attacks that target availability (DoS/DDoS), confidentiality and integrity (MITM), and identity (spoofing). This report summarizes their mechanisms, impact, examples, and practical mitigations. It synthesizes academic surveys and real incident analyses to give actionable guidance.

## 2. Denial of Service (DoS / DDoS):

### 2.1 How it works

A Denial of Service (DoS) attack aims to make a target resource (website, API, network service) unavailable to legitimate users by exhausting resources — bandwidth, compute, connection tables, or application resources. When multiple distributed machines (often botnets) participate it becomes a Distributed DoS (DDoS). Modern DDoS campaigns use volumetric floods, protocol abuses, and application-level requests to overload targets.

### 2.2 Types & techniques

* **Volumetric attacks**: saturate bandwidth (UDP floods, amplification such as memcached/NTP).
* **Protocol attacks**: exhaust protocol state (SYN floods, TCP connection exhaustion).
* **Application-layer attacks**: send resource-expensive requests (slowloris, heavy search queries) to overwhelm application logic.
* **Amplification attacks**: send forged requests to reflectors (DNS, NTP, memcached) that multiply traffic to victims. The 2018 GitHub attack used memcached amplification to produce terabits/second traffic.

### 2.3 Real-world examples

* **GitHub (March 2018)** — Memcached amplification produced ~1.35 Tbps peak traffic; scrubbing via Akamai mitigated the attack. This incident highlights how exposed amplification services on the Internet become force multipliers for DDoS.
* Numerous IoT/botnet-driven attacks (Mirai family) that leveraged insecure devices to form large attack fleets are covered across DDoS surveys.

### 2.4 Mitigations & best practices

* **Network edge filtering**: block spoofed traffic, apply rate limits and access control lists (ACLs).
* **Anti-DDoS services / scrubbing**: cloud scrubbing centers (Akamai/Cloudflare/AWS Shield) absorb and filter large volume attacks.
* **Protocol hardening**: disable or rate-limit UDP services that can be abused for amplification (memcached, open DNS recursive).
* **Capacity & redundancy**: overprovision bandwidth and use multi-CDN / multi-ISP strategies.
* **Application protections**: WAFs, caching, request throttling and challenge-response (CAPTCHA) for abusive clients.
* **Anomaly detection / ML**: modern surveys discuss ML methods to detect DDoS patterns, especially in IoT/SDN environments.

---

## 3. Man-in-the-Middle (MITM) Attacks:

### 3.1 How it works

A MITM attack occurs when an adversary intercepts or relays communications between two parties and can eavesdrop, modify, or inject traffic without the participants’ consent. MITM can be performed via ARP spoofing on LANs, rogue Wi-Fi access points, DNS manipulation, or HTTPS stripping. Attackers can intercept credentials, session cookies, or confidential data. Surveys detail a broad taxonomy and detection approaches.

### 3.2 Common MITM variants

* **ARP spoofing / poisoning**: attacker poisons ARP caches on LAN so traffic is routed via the attacker.
* **DNS spoofing/poisoning**: resolve a domain to attacker IPs to intercept users.
* **SSL/TLS stripping**: downgrade or remove TLS, forcing plaintext HTTP.
* **Rogue access points / evil twin**: attacker stands up a Wi-Fi AP that mimics a legitimate network.
* **Proxy/Middleware insertion**: insertion of a malicious proxy or compromised router.

### 3.3 Detection & mitigation

* **Encryption & authentication**: use TLS with strong configurations (HSTS, modern ciphers), certificate pinning where applicable.
* **Mutual authentication**: for sensitive systems, use mutual TLS.
* **Network hardening**: DHCP/ARP inspection, dynamic ARP inspection (on managed switches), and 802.1X for Wi-Fi.
* **DNS security**: DNSSEC and DNS over HTTPS/TLS where possible.
* **Anomaly detection**: monitor ARP table changes, unexpected certificate changes, or abnormal routing. Modern detection research applies traffic analysis and ML for MITM detection.

---

## 4. IP Spoofing and Related Impersonation:

### 4.1 How IP spoofing works

**IP spoofing** is the forging of the source IP address in packet headers so traffic appears to originate from another host. Spoofing can be used to hide attackers’ origin, to facilitate amplification (by placing victim as reply target), or to bypass simple access controls. Detection of spoofed packets is non-trivial on the open Internet. Research proposes techniques such as hop-count verification (TTL/hop-count based filters) and packet attribute analysis for detection.

### 4.2 Role in DDoS and other attacks

* **Reflection/amplification**: Spoofed source addresses allow reflected replies to be sent to victims (amplification).
* **Session hijacking & spoofed admin access**: Spoofing is foundational for impersonation attacks aiming to trick services or session logic.
* **Network instability**: spoofed routing information can misdirect traffic or disturb routing tables.

### 4.3 Detection & defenses

* **Ingress/Egress filtering (BCP 38 / RFC 2827)**: ISPs and enterprise borders should drop packets with invalid source addresses that should not originate from connected networks. Widespread deployment reduces spoofing feasibility.
* **Hop-count / TTL verification (Unicast Reverse Path Forwarding, uRPF)**: check whether the incoming packet’s route is consistent with its source.
* **Anomaly detection**: statistical analysis of packet attributes to detect spoofed flows. Academic surveys compare methods and recommend multi-layer defenses.

---

## 5. Recommendations & Best Practices:

1. **Harden network perimeter**: implement ACLs, uRPF, and BCP 38 at ISP/edge routers where feasible.
2. **Use TLS everywhere**: enforce HTTPS with HSTS, modern cipher suites, and certificate management. Pin certificates for critical apps.
3. **Anti-DDoS strategy**: use cloud scrubbing services and rate limiting; disable UDP services that can be abused.
4. **Monitor & detect**: deploy IDS/IPS, flow monitoring (NetFlow/sFlow), and anomaly detectors; log and alert on unusual ARP/DNS/certificate events.
5. **Segment networks**: separate critical services and management interfaces from user networks; apply 802.1X for Wi-Fi.
6. **Patch & inventory**: maintain an asset inventory, patch known service vulnerabilities, and remove exposed amplification services.
7. **Incident plan**: have DDoS response runbooks, contact points with ISPs and scrubbing services, and forensic logging.

---

## 6. Conclusion:

DoS/DDoS, MITM, and IP spoofing remain among the most impactful network threats today. Effective defense requires **layered measures**: protocol hardening, perimeter filtering, encryption, detection, and operational readiness. Recent research emphasizes automated detection (ML) and SDN-centric defenses, but practical steps like ingress filtering, TLS enforcement, and DDoS scrubbing are immediate and necessary actions.

---

## 7. References & Suggested Reading:

Below are the main scholarly sources and high-quality articles used to produce this report. Use these in your reference section 

1. **Distributed Denial of Service Attacks: A Survey** — survey paper covering DDoS taxonomy and defenses (2021).
2. **Detecting and Mitigating DDoS Attacks with AI: A Survey** — arXiv survey on ML/AI DDoS detection methods (2025).
3. **A Survey of Man-in-the-Middle Attacks** — comprehensive literature review and taxonomy of MITM attacks.
4. **A Study on Efficient Detection of Network-based IP Spoofing** (Seo et al., 2016) — analysis and detection techniques.
5. **Survey on Defenses Techniques Used for Controlling IP Spoofing** — survey comparing anti-spoofing methods.
6. **GitHub DDoS memcached incident reporting and analysis** — Wired article and mitigation summary (2018).
7. **Man-in-the-Middle Attacks in MANETs** — MDPI article on MITM detection in mobile ad hoc networks (2022).

