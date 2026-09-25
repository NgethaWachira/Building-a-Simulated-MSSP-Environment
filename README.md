# Building-a-Simulated-MSSP-Environment

## Objective
The objective of this project was to design and build a simulated Managed Security Service Provider (MSSP) environment from the ground up. I wanted hands-on experience with the multi-tenant Microsoft security stack, including Azure Lighthouse, Microsoft Sentinel, Microsoft Defender XDR and Entra ID Governance, the same tools real MSSPs use to protect multiple client organizations. Alongside the environment, I set out to create an AI-powered SOC automation pipeline using Claude. When a Sentinel incident fires, the pipeline runs KQL investigations, reaches an evidence-based verdict and escalates suspicious findings to a ticketing system for client follow-up. The project covered the full lifecycle of an MSSP engagement: onboarding a client tenant, setting up cross-tenant delegated access, tuning detection rules, enrolling devices into Intune and Defender for Endpoint, and building a scalable model for granting analysts response permissions. The broader aim was to deepen my practical understanding of how modern security operations work, and to explore how AI can support analysts in handling incidents more efficiently.

### Tools Used
<div>
  <img src="https://img.shields.io/badge/-Microsoft%20Azure-0078D4?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Microsoft Azure">
  <img src="https://img.shields.io/badge/-Microsoft%20Sentinel-8DD3C7?&style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Sentinel">
  <img src="https://img.shields.io/badge/-Microsoft%20Defender%20Portal-E81123?&style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Defender Portal">
  <img src="https://img.shields.io/badge/-Azure%20Log%20Analytics-1F77B4?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Log Analytics">
  <img src="https://img.shields.io/badge/-Azure%20Function%20App-FF7F0E?&style=for-the-badge&logo=azurefunctions&logoColor=white" alt="Azure Function App">
  <img src="https://img.shields.io/badge/-Azure%20App%20Service-2CA02C?&style=for-the-badge&logo=azure&logoColor=white" alt="Azure App Service">
  <img src="https://img.shields.io/badge/-Azure%20Logic%20Apps-D62728?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Logic Apps">
  <img src="https://img.shields.io/badge/-Azure%20Managed%20Identity-9467BD?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Managed Identity">
  <img src="https://img.shields.io/badge/-Azure%20Entra%20ID-8C564B?&style=for-the-badge&logo=microsoft&logoColor=white" alt="Entra ID">
  <img src="https://img.shields.io/badge/-Azure%20Key%20Vault-E377C2?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Key Vault">
  <img src="https://img.shields.io/badge/-Azure%20Application%20Insights-7F7F7F?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Application Insights">
  <img src="https://img.shields.io/badge/-Azure%20ARM%20API-BCBD22?&style=for-the-badge&logo=azuredevops&logoColor=white" alt="Azure Resource Manager API">
  <img src="https://img.shields.io/badge/-Anthropic%20API-AEC7E8?&style=for-the-badge&logo=anthropic&logoColor=white" alt="Anthropic API">
  <img src="https://img.shields.io/badge/-Python%203.11-FFDD57?&style=for-the-badge&logo=python&logoColor=black" alt="Python 3.11">
  <img src="https://img.shields.io/badge/-Azure%20Functions%20Core%20Tools-98DF8A?&style=for-the-badge&logo=azurefunctions&logoColor=white" alt="Azure Functions Core Tools">
  <img src="https://img.shields.io/badge/-Azure%20CLI%20(az)-C5B0D5?&style=for-the-badge&logo=azuredevops&logoColor=white" alt="Azure CLI">
  <img src="https://img.shields.io/badge/-Git-F7B6D2?&style=for-the-badge&logo=git&logoColor=white" alt="Git">
  <img src="https://img.shields.io/badge/-PowerShell-C49C94?&style=for-the-badge&logo=powershell&logoColor=white" alt="PowerShell">
  <img src="https://img.shields.io/badge/-Command%20Prompt%20(CMD-FB8072)?&style=for-the-badge&logo=windows&logoColor=white" alt="Command Prompt">
  <img src="https://img.shields.io/badge/-Azure%20DevOps-BEBADA?&style=for-the-badge&logo=azuredevops&logoColor=white" alt="Azure DevOps">
  <img src="https://img.shields.io/badge/-Azure%20Pipelines-FCCDE5?&style=for-the-badge&logo=azurepipelines&logoColor=white" alt="Azure Pipelines">
  <img src="https://img.shields.io/badge/-KQL%20(Query%20Language)-FDB462?&style=for-the-badge&logo=azuredataexplorer&logoColor=white" alt="KQL">
</div>

### SOC Automation Architecture
<hr style="border: 0; border-top: 1px solid #eee;">
<br>
<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/dbae9e838715b43c27fad3a1e43ab101d7ef5d6a/Images/camphor-clavis-soc-automation-architecture.png" width="300" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/dbae9e838715b43c27fad3a1e43ab101d7ef5d6a/Images/camphor-clavis-full-system-overview.png" width="345" />
</p>

## Steps
### Domain Registration and MSSP Foundation
- I registered the MSSP domain camphorclavis.com through Porkbun. At this stage, no web hosting or separate email hosting is required. The domain is primarily used for identity and Azure/Entra ID configuration, with Porkbun providing the required DNS management. The initial setup involves adding the TXT record provided by Entra ID to verify ownership of the domain. Later, if email services are required, Microsoft 365 can provide Exchange Online mailboxes using the verified domain, with the required MX, SPF, and DKIM records configured through Porkbun. Web hosting is optional and is not required for the MSSP lab.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7c13a36dc99858348b2d922175afad98630de39e/Images/1.png" width="700" />
</p>

### Creating a Microsoft account
- A Microsoft account, camphor.customer1@outlook.com, was created to represent Customer 1, using a separate browser profile and a Netherlands VPN endpoint to simulate an independent customer identity and sign-in location. Initial Azure portal sign-ins repeatedly failed with an MSAL state_mismatch error caused by browser session and cookie conflicts when using multiple Microsoft accounts.

### Raised a support ticket for a microsoft subscription
- Microsoft Support reviewed the account and removed the restriction that was preventing the creation of an Azure subscription. This allowed the Customer 1 account to complete the Azure signup process and provision its own Default Directory (tenant) and subscription. The subscription was initially empty, with no Azure resources deployed, providing a clean starting point for building the simulated customer environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/922cf992e16d4d729f938878a53e75dcd1439f4c/Images/6a.png" width="700" />
</p>

### Cost reality check
- We monitored the project costs to avoid unexpected charges. Sentinel ingestion was kept at a low level across the tenant, while Microsoft 365 E5 licenses were used only where needed. Budget alerts were configured in Azure Cost Management so we could receive notifications before costs increased significantly. Once the required testing was completed, unnecessary ingestion and licenses were paused rather than running continuously.  

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/8548bbb1506cff6b49917caa373a64dab2075365/Images/5.png" width="350" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/8548bbb1506cff6b49917caa373a64dab2075365/Images/6.png" width="350" />
</p>

### Consolidating to just Customer 1
- After encountering repeated Microsoft restrictions while creating additional customer tenants, we decided to consolidate the lab into Customer 1. The plan was to use this single tenant to host all required telemetry sources, including Microsoft Defender XDR, Cloud Apps, Office 365, Entra ID identity data, and Intune. This simplified the environment while still providing diverse data for testing the SOC automation and investigation pipeline.

### “No business presence” blocked all self-service purchasing
- A newly created Entra ID tenant (with no Microsoft 365 service ever purchased) could not be used to buy Microsoft 365 E5 directly — the retail storefront requires an existing commercial relationship (“business presence”) before it will sell into that tenant, but establishing that presence normally requires making a purchase, creating a circular blocker.
 
- Resolution: A small, self-service-eligible product (Microsoft 365 Business Basic, later superseded by an E3 trial) was purchased first specifically to establish business presence, after which the admin center’s internal purchase catalog exposed E5 as purchasable, resolving the deadlock.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/11.png" width="700" />
</p>

- The required Microsoft 365 E5 licenses were assigned with the recommended services enabled. After the licenses were assigned, the Microsoft Defender portal was opened and confirmed to be fully accessible, including Endpoints, Email & collaboration, Identities, Cloud Apps, and Sentinel integration. Entra ID was also checked to confirm that the E5 identity features were available. No additional configuration was needed at this stage.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/11.png" width="700" />
</p>

### Retail commerce silently created a second, unintended tenant
- The E3 trial flow created a new tenant instead of adding the service to the existing Customer 1 tenant. Because the new tenant was different, the existing Azure PAYG subscription was not initially available under the new tenant.
  
- The solution was to use Change directory to move the existing Azure subscription into the newly created tenant. This effectively consolidated the Azure subscription and Microsoft 365 business services into the new tenant, rather than creating another separate environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/12a.jpg" width="375" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/18.png" width="250" />
</p>

### Creating the Log Analytics workspace + Sentinel
- Next we created the Log Analytics workspace, configuring basic cost-control tags, and then enabling Microsoft Sentinel on that workspace. The final state was law-customer1, in resource group rg-customer1-soc, located in South Africa North, under the Camphor Customer 1 subscription. Once the workspace appeared in the Sentinel creation page, it was ready to have Sentinel enabled and configured.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/dc9b4b454ffd209e9fdb7baf3e469caf45eaf6bd/Images/24.png" width="700" />
</p>

### Setting the daily ingestion cap first
- Before enabling any Sentinel connectors, a daily ingestion limit was configured on the law-customer1 Log Analytics workspace to help control costs. The Data Cap setting was used to limit the amount of data that could be ingested each day, with 1 GB per day chosen as a suitable limit for the lab. Retention was left at the default 31 days, which was sufficient for active testing and investigation.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/dc9b4b454ffd209e9fdb7baf3e469caf45eaf6bd/Images/25.png" width="700" />
</p>

### Sentinel content hub and data sources
- Microsoft Sentinel's Content Hub was used to install the required solution packages for Microsoft Defender XDR, Microsoft Entra ID, and Microsoft 365. Content Hub provides packaged resources such as data connectors, analytics rules, workbooks, and hunting queries. After installing the Defender XDR solution, we connected Sentinel to different data sources to enable data ingestion. The actual cost comes from data ingestion, which was already controlled by the workspace's daily ingestion cap.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/dc9b4b454ffd209e9fdb7baf3e469caf45eaf6bd/Images/26.png" width="700" />
</p>

### Sentinel analytics rule
- After connecting the required data sources, we configured Microsoft Sentinel Analytics rules to detect suspicious activity and generate incidents. From the available rule templates, we enabled selected rules for Microsoft Defender XDR, Microsoft Entra ID, and Microsoft 365, including the Defender XDR rule that creates Sentinel incidents from Defender alerts, sign-ins to disabled accounts, anomalous or impossible-travel sign-ins, malicious inbox rules, unusual file activity, and changes to authentication methods for privileged accounts. These rules provided the initial detection baseline for the SOC automation pipeline.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/c15bc7d83fcf64d17ed8a292e47839865e5f9b11/Images/33.png" width="700" />
</p>

### Creating test users
- After enabling the six initial analytics rules, the next step was to create test users in the Customer 1 tenant. These accounts were created to generate realistic sign-in and user activity that the Sentinel analytics rules could monitor. The test users were created first, before signing in with them or generating additional activity, providing separate accounts for safely testing the detection and investigation process.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/34.png" width="700" />
</p>

### Creating and testing a malicious inbox rule
- To test the Malicious Inbox Rule detection, we created an inbox rule for the test account admin2@camphorcustomer1.onmicrosoft.com. We configured the rule to identify messages containing sensitive keywords and then forward them to the controlled external test address mesainenterprisesltd@gmail.com while moving the messages to a separate folder to hide them from the inbox.

- We then generated test activity by performing failed sign-ins followed by a successful sign-in. Afterward, we allowed time for the configured analytics rules and Defender detections to process the activity and generate any resulting alerts or incidents. We reviewed the SignInLogs data and confirmed that the Entra ID connector is collecting detailed sign-in information.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/35.png" width="700" />
</p>

### Azure lighthouse delegation setup
- The next step was to configure Azure Lighthouse so the MSSP tenant could access Customer 1’s Sentinel environment
#### Role definitions
- We identified the built-in Azure roles required for the MSSP to work with Customer 1’s Sentinel environment. These included Microsoft Sentinel Contributor, Microsoft Sentinel Responder, and Reader. These roles were included in the Lighthouse delegation so the MSSP could access and manage the required Sentinel resources.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/37.png" width="700" />
</p>

#### Group creation
- We then created a security group named Camphor Clavis SOC Analysts in Entra ID. We added the required MSSP analyst account to the group and copied the group’s Object ID from its Overview page. This Object ID was then used in the Lighthouse delegation template so access could be assigned to the group instead of an individual user.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/38.png" width="700" />
</p>

#### Custom deployment
- We signed into the Customer 1 tenant and opened Deploy a custom template in the Azure portal. We pasted the Lighthouse ARM template into the editor, replaced the placeholder with the Camphor Clavis SOC Analysts group Object ID, selected the Camphor Customer 1 subscription, and deployed the template. This published the Lighthouse delegation from Customer 1 to the MSSP tenant, allowing the assigned MSSP group to access the customer environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/40.png" width="390" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/42.png" width="315" />
</p>

### Lighthouse verification
- We signed into the MSSP tenant as mssptriagelabke@outlook.com and checked Service providers / My customers, but Customer 1 was not listed. We then signed back into the Customer 1 tenant and opened Subscriptions > Deployments > CustomDeployment-20260912132812 to verify both the registrationDefinition and registrationAssignment, which showed provisioningState: Succeeded. 

- We then returned to the MSSP tenant and used the direct Customer 1 subscription URL and Subscriptions page to confirm whether the delegated Camphor Customer 1 subscription and law-customer1 workspace were accessible.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/43.png" width="700" />
</p>

### Workspace Manager
- We went from the MSSP Sentinel workspace law-triage-lab to Configuration > Workspace Manager (Preview) and found that Workspace Manager was not yet enabled on the workspace. The page confirmed that at least two Sentinel workspaces, Microsoft Sentinel Contributor access on the central and member workspaces, and Azure Lighthouse for cross-tenant management were required.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/44.png" width="700" />
</p>

- Since the Add workspace button was grayed out, we followed “enable workspace manager in your Microsoft Sentinel settings” to enable Workspace Manager on law-triage-lab before adding law-customer1 as the member workspace.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/45.png" width="700" />
</p>

- We managed to add the law-customer1 workspace successfully to Workspace Manager, with the page confirming “1 Members.” We then returned to the Sentinel workspace view and confirmed that law-customer1 was now registered as a managed member workspace under law-triage-lab. This completed the Workspace Manager configuration needed to manage both the MSSP workspace and the Customer 1 workspace from the central environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/46.png" width="700" />
</p>

### Current MSSP environment status
- At this stage, we have successfully connected the MSSP tenant and Customer 1 through Azure Lighthouse. Microsoft Sentinel was enabled in both environments, with XDR, Entra ID, and Microsoft 365 connectors and analytics rules configured on the Customer 1 side.

- Test activity was also flowing into Sentinel, and law-customer1 has been successfully added to Workspace Manager under the MSSP workspace law-triage-lab, establishing the foundation for centralized management of the customer environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/47.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/48.png" width="700" />
</p>







