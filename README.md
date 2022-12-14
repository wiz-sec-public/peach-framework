<p align="center"><img width="40%" align="center" src="https://github.com/wiz-sec/peach-framework/main/assets/peaches.png" class="center"></p>

## Introducing PEACH, a tenant isolation framework for cloud applications

Over the past year and a half, Wiz researchers and other members of the cloud security community discovered several cross-tenant vulnerabilities in various multi-tenant cloud applications (including [ChaosDB](https://www.wiz.io/blog/chaosdb-how-we-hacked-thousands-of-azure-customers-databases), [ExtraReplica](https://www.wiz.io/blog/wiz-research-discovers-extrareplica-cross-account-database-vulnerability-in-azure-postgresql) and [Hell’s Keychain](https://www.wiz.io/blog/hells-keychain-supply-chain-attack-in-ibm-cloud-databases-for-postgresql)). Each of these critical vulnerabilities could have potentially enabled malicious actors to access data belonging to any customer of the affected applications.

<p align="center"><img width="80%" align="center" src="https://github.com/wiz-sec/peach-framework/main/assets/attack-sequence.png" alt="Typical vulnerability-enabled cross-tenant attack sequence" class="center"></p>
<p align="center"><i>Typical vulnerability-enabled cross-tenant attack sequence</i></p>

Although these issues have been reported on extensively and were dealt with appropriately by the relevant vendors, we’ve seen little public discussion on how to mitigate such vulnerabilities across the entire industry. The vast majority of modern SaaS and PaaS applications are multi-tenant, and anyone building or using these services should have an interest in solving this problem.

In addition, something that stood out to us about each of these vulnerabilities was their root cause: **improperly implemented security boundaries**, usually compounded by otherwise harmless **bugs in customer-facing interfaces**.

As time went by, we began noticing a problematic pattern:
1.	There is no **common language** in the industry to talk about best practices for tenant isolation, so each vendor ends up relying on different terminology and implementation standards for their security boundaries, making it difficult to assess their efficacy.
2.	There is no **baseline** for what measures vendors should be expected to take in order to ensure tenant isolation in their products, neither in terms of which boundaries they’re using or how they are actually implemented.
3.	There is no standard for **transparency** – while some vendors are very forthcoming about the details of their security boundaries, others share very little about them. This makes it harder for customers to manage the risks of using cloud applications.

This led us to develop **PEACH**, with the goal of modelling tenant isolation in cloud applications, evaluating security posture, and outlining ways to improve it if necessary - all based on the lessons we've learned in our cloud vulnerability research.

### Using PEACH

The first part of the security review process involves a tenant isolation review (see example [here](/case-studies/chaosdb.md)). This isolation review analyzes the risks associated with customer-facing interfaces and determines: 
1. The **complexity of the interface** as a predictor of vulnerability (see examples [here](/examples/interface-examples.md)); 
2. Whether the interface is **shared or duplicated** per tenant; 
3. What type of **security boundaries** are in place (e.g. hardware virtualization - see other types [here](/specifications/security-boundaries.md)); 
4. How strongly these boundaries have been **implemented**.

In order to gauge how strongly the security boundaries have been implemented (4), we propose using the following five parameters (P.E.A.C.H. - defined in detail [here](/specifications/hardening-factors.md)):
1.	**P**rivilege hardening
2.	**E**ncryption hardening
3.	**A**uthentication hardening
4.	**C**onnectivity hardening
5.	**H**ygiene

The second part of the security review process consists of remediation steps to manage the risk of cross-tenant vulnerabilities and improve isolation as necessary. These includes reducing interface complexity,  enhancing tenant separation, and increasing interface duplication, all while accounting for operational context such as budget constraints, compliance requirements, and expected use-case characteristics of the service.

<p align="center"><img width="60%" align="center" src="https://github.com/wiz-sec/peach-framework/main/assets/review-process.png" alt="Isolation design review procedure" class="center"></p>
<p align="center"><i>Isolation design review procedure</i></p>

### Getting started with PEACH

To find out more about the PEACH framework, check out the [PEACH website](peach.wiz.io) you to learn about principles for designing cloud applications with strong tenant isolation, and modelling your services against the threat of isolation escape. Additionally, you can see which questions to ask vendors to evaluate your security posture considering the risk of cross-tenant vulnerabilities. You may also read [our new whitepaper](), which takes a closer look at the PEACH framework while delving into prior work on the subject of tenant isolation.

### Get involved

We would be thrilled to receive your feedback so we can improve the framework and make it as useful as possible for cloud application developers – feel free to reach out to us directly or [create an issue](https://github.com/wiz-sec/peach-framework/issues/new) here in our GitHub repository.

You can also contribute to this project by adding content, such as your own [case studies](/case-studies) of using the framework to analyze the isolation scheme of a cloud application or to determine the root cause of a cloud vulnerability.

### Acknowledgements
We would like to extend our gratitude to Christophe Parisel (Senior Cloud Security Architect, Société Générale), Cfir Cohen (Staff Software Engineer, Google), Kat Traxler (Principal Security Researcher, VectraAI), Srinath Kuruvadi (Head of Cloud Security, Netflix), Joseph Kjar (Senior Cloud Security Engineer, Netflix), Mike Kuhn (Managing Principal, Coalfire), Daniel Pittner (Software Architect, IBM Cloud), and Adam Callis (Information Security Architect, Cisco) for sharing constructive input throughout the development of this framework.

We would also like to thank AWS for their review of our whitepaper and the valuable feedback they provided. We highly appreciate their willingness to help us identify tenant isolation best practices and their commitment to improving security transparency for cloud customers.

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
