# FREEBSD with ZFS, CBSD and reggae

## What is a ZFS?

The Z File System, or ZFS, is an advanced file system designed to overcome many of the major problems found in previous designs.

Originally developed at Sun™, ongoing open source ZFS development has moved to the OpenZFS Project.

ZFS has three major design goals:

*    Data integrity: All data includes a checksum of the data. When data is written, the checksum is calculated and written along with it. When that data is later read back, the checksum is calculated again. If the checksums do not match, a data error has been detected. ZFS will attempt to automatically correct errors when data redundancy is available.

 *   Pooled storage: physical storage devices are added to a pool, and storage space is allocated from that shared pool. Space is available to all file systems, and can be increased by adding new storage devices to the pool.

  *  Performance: multiple caching mechanisms provide increased performance. ARC is an advanced memory-based read cache. A second level of disk-based read cache can be added with L2ARC, and disk-based synchronous write cache is available with ZIL.
 
READ more information on [Freebsd ZFS](https://www.freebsd.org/doc/handbook/zfs.html)

## What is CBSD?

CBSD is a management layer written for the FreeBSD jai subsystem, bhyve and Xen. 
The project is positioned as a single integrated tool of comprehensive solution for building and deploying virtual environments quickly with pre-defined software sets with minimal configuration.

Read more here [CBSD.io](https://cbsd.io)

## How user tool reggae?

### Initialization

Install Reggae from ports or pkg:
```
make -C /usr/ports/sysutils/reggae install
```
or
```
pkg install -y reggae
```
Initialize PF, CBSD and ZFS dataset:

```reggae network-init```  # it sets up PF, Unbound and SSH, which you should (re)start

```reggae cbsd-init```

```reggae master-init```   # creates resolver and dhcp jails

In most cases, the default config is acceptable, but if you want to change something `/usr/local/etc/reggae.conf` is the right place

### Create service

Create simple service

```
mkdir myservice
cd myservice
reggae init
make
```
Create service with shell and ansible provisioners

```
mkdir myservice
cd myservice
reggae init shell ansible
make
```
### Create project

Project consists of multiple services

```
mkdir myproject
cd myproject
reggae project-init
```
Resulting Makefile is this:
```
REGGAE_PATH = /usr/local/share/reggae
#SERVICES = consul https://github.com/mekanix/jail-consul \
#      letsencrypt https://github.com/mekanix/jail-letsencrypt \
#      ldap https://github.com/mekanix/jail-ldap \
#      mail https://github.com/mekanix/jail-mail \
#      jabber https://github.com/mekanix/jail-jabber \
#      webmail https://github.com/mekanix/jail-webmail \
#      web https://github.com/mekanix/jail-web \
#      webconsul https://github.com/mekanix/jail-webconsul

.include <${REGGAE_PATH}/mk/project.mk>
```
Every service that is commented out is created using reggae init and edited to add extra functionality.
