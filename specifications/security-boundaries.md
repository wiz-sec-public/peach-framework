# Primary Boundaries

## Hardware Separation
Wherein instances belonging to different tenants run on separate physical machines (IaaS such as bare-metal servers or dedicated physical datacenters). This is the highest practical level of isolation but is not considered cost-effective at scale, nor is the added level of isolation significant in comparison to the use of industry-vetted hardware virtualization (this becomes especially apparent in cases where hardware separation has been insufficiently hardened).

## Hardware Virtualization 
Wherein instances belonging to different tenants run in separate virtual machines running on shared hardware. This is generally considered a high level of isolation.

## Containerization
Wherein instances belonging to different tenants run in separate containers running on shared hardware (or nested within a shared virtual machine) via operating-system-level virtualization. Containers are not considered an especially effective security boundary on their own, and therefore this should generally be treated as a low level of isolation.

When using containerization as a security boundary, vendors should enforce permission restrictions on deployed workloads , via technologies such as SECCOMP , AppArmor  and gVisor , thereby limiting the capacity of a compromised container from affecting its host. Additional mitigations may include implementing a read-only filesystem, limiting the privileges of user accounts, disabling the root user, and minimizing the number of running highly privileged processes. Moreover, vendors should generally prefer the use of hardened operating systems.

## Data Segmentation
Wherein data belonging to different tenants is stored and/or processed on a shared storage component (virtual or physical), but unique keys are used to encrypt and/or authorize access to each tenant’s data. This is generally considered a low level of isolation, as it’s highly dependent on the implementation of other aspects of the architecture (such as key storage security).

# Secondary Boundaries

## Network Segmentation 
Wherein instances belonging to different tenants run on separate machines (virtual or physical) in the same network, which is segmented into single tenant VPNs or subnets (logically separated via encapsulation, encryption, and firewalling). This is generally considered a high level of isolation.

## Identity Segmentation
Wherein roles are assigned to different tenants (whether or not customers can access them directly under normal circumstances) which are limited by deny-by-default policies, such that each tenant cannot perform any actions affecting resources belonging to any other tenant . This is generally considered a high level of isolation.
