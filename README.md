## Introducing PEACH, a tenant isolation framework for cloud applications

Over the past year and a half, Wiz researchers and other members of the cloud security community discovered several cross-tenant vulnerabilities in various multi-tenant cloud applications (including [ExtraReplica](https://www.wiz.io/blog/wiz-research-discovers-extrareplica-cross-account-database-vulnerability-in-azure-postgresql) and [Hell’s Keychain](https://www.wiz.io/blog/hells-keychain-supply-chain-attack-in-ibm-cloud-databases-for-postgresql)). Each of these critical vulnerabilities could have potentially enabled malicious actors to access data belonging to any customer of the affected applications.

Although these issues have been reported on extensively and were dealt with appropriately by the relevant vendors, we’ve seen little public discussion on how to mitigate such vulnerabilities across the entire industry. The vast majority of modern SaaS and PaaS applications are multi-tenant, and anyone building or using these services should have an interest in solving this problem.

As time went by, we began noticing a problematic pattern:
1.	There is no common language in the industry to talk about best practices for tenant isolation, so each vendor ends up relying on different terminology and implementation standards for their security boundaries, making it difficult to assess their efficacy.
2.	There is no baseline for what measures vendors should be expected to take in order to ensure tenant isolation in their products, neither in terms of which boundaries they’re using or how they are actually implemented.
3.	There is no standard for transparency – while some vendors are very forthcoming about the details of their security boundaries, others share very little about them. This makes it harder for customers to manage the risks of using cloud applications.
This pattern led us to develop PEACH , with the goal of modelling tenant isolation in cloud applications, evaluating security posture, and outlining ways to improve it if necessary.

### Using PEACH

The first part of the security review process involves a tenant isolation review. This isolation review analyzes the risks associated with customer-facing interfaces and determines: 
1. the complexity of the interface as a predictor of vulnerability (see examples [here](/examples/interface-examples.md)); 
2. whether the interface is shared or duplicated per tenant; 
3. what type of security boundaries are in place (e.g. hardware virtualization - see other types [here](/specifications/security-boundaries.md)); 
4. how strongly these boundaries have been implemented.

In order to gauge how strongly the security boundaries have been implemented (4), we propose using the following five parameters (P.E.A.C.H. - defined in detail [here](/specifications/hardening-factors.md)):
1.	**P**rivilege hardening
2.	**E**ncryption hardening
3.	**A**uthentication hardening
4.	**C**onnectivity hardening
5.	**H**ygiene

The second part of the security review process consists of remediation steps to manage the risk of cross-tenant vulnerabilities and improve isolation as necessary. These includes reducing interface complexity,  enhancing tenant separation, and increasing interface duplication, all while accounting for operational context such as budget constraints, compliance requirements, and expected use-case characteristics of the service.

### Case study – ChaosDB

[ChaosDB](https://www.wiz.io/blog/chaosdb-explained-azures-cosmos-db-vulnerability-walkthrough) was a cross-tenant vulnerability in Azure Cosmos DB disclosed by Wiz in August 2021. The attack sequence consisted of deploying an embedded Jupyter Notebook, exploiting a local privilege escalation vulnerability, modifying firewall rules to gain unrestricted network access, authenticating to the CosmosDB backend, and abusing this access to retrieve and decrypt other tenants’ credentials.

By using the PEACH framework to model Cosmos DB’s initial state prior to  ChaosDB’s disclosure, we can conduct a root cause analysis of the vulnerability. To the best of our understanding, each tenant’s embedded Jupyter Notebook ran in a container nested within a virtual machine. Although this might appear to be a strong isolation scheme, the interface’s hardening factors revealed critical gaps at the implementation level:
1.	**Privilege** hardening gap – tenant-allocated VM with access to shared admin certificate.
2.	**Encryption** hardening gap – tenant API keys encrypted with shared key.
3.	**Authentication** hardening gap – self-signed certificate not validated.
4.	**Connectivity** hardening gap – network controls only enforced within container (iptables) and orchestrator interface accessible from tenant container.
5.	**Hygiene** gap – tenant access to unrelated certificates and keys.
 
<p align="center"><img align="center" src="https://github.com/wiz-sec/peach-framework/blob/adding-content/assets/chaosdb.png" alt="Estimated isolation scheme of Cosmos DB-embedded Jupyter Notebook at the time of ChaosDB’s discovery" class="center"></p>
<p align="center"><i>Estimated isolation scheme of Cosmos DB-embedded Jupyter Notebook at the time of ChaosDB’s discovery</i></p>

While this isolation scheme can ensure tenant isolation for relatively simple interfaces, it is ill-suited to highly complex ones such as Jupyter Notebook. In the case of ChaosDB, this complexity resulted in PEACH gaps that ultimately enabled our attack sequence to unfold.

### Getting started with PEACH

To find out more about the PEACH framework, check out the [PEACH website](peach.wiz.io) you to learn about principles for designing cloud applications with strong tenant isolation, and modelling your services against the threat of isolation escape. Additionally, you can see which questions to ask vendors to evaluate your security posture considering the risk of cross-tenant vulnerabilities. You may also read [our new whitepaper](), which takes a closer look at the PEACH framework while delving into prior work on the subject of tenant isolation.

### Just PEACHy

PEACH is based on the lessons we’ve learned over the course of our cloud vulnerability research, and we’re already using it internally at Wiz as part of our product design review process. We’ve been workshopping these ideas with various partners over the past few months and have decided that we’re ready to share them with the community as well.

We would be thrilled to receive your feedback so we can improve the framework and make it as useful as possible for cloud application developers – feel free to reach out to us directly or [create an issue](https://github.com/wiz-sec/peach-framework/issues/new) here in our GitHub repository.

### Acknowledgements
We would like to extend our gratitude to Christophe Parisel (Senior Cloud Security Architect, Société Générale), Cfir Cohen (Staff Software Engineer, Google), Kat Traxler (Principal Security Researcher, VectraAI), Srinath Kuruvadi (Head of Cloud Security, Netflix), Joseph Kjar (Senior Cloud Security Engineer, Netflix), Mike Kuhn (Managing Principal, Coalfire), Daniel Pittner (Software Architect, IBM Cloud), and Adam Callis (Information Security Architect, Cisco) for sharing constructive input throughout the development of this framework. We would also like to thank AWS for their review of our whitepaper and the valuable feedback they provided. We highly appreciate their willingness to help us identify tenant isolation best practices and their commitment to improving security transparency for cloud customers.

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
