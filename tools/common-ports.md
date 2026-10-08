---
name: Common Ports
category: Cheatsheets & Playbooks
description: Common TCP/UDP ports, healthcare protocols, SCADA ports and TTL fingerprinting reference.
tags: [ports, tcp, udp, scada, healthcare, networking, RTFM]
---

# Common Ports

Quick reference for common service ports, healthcare protocols, SCADA ports and TTL fingerprinting.

## Common ports

Proto shows the transport the service actually uses. `/etc/services` (IANA) registers most ports for both TCP and UDP even when only one is used. Look one up with `getent services 3306/tcp` or `grep -w 3306 /etc/services`.

| Port | Proto | Service |
|------|-------|---------|
| 7 | TCP/UDP | Echo |
| 9 | TCP/UDP | Discard |
| 13 | TCP/UDP | Daytime |
| 17 | TCP/UDP | QOTD |
| 19 | TCP/UDP | Chargen (UDP amplification) |
| 20 | TCP | FTP (Data) |
| 21 | TCP | FTP (Control) |
| 22 | TCP | SSH/SCP/SFTP |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 43 | TCP | WHOIS |
| 49 | TCP | TACACS+ |
| 53 | TCP/UDP | DNS (TCP for zone transfers and large replies) |
| 67-68 | UDP | DHCP/BOOTP (server/client) |
| 69 | UDP | TFTP |
| 70 | TCP | Gopher |
| 79 | TCP | Finger |
| 80 | TCP | HTTP |
| 88 | TCP/UDP | Kerberos |
| 110 | TCP | POP3 |
| 111 | TCP/UDP | RPC portmapper |
| 113 | TCP | Ident |
| 119 | TCP | NNTP |
| 123 | UDP | NTP |
| 135 | TCP | Windows RPC endpoint mapper |
| 137 | UDP | NetBIOS Name Service |
| 138 | UDP | NetBIOS Datagram |
| 139 | TCP | NetBIOS Session (SMB) |
| 143 | TCP | IMAP4 |
| 161 | UDP | SNMP |
| 162 | UDP | SNMP Trap |
| 179 | TCP | BGP |
| 319-320 | UDP | PTP (event/general) |
| 389 | TCP/UDP | LDAP (UDP = CLDAP) |
| 443 | TCP/UDP | HTTPS (UDP = HTTP/3 QUIC) |
| 445 | TCP | SMB |
| 464 | TCP/UDP | Kerberos kpasswd |
| 465 | TCP | SMTPS (implicit TLS) |
| 500 | UDP | ISAKMP/IKE |
| 512 | TCP | rexec |
| 513 | TCP | rlogin |
| 514 | TCP | rsh |
| 514 | UDP | Syslog |
| 515 | TCP | LPD |
| 520 | UDP | RIP |
| 521 | UDP | RIPng |
| 543-544 | TCP | Kerberos rlogin/rsh (klogin/kshell) |
| 546-547 | UDP | DHCPv6 (client/server) |
| 548 | TCP | AFP |
| 554 | TCP | RTSP |
| 563 | TCP | NNTPS |
| 587 | TCP | SMTP Submission (STARTTLS) |
| 593 | TCP | RPC over HTTP |
| 623 | UDP | IPMI |
| 631 | TCP | IPP/CUPS |
| 636 | TCP | LDAPS |
| 646 | TCP/UDP | MPLS LDP |
| 749 | TCP | Kerberos admin |
| 853 | TCP/UDP | DNS over TLS / DNS over QUIC |
| 873 | TCP | rsync |
| 902 | TCP | VMware ESXi |
| 989-990 | TCP | FTPS (Data/Control) |
| 992 | TCP | Telnet over TLS |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |
| 1080 | TCP | SOCKS proxy |
| 1099 | TCP | Java RMI registry |
| 1194 | UDP/TCP | OpenVPN (UDP default) |
| 1433 | TCP | MS-SQL |
| 1434 | UDP | MS-SQL Browser |
| 1521 | TCP | Oracle |
| 1701 | UDP | L2TP |
| 1720 | TCP | H.323 |
| 1723 | TCP | PPTP (+ GRE) |
| 1812-1813 | UDP | RADIUS (auth/accounting) |
| 1883 | TCP | MQTT |
| 1900 | UDP | SSDP/UPnP |
| 1985 | UDP | HSRP |
| 2049 | TCP/UDP | NFS |
| 2055 | UDP | NetFlow |
| 2375-2376 | TCP | Docker API (HTTP/TLS) |
| 2379-2380 | TCP | etcd (client/peer) |
| 2401 | TCP | CVS pserver |
| 3128 | TCP | Squid proxy |
| 3260 | TCP | iSCSI |
| 3268-3269 | TCP | LDAP Global Catalog (/SSL) |
| 3306 | TCP | MySQL/MariaDB |
| 3389 | TCP/UDP | RDP |
| 3478 | TCP/UDP | STUN/TURN |
| 3632 | TCP | distcc |
| 3690 | TCP | SVN |
| 3702 | UDP | WS-Discovery |
| 4369 | TCP | Erlang EPMD |
| 4444 | TCP | Metasploit default listener |
| 4500 | UDP | IPsec NAT-T |
| 4569 | UDP | IAX2 (Asterisk) |
| 4786 | TCP | Cisco Smart Install |
| 4789 | UDP | VXLAN |
| 5060 | TCP/UDP | SIP |
| 5061 | TCP | SIPS |
| 5222 | TCP | XMPP client |
| 5269 | TCP | XMPP server |
| 5353 | UDP | mDNS |
| 5355 | UDP | LLMNR |
| 5432 | TCP | Postgres |
| 5671-5672 | TCP | AMQP (TLS/plain) |
| 5683 | UDP | CoAP |
| 5800 | TCP | VNC over HTTP |
| 5900 | TCP | VNC |
| 5985-5986 | TCP | WinRM (HTTP/HTTPS) |
| 6000-6063 | TCP | X11 |
| 6379 | TCP | Redis |
| 6443 | TCP | Kubernetes API |
| 6514 | TCP | Syslog over TLS |
| 6665-6669 | TCP | IRC |
| 6697 | TCP | IRC over TLS |
| 7001 | TCP | WebLogic |
| 8009 | TCP | AJP (Tomcat) |
| 8080 | TCP | HTTP alt / proxy |
| 8443 | TCP | HTTPS alt |
| 8883 | TCP | MQTT over TLS |
| 9001 | TCP | Tor |
| 9042 | TCP | Cassandra |
| 9050 | TCP | Tor SOCKS |
| 9100 | TCP | JetDirect (raw printing) |
| 9200, 9300 | TCP | Elasticsearch (HTTP/transport) |
| 9389 | TCP | AD Web Services |
| 9418 | TCP | Git protocol |
| 10000 | TCP | Webmin |
| 10250 | TCP | Kubelet API |
| 11211 | TCP/UDP | Memcached (UDP amplification) |
| 11371 | TCP | HKP (PGP keyserver) |
| 27017-27018 | TCP | MongoDB |
| 51820 | UDP | WireGuard |

## Application default ports

Defaults only — admins often change them. Ports already listed above (Redis, MongoDB, Elasticsearch, etc.) are not repeated.

### Web & dev frameworks

| Port | Proto | Application |
|------|-------|-------------|
| 3000 | TCP | Node/Express, Rails, Next.js, Grafana, Gitea |
| 4000 | TCP | GraphQL (Apollo Server) — GraphQL has no own port, usually `/graphql` on the app's HTTP port |
| 4200 | TCP | Angular dev server |
| 4848 | TCP | GlassFish admin |
| 5000 | TCP | Flask, Docker Registry |
| 5173 | TCP | Vite dev server |
| 8000 | TCP | Django, Splunk Web |
| 8005 | TCP | Tomcat shutdown |
| 8080 | TCP | Tomcat, Jenkins, Spring Boot, Hasura GraphQL |
| 8888 | TCP | Jupyter Notebook |
| 9990 | TCP | WildFly/JBoss admin |

### Databases & caches

| Port | Proto | Application |
|------|-------|-------------|
| 5984 | TCP | CouchDB |
| 7474, 7687 | TCP | Neo4j (HTTP/Bolt) |
| 8086 | TCP | InfluxDB |
| 8091 | TCP | Couchbase |
| 8123 | TCP | ClickHouse (HTTP) |
| 8983 | TCP | Apache Solr |
| 9000 | TCP | ClickHouse (native), MinIO API, SonarQube, Portainer |
| 16379 | TCP | Redis Cluster bus |
| 26257 | TCP | CockroachDB |
| 26379 | TCP | Redis Sentinel |

### Messaging & streaming

| Port | Proto | Application |
|------|-------|-------------|
| 2181 | TCP | ZooKeeper |
| 4222 | TCP | NATS |
| 9092 | TCP | Kafka |
| 15672 | TCP | RabbitMQ management |

### DevOps, CI & orchestration

| Port | Proto | Application |
|------|-------|-------------|
| 2377 | TCP | Docker Swarm management |
| 4505-4506 | TCP | SaltStack |
| 4646 | TCP | Nomad |
| 7946 | TCP/UDP | Docker Swarm node discovery |
| 8081 | TCP | Nexus, Artifactory |
| 8140 | TCP | Puppet |
| 8200 | TCP | HashiCorp Vault |
| 8500 | TCP | Consul |
| 10255 | TCP | Kubelet read-only |
| 30000-32767 | TCP/UDP | Kubernetes NodePort range |
| 50000 | TCP | Jenkins agent |

### Monitoring & logging

| Port | Proto | Application |
|------|-------|-------------|
| 5044 | TCP | Logstash (Beats input) |
| 5601 | TCP | Kibana |
| 5666 | TCP | Nagios NRPE |
| 8089 | TCP | Splunk management |
| 8125 | UDP | StatsD |
| 9090 | TCP | Prometheus |
| 9093 | TCP | Alertmanager |
| 9100 | TCP | Prometheus Node Exporter |
| 9997 | TCP | Splunk forwarder |
| 10050-10051 | TCP | Zabbix agent/server |
| 12201 | TCP/UDP | Graylog GELF |

### Big data & AI

| Port | Proto | Application |
|------|-------|-------------|
| 7077 | TCP | Spark master |
| 8088 | TCP | Hadoop YARN |
| 9870 | TCP | Hadoop NameNode UI (50070 on v2) |
| 11434 | TCP | Ollama |

### Debug & test interfaces

| Port | Proto | Application |
|------|-------|-------------|
| 1025, 8025 | TCP | MailHog (SMTP/web) |
| 4444 | TCP | Selenium Grid |
| 5005 | TCP | Java JDWP (common) |
| 9222 | TCP | Chrome DevTools remote debugging |
| 9229 | TCP | Node.js inspector |
| 9901 | TCP | Envoy admin |

## Healthcare ports

| Port | Proto | Service |
|------|-------|---------|
| 104 | TCP | DICOM |
| 2575 | TCP | HL7 (MLLP) |
| 11112 | TCP | DICOM (alt, non-privileged) |

## SCADA ports

| Port | Proto | Service |
|------|-------|---------|
| 102 | TCP | ICCP / Siemens S7 (ISO-TSAP) |
| 502 | TCP | Modbus TCP |
| 1089-1091 | TCP/UDP | Foundation Fieldbus HSE |
| 2222 | UDP | EtherNet/IP (I/O) |
| 4000 | TCP/UDP | ROC Plus |
| 4840 | TCP | OPC UA Discovery |
| 20000 | TCP/UDP | DNP3 |
| 34962-34964 | TCP/UDP | PROFINET |
| 44818 | TCP/UDP | EtherNet/IP (explicit) |
| 47808 | UDP | BACnet/IP |

## TTL fingerprinting

| TTL | OS |
|-----|-----|
| 128 | Windows |
| 64 | Linux |
| 255 | Network / Solaris |

> Source: RTFM — Red Team Field Manual v2
