---
title: "Works"
# meta title
meta_title: "Works"
# meta description
description: ""
# save as draft
draft: false
---

{{< toc >}}

## Projects

{{< tabs >}}
{{< tab "Re Protocol" >}}

FIX protocol on blockchain.

{{< button label="Learn More" link="/re" style="solid" >}}
{{< /tab >}}

{{< tab "Cendermint" >}}

A feature-rich Prometheus exporter tailored for Cosmos SDK chains.

{{< button label="Source Code" link="https://github.com/jim380/Cendermint" style="solid" >}}
{{< /tab >}}

{{< tab "Coris Explorer" >}}

A community-built explorer for the Cosmos ecosystem, built from scratch.

{{< button label="Source Code" link="https://github.com/jim380/coris-v2" style="solid" >}}
{{< /tab >}}

{{< tab "Bootstrap Me" >}}

A tool for easier and faster node launch on any Cosmos chain.
{{< button label="Source Code" link="https://github.com/jim380/bootstrap-me" style="solid" >}}
{{< /tab >}}

{{< tab "Node Tooling" >}}

Streamline node operations with code.
{{< button label="Source Code" link="https://github.com/jim380/node_tooling" style="solid" >}}
{{< /tab >}}

{{< /tabs >}}

## Infrastructure

### Networking

At the high-level our validator nodes are shielded from the public internet by a cluster of relay nodes in a private network. The relay nodes sit behind a separate cluster of sentry nodes that’s connected directly to the open network. This architecture design was put in place to mitigate DDoS attacks by employing the following techniques:

- **Firewall Protection** - Our validator nodes’ public IPs are not gossiped to the open network or whitelisted, trusted peers. The firewall on the relay nodes blocks all connections from IP addresses that’s not whitelisted
- **VPN Connection** - Our relay nodes are interconnected using VPN, and have WireGaurd installed from host to host. We opted for VPN instead of VPC peering so that we wouldn’t be restricted to having the relay nodes deployed and hosted on one single cloud platform
- **Private Linking** - Our validator nodes are connected with each others using Megaport, a software that enables direct and private connectivity between different environments to negate the use of the public internet

See below for a detailed overview of our architecture.

{{< image src="images/validator_architecture.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

### Key Management

{{< image src="images/key_management.png" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

We employ Tendermint KMS (tmkms) with YubiHSM2 for handling block signing and enforcing double-sign protection e.g. Proposal, PreVotes, PreCommits. This combination ensures high availability of the validator’s signing keys as well as the ability to survive a compromised node process or host machine. Similarly our account keys are safely stored in the Ledger Nano S hardware wallets.

### Monitoring and Alerting

- Leveraging our proprietary exporter [Cendermint](https://github.com/jim380/Cendermint), we harness the power of Grafana, Prometheus, and Alert Manager to deliver robust, time-series based monitoring and alerting
- As a secondary safeguard, we employ Uptime Kuma, ensuring our systems are always up and running
- For comprehensive infrastructure and application oversight, we operate a self-hosted Checkmk instance, providing us with advanced monitoring capabilities

### Additional Security Measures

#### Operating System Security

- Appropriately patched linux kernel version
- Security framework enabled (e.g. SELinux)
- GRUB boot loader configured with password
- Root permission enforced on system-level directories and files
- Enforced password policy

#### Remote Access

- Pluggable Authentication Modules (PAM) enabled
- SSH hardening (e.g. no root login, no X11Forwarding etc)
- SSH keys stored in hardware wallets

#### Networking

- Firewall, iptables configured with explicit rules
- External DDOS protection
- Intrusion detection system enabled with OSSEC and Fail2Ban
