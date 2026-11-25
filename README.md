\# AttackSurface-Lab



\## Objective

Demonstrate how running services expose ports, and verify that

hardening removes that exposure, using Nmap on WSL2 Ubuntu.



\## Environment

\- Windows 11 with WSL2 (Ubuntu)

\- Nmap 7.98

\- Python 3 http.server as the test service



\## Methodology

1\. Baseline scan with no services running

2\. Started a web server on port 8080 and rescanned

3\. Stopped the server and rescanned to verify remediation



\## Results

| Scan | State | Open ports |

|------|-------|-----------|

| 01\_baseline\_closed | Nothing running | 0 |

| 02\_web\_server\_open | Web server running | 1 (8080/tcp) |

| 03\_after\_hardening | Server stopped | 0 |



\## Findings

\- Starting a service immediately created an open port.

\- Nmap labeled port 8080 as "http-proxy" from the port number, but

&#x20; -sV identified the service as: Python http.server

\- The server logged a "connection reset" error during the scan,

&#x20; showing that scanning activity is detectable.

\- Stopping SSH with `service ssh stop` did not close port 22, because

&#x20; ssh.socket kept listening. Both the socket and the service had to

&#x20; be stopped.



\## Lessons learned

\- Always verify changes with a rescan instead of assuming.

\- A service can stay reachable through socket activation.

\- Stopping SSH not close the port, we need to stop the socket as well. 



\## Mapping to Cisco Intro to Cybersecurity

Attack surface, vulnerability identification, mitigation, and

verification.



\## Limitations

Scans were run against localhost only. The next phase will scan

between separate VMs to simulate a real network.

