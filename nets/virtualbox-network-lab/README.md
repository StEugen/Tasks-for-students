# Laboratory: Observe, Enumerate, and Defend a VirtualBox Network

## Goal

Build a small isolated network, observe normal traffic, enumerate services from another host,
and apply a host firewall. This laboratory connects networking theory to practical red-team and
blue-team work without touching networks outside the lab.

## Learning outcomes

After completing the laboratory, you should be able to:

- distinguish NAT, bridged, host-only, and internal VirtualBox networking;
- configure and verify static IPv4 addresses;
- explain the roles of ARP, ICMP, TCP, UDP, ports, routes, and listening sockets;
- capture and interpret packets with `tcpdump`;
- enumerate only an authorized lab host with `nmap`;
- restrict a service with a Linux firewall and prove that the control works.

## Topology

Create three Linux virtual machines. Debian is recommended; Kali may be used for the analyst VM.

| VM | Role | Internal address | Hostname |
|---|---|---:|---|
| VM 1 | client | `10.10.10.11/24` | `client` |
| VM 2 | server | `10.10.10.20/24` | `server` |
| VM 3 | analyst | `10.10.10.30/24` | `analyst` |

Each VM should have:

- Adapter 1: NAT, used only for package installation and updates;
- Adapter 2: Internal Network, named `sec-lab`;
- no default gateway configured on the internal interface.

```text
                 VirtualBox Internal Network: sec-lab

 client                 server                  analyst
10.10.10.11  <------> 10.10.10.20 <--------> 10.10.10.30
   NAT                     NAT                     NAT
    |                       |                       |
 package updates only; do not expose lab services through NAT forwarding
```


## Prerequisites

Install these tools on the appropriate VMs:

- all VMs: `iproute2`, `iputils-ping`, `curl`;
- server: `python3`, `nftables`, `tcpdump`;
- analyst: `nmap`, `tcpdump`.

Ask the teacher before installing packages. Keep a record of what was installed.

## Part 1 — Build and verify the network

1. Create or clone the three VMs and take a snapshot named `before-network-lab`.
2. Configure Adapter 2 on every VM as Internal Network `sec-lab`.
3. Assign the addresses from the topology table.
4. On every VM, collect:

```bash
hostname
ip -brief address
ip route
```

5. From `client`, test reachability to `server` and `analyst`.
6. Display the neighbor table before and after a ping:

```bash
ip neigh
```

### Questions

1. Which interface belongs to NAT and which belongs to `sec-lab`? What proves it?
2. Why is no gateway required for communication inside `10.10.10.0/24`?
3. What changed in the neighbor table after the ping?
4. What would Bridged Networking change, and why is it a poor default for this lab?

## Part 2 — Observe packets

On `server`, identify the internal interface and capture only ARP and ICMP traffic on it. Use the
`tcpdump` manual to determine the correct interface and capture expression.

While capture is active:

1. ping `server` from `client`;
2. stop the capture safely with `Ctrl-C`;
3. repeat the capture and save it as `icmp-arp.pcap`;
4. inspect the saved file with `tcpdump -r` or Wireshark.

### Evidence to collect

- the ARP request and reply;
- source and destination MAC addresses;
- one ICMP echo request and reply;
- source and destination IP addresses;
- packet count shown when `tcpdump` exits.

Explain what each packet did. Do not merely paste output.

## Part 3 — Create and enumerate a service

On `server`, create a directory containing a harmless `index.html`, then run a temporary HTTP service
bound specifically to `10.10.10.20` on TCP port `8000`. Python's `http.server` module is sufficient.

1. Prove locally that the socket is listening using `ss`.
2. Request the page from `client` with `curl`.
3. From `analyst`, perform these authorized checks against **only `10.10.10.20`**:
   - host discovery;
   - a scan of TCP ports `22` and `8000`;
   - service/version detection only for those ports.
4. Capture the connection on `server` and identify the TCP three-way handshake.
5. Stop the temporary HTTP server when this part is complete.

### Questions

1. What is the difference between a listening socket and an established connection?
2. Which packets form the TCP three-way handshake?
3. How did the scan look different from the normal HTTP request in the packet capture?
4. Why can service/version detection be useful to both attackers and defenders?

## Part 4 — Defend the service

Use `nftables` on `server`. Before editing rules, export the current ruleset and confirm that you still
have VirtualBox console access. A firewall typo should become a lesson, not a reinstall.

Create a minimal policy that:

- permits loopback traffic;
- permits established and related traffic;
- permits ICMP from the lab subnet;
- permits TCP port `8000` only from `client` (`10.10.10.11`);
- preserves the teacher-approved SSH access path, if SSH is in use;
- rejects or drops other inbound traffic from the internal network.

Test and record results:

| Test | Expected result |
|---|---|
| `client` → server TCP/8000 | allowed |
| `analyst` → server TCP/8000 | blocked or rejected |
| client ping → server | allowed |
| server outbound update traffic through NAT | still functional |

Then inspect firewall counters and explain which rules matched. Restore the original ruleset at the end
unless the teacher asks you to keep the new policy.

## Optional challenge — UDP is not small TCP

On `server`, create a temporary UDP listener with a teacher-approved tool. Send a datagram from
`client`, capture it, and compare the exchange with TCP. Explain why a UDP scan is harder to interpret.

## Security assessment report

Use the shared [security laboratory report template](../../templates/security-lab-report-template.md).

Required report content:

1. define the three VMs, subnet, permitted scan targets and ports, prohibited actions, and test window;
2. include the final topology and addressing table in the Environment section;
3. register the packet captures, scan output, service-listener output, and firewall ruleset as evidence;
4. document ARP/ICMP behavior and the TCP handshake as technical observations;
5. report exposed services and ineffective access restrictions as findings only when the evidence supports them;
6. explain **attacker visibility** and **defender visibility** for discovery, connection, and scan traffic;
7. record firewall implementation in the activity timeline;
8. present the before/after access matrix as a retest, including expected and actual results;
9. give specific recommendations and note the limitations of a small VirtualBox laboratory.

At minimum, the evidence directory should contain:

```text
evidence/
├── E-001-topology.txt
├── E-002-icmp-arp.pcap
├── E-003-tcp-handshake.pcap
├── E-004-service-scan.txt
├── E-005-listening-sockets.txt
├── E-006-firewall-before.txt
├── E-007-firewall-after.txt
└── E-008-retest-matrix.md
```

File names may differ, but evidence IDs in the report and filenames must agree. Answers to laboratory
questions should appear as supported observations, findings, or appendix notes—not as an isolated
question-and-answer dump.

## Completion criteria

- all three VMs communicate on the isolated subnet;
- You can explain, not just reproduce, ARP and the TCP handshake;
- enumeration remains inside the assigned target and port scope;
- the firewall permits the client while blocking the analyst from TCP/8000;
- temporary services are stopped and evidence is organized;
- the report separates confirmed findings from technical observations and unsupported assumptions;
- every security claim references evidence and every control change includes a retest result.
