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
