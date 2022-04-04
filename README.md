# catalog-zone-dns-setup
Automated DNS infrastructure setup with Catalog Zones
This is Ansible Playbook to setup a full blown DNS infrastructure over multiple servers/regions
for easy DNS zone management.
Main selling point of this setup is that DNS zone management happens automatically via usage of 
new DNS infrastructure feature called 'Catalog Zones'. Only interaction of admin user is via web interface, where new zones can be defined, old removed and DNS records edited, created. DNSSEC support is also enabled with a single click of a button from administration interface. All the changes then are syncrhonized with primary public DNS servers in various hosting zones for maximum availability. 

High level overview:
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
 
 
