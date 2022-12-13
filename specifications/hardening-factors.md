## Privilege Hardening
1.	Tenants and hosts generally have minimal permissions in the service environment, thereby adhering to the principle of least privilege.
2.	In particular, each tenant is not authorized to read or write to other tenants’ data unless explicitly approved by the tenants involved, and each host does not have read or write access to other hosts.
3.	Privileges are verified prior to execution of operations.

## Encryption Hardening
1.	Data owned by and related to each tenant – at-rest and in-transit – is encrypted with a key unique to that tenant, regardless of architecture.
2.	Logs pertaining to each tenant’s activity are encrypted by a secret shared only between the tenant and the control plane .

## Authentication Hardening
1.	Communications between each tenant and the control plane (in both directions) use authentication with a key or certificate unique to each tenant.
2.	Authentication keys are validated, and self-signed keys are blocked.

## Connectivity Hardening 
1.	All inter-host connectivity is blocked by default unless explicitly approved by the tenants involved (so as to facilitate database replication, for example); hosts cannot connect to other hosts in the service environment, except those used by the control plane (i.e., a hub-and-spoke configuration).
2.	Hosts do not accept incoming connection requests from other hosts unless explicitly approved by the tenants involved, except those used by the control plane (in anticipation of scenarios in which an attacker manages to overcome connectivity limitations on their own compromised host).
3.	Tenants cannot arbitrarily access any external resource (both within the service environment and on the Internet) and are limited to communicating with only pre-approved resources or those explicitly approved by the tenant.

## Hygiene
Unnecessary data scattered throughout the environment might serve as clues or quick wins for malicious actors, particularly those that have managed to breach one or more security boundaries, which in turn enable further reconnaissance and lateral movement. Therefore, vendors can eliminate the following types of data at the design level and also regularly scan for forgotten artifacts:
1.	Secrets – The interface and datastore (and underlying host, in the case of virtualization or containerization) do not contain any keys or credentials that would allow authentication to other tenants’ environments or decryption of other tenants’ backend communications or logs.
2.	Software – The instance and datastore (and underlying host) do not contain any built-in software or source code that could enable reconnaissance or lateral movement.
3.	Logs – Each tenant’s logs are hidden from and inaccessible by other tenants; logs accessible to each tenant do not contain any information pertaining to other tenants’ activity.
