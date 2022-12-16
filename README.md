# Home Lab Bootstrap Builder

Infrastructure and tools to help me build/rebuild my home lab. Trying to get my
lab infrastructure to be immutable, and configuration-as-code driven.

## Design objectives

1. Lab systems and network operates as a sub-domain of my home network
2. Lab will have it's own "router" to provide DNS/DHCP/ROUTING for
   lab machines
3. The lab ingress router must be immutable.
4. Lab router machine will also act as bootstrap machine for other hosts
5. Bootstrap-the-bootstrap approach

## Todo

- [ ] Build bootstrap-bootstrap KVM machine
- [ ] Template-based boot server configuration
