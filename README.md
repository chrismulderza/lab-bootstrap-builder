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

## Building

### Containers

The infrastructure makes use of a number of conatiners to provide services
and build configurations.

#### Centos 9 Stream with `systemd` (centos-9-init)

To avoid having to build multiple container that will each just host a single
service, I've opted to build a Centos 9 Stream container with `systemd` to 
supervise services such as DNS/DHCP etc.

The Podman `Containerfile` for this container is in `containers/Containerfile.centos-init`
and can be built using:

```bash
podman build -t centos-9-init -f containers/Containerfile.centos-init
```

##### (Optional) Speed up container builds

To speed up the container build you can cache some of the `dnf` metadata:

1. At the top of the source tree create a cache directory

```bash
mkdir -p cache/centos/9/dnf
```

2. Prime the cache by adding the directory as a volume to a Centos container
   and downloading `dnf` metadata

```bash
podman run -it --name cacheprimer --rm -v $(pwd)/cache/centos/9/dnf:/var/cache/dnf:z quay.io/centos/centos:stream9 /bin/bash -c "dnf -y install epel-release && dnf makecache"
```

3. You can now mount the cache directory into a build as on overlay mount:

```bash
podman build -v $(pwd)/cache/centos/9/dnf:/var/cache/dnf:O -t centos-9-init -f containers/Containerfile.centos-init
```

## Todo

- [ ] Build bootstrap-bootstrap KVM machine
- [ ] Template-based boot server configuration
