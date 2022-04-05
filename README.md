### Overview
Automated DNS infrastructure setup with help of [Catalog Zones](https://datatracker.ietf.org/doc/html/draft-ietf-dnsop-dns-catalog-zones)
This is Ansible Playbook for a full setup of DNS infrastructure over multiple servers/regions for easy DNS zone management.
Main selling point of this setup is that DNS zone management happens automatically via usage of new DNS infrastructure feature called 'Catalog Zones'. Only interaction of admin user is via WEB interface, where new zones can be defined, old zones removed and records created or edited. DNSSEC can be enabled for a zone from administration interface.

### Components
Whole setup consist of following components open-source components:
* Backend Database - [PostgreSQL](https://www.postgresql.org/)
* Primary DNS server - [PowerDNS](https://www.powerdns.com/)
* Admin interface - [PowerDNS-Admin](https://github.com/PowerDNS-Admin/PowerDNS-Admin)
* Secondary DNS server - [Knot](https://www.knot-dns.cz/)

Once [NSD](https://www.nlnetlabs.nl/projects/nsd/about/) has catalog zone support, it will also be added to this setup in order to diversify public DNS server cluster, in case there is an issue with Knot software - whole infrastructure won't be impacted.

### High level overview:
```
            ┌─────────────┐      ┌────────────────┐        ┌───────────────────┐
            │ PostgreSQL  │      │ PowerDNS 4.4.x │        │PowerDNS-Admin     │
            │ DB backend  ├──────┤ zone catalog   │        │DNS administration │
            └─────────────┘      │ powered DNS    │        │WEB interface      │
                                 │ server         ◄────────┤                   │
                                 │                │        │                   │
                                 │                │        │                   │
                                 │                │        │                   │
                                 └───────┬────────┘        └───────────────────┘
                                         │
                                         │
                      Firewall           │
   ──────────────────────────────────────┼───────────────────────────────────── 
                                         │
                           AXFR-TSIG     │     AXFR-TSIG
         ┌───────────────────────────────┼─────────────────────────────┐
         │                               │                             │
         │                               │                             │
         │                               │                             │
         │                               │                             │
         │                               │                             │
    North│America                     Europe                         Asia
         │                               │                             │
 ┌───────▼───────┐               ┌───────▼────────┐           ┌────────▼──────┐
 │Knot DNS 3.1.x │               │ Knot DNS 3.1.x │           │ Knot DNS 3.1.x│
 │               │               │                │           │               │
 │               │               │                │           │               │
 │ ns1.example   │               │ ns2.example    │           │ ns3.example   │
 │               │               │                │           │               │
 └───────────────┘               └────────────────┘           └───────────────┘
``` 
 
 
