\# Task 4 - Part A

\# Reconnaissance Report



\## Target



dodopayments.tech





\## Methodology



Passive reconnaissance was performed.



Sources used:



\- crt.sh

\- Certificate Transparency Logs

\- DNS Enumeration



No active scanning or exploitation was performed.





\---



\# Discovered Assets



| Subdomain | Observation |

|-----------|-------------|

| pinacolada.dodopayments.tech | Public certificate found |

| ozone.dodopayments.tech | Public certificate found |

| sequin.dodopayments.tech | Public certificate found |

| plunk-v2.dodopayments.tech | Email service |

| api-plunk-v2.dodopayments.tech | API endpoint |

| app-plunk-v2.dodopayments.tech | Application endpoint |

| docs-plunk-v2.dodopayments.tech | Documentation endpoint |

| partner-api.dodopayments.tech | API endpoint |

| prowler.infra.dodopayments.tech | Security assessment service |

| prowler-api.infra.dodopayments.tech | API service |

| sentry.dodopayments.tech | Error monitoring |

| sentry-prod.infra.dodopayments.tech | Production monitoring |

| n8n.dodopayments.tech | Workflow automation |

| ozone-v2.dodopayments.tech | Application endpoint |

| signoz-prod.infra.dodopayments.tech | Observability platform |

| kafka-ui-prod.infra.dodopayments.tech | Kafka dashboard |

| kafka-ui-dev.infra.dodopayments.tech | Development dashboard |





\---



\# Security Observations



Certificate Transparency logs revealed multiple publicly visible subdomains.



The discovered naming patterns expose internal technology information including:



\- Kafka

\- Sentry

\- SigNoz

\- n8n

\- Prowler



Public discovery of infrastructure assets should be reviewed to ensure that only intended services are exposed.





\---



\# Scope



Assessment was limited to passive reconnaissance.



No vulnerability scanning, exploitation, or intrusive testing was performed.

