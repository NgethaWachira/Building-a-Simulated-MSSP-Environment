# Building-a-Simulated-MSSP-Environment

## Objective
The objective of this project was to design and build a simulated Managed Security Service Provider (MSSP) environment from the ground up. I wanted hands-on experience with the multi-tenant Microsoft security stack, including Azure Lighthouse, Microsoft Sentinel, Microsoft Defender XDR and Entra ID Governance, the same tools real MSSPs use to protect multiple client organizations. </br> </br> 
Alongside the environment, I set out to create an AI-powered SOC automation pipeline using Claude. When a Sentinel incident fires, the pipeline runs KQL investigations, reaches an evidence-based verdict and escalates suspicious findings to a ticketing system for client follow-up. The project covered the full lifecycle of an MSSP engagement: onboarding a client tenant, setting up cross-tenant delegated access, tuning detection rules, enrolling devices into Intune and Defender for Endpoint, and building a scalable model for granting analysts response permissions. <br></br> 
The broader aim was to deepen my practical understanding of how modern security operations work, and to explore how AI can support analysts in handling incidents more efficiently.

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
- A Microsoft account, `camphor.customer1@outlook.com`, was created to represent Customer 1, using a separate browser profile and a Netherlands VPN endpoint to simulate an independent customer identity and sign-in location. Initial Azure portal sign-ins repeatedly failed with an MSAL state_mismatch error caused by browser session and cookie conflicts when using multiple Microsoft accounts.

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
- To test the Malicious Inbox Rule detection, we created an inbox rule for the test account `admin2@camphorcustomer1.onmicrosoft.com`. We configured the rule to identify messages containing sensitive keywords and then forward them to the controlled external test address `mesainenterprisesltd@gmail.com` while moving the messages to a separate folder to hide them from the inbox.

- We then generated test activity by performing failed sign-ins followed by a successful sign-in. Afterward, we allowed time for the configured analytics rules and Defender detections to process the activity and generate any resulting alerts or incidents. We reviewed the SignInLogs data and confirmed that the Entra ID connector is collecting detailed sign-in information.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/35.png" width="700" />
</p>

### Azure lighthouse delegation setup
The next step was to configure Azure Lighthouse so the MSSP tenant could access Customer 1’s Sentinel environment
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
- We signed into the MSSP tenant as `mssptriagelabke@outlook.com` and checked Service providers / My customers, but Customer 1 was not listed. We then signed back into the Customer 1 tenant and opened Subscriptions > Deployments > CustomDeployment-20260912132812 to verify both the registrationDefinition and registrationAssignment, which showed provisioningState: Succeeded. 

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

### Customer 1 sentinel relay setup
- Lighthouse does not provide a centralized notification mechanism for new Sentinel incidents. Because the “When a Microsoft Sentinel incident is created” trigger is scoped to the customer’s Sentinel workspace, each client requires a small relay configuration consisting of a Logic App and Sentinel automation rule in that client’s environment.

- The investigation and triage logic remains centralized in the MSSP’s existing func-mssp-triage2 Function App, which can be reused across all customers. From law-customer1, we'll navigate to creating the Logic App from there.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/90d9d5db2d2a934f4d5840aafac83efca3987a04/Images/49.png" width="700" />
</p>

### Deployment option - consumption plan
- The hosting option for the Customer 1 relay is Consumption. This matches the existing la-sentinel-relay in rg-triage-lab and the deployment pattern used for the MSSP pipeline. Consumption provides a fully managed, multi-tenant Logic App with pay-per-operation pricing, making it suitable for an incident relay that runs only when Sentinel incidents are created. 

- We therefore created la-sentinel-relay-customer1 using the Consumption plan in rg-customer1-soc, alongside law-customer1, while keeping the centralized investigation logic in func-mssp-triage2.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/50.png" width="700" />
</p>

### Gap in the delegation scope
- We identified that the Customer 1 subscription was missing the required Microsoft.Web and Microsoft.Logic resource providers, and the MSSP delegation did not include Logic App Contributor permissions. We therefore updated the existing Azure Lighthouse delegation template by adding the Logic App Contributor role alongside the existing Sentinel Contributor, Sentinel Responder, and Reader roles. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/51.png" width="700" />
</p>

- This is a one-time subscription-level prerequisite that must be completed by the Customer 1 native administrator, `CamphorCust@CamphorCustomer1.onmicrosoft.com`. After registration, we can return to the MSSP account and continue creating the Logic App. The updated template was deployed to Camphor Customer 1 using the same mspOfferName, updating the existing delegation rather than creating a duplicate.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/52.png" width="700" />
</p>

- So to recap the full picture with both identities' correct roles:

`CamphorCust@CamphorCustomer1.onmicrosoft.com (Customer 1's native admin) > publishes/updates the Lighthouse delegation template, and is the only one who can register resource providers (Microsoft.Web, Microsoft.Logic) on that subscription`

`mssptriagelabke@outlook.com (our MSSP tenant) > is the one who receives that delegated access, and uses it to create the Logic App, configure the Sentinel trigger, and eventually run the automation`

### Customer 1 logic app realy
- At this stage, we deployed la-sentinel-relay-customer1 in rg-customer1-soc, using the Consumption plan in South Africa North, alongside the existing MSSP relay la-sentinel-relay. We can now proceed with configuring the Sentinel trigger on la-sentinel-relay-customer1 and select law-customer1 as the monitored workspace.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/53.png" width="700" />
</p>

### Selecting the sentinel trigger
- In the Logic App designer, we selected the Microsoft Sentinel incident trigger. This is the appropriate trigger for the automation because the workflow is designed to start when a Sentinel incident is created and then send the incident to the centralized triage function for investigation. 

- The Microsoft Sentinel alert trigger is intended for individual alerts, while Microsoft Sentinel entity is designed for entity-focused workflows. We therefore used Microsoft Sentinel incident and proceeded with the MSSP account `mssptriagelabke@outlook.com` to configure the connection and select law-customer1 as the monitored workspace.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/54.png" width="700" />
</p>

### Configuring the sentinel connection
- For the Microsoft Sentinel incident trigger, we selected OAuth authentication and used the MSSP analyst account `mssptriagelabke@outlook.com`. The Tenant ID was left blank or set to the MSSP tenant ID 2947e56e-1901-4f69-9fc0-f9c8a7ce2cc5, since the connection authenticates the MSSP account while Azure Lighthouse provides access to the delegated Customer 1 resources. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/55.png" width="700" />
</p>

- The connection completed successfully, showing “Connected to `live.com#mssptriagelabke@outlook.com`”, confirming that the OAuth connection was established with the correct account. We could then continue to the trigger parameters and select Camphor Customer 1 > rg-customer1-soc > law-customer1.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/56.png" width="700" />
</p>

### Preparing the HTTP action
- Because the newer Microsoft Sentinel trigger does not require the subscription, resource group, or workspace to be selected during trigger configuration, the trigger setup was completed once the OAuth connection showed “Connected to `live.com#mssptriagelabke@outlook.com`”. We then moved to the HTTP action, which will send the incident data from la-sentinel-relay-customer1 to the existing func-mssp-triage2 Function App. 

- Customer 1’s workspace ID was retrieved as 1c913899-5837-4bcd-9eda-316fddf4dff8. The remaining value needed to complete the HTTP request is the SentinelTriage function key, which was retrieved from Cloud Shell by telling Azure CLI which Azure subscription to work in before running the command.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/438888442273abb2bc4e7a1927cd3fc78ce7f2af/Images/57.png" width="700" />
</p>

### Configuring the HTTP action
- We configured the HTTP action in la-sentinel-relay-customer1 to send a POST request to the existing func-mssp-triage2 Function App. The request uses the SentinelTriage function endpoint, with Content-Type: application/json, and passes the Sentinel incident ID, Customer 1 workspace ID 1c913899-5837-4bcd-9eda-316fddf4dff8, customer identifier customer1, and the triggering alert rule name in the request body. 

- The Logic App was then saved so it could forward Customer 1 incidents to the centralized MSSP triage function.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/438888442273abb2bc4e7a1927cd3fc78ce7f2af/Images/59.png" width="700" />
</p>

### Sentinel workspace model
- Microsoft Defender portal’s primary/secondary Sentinel workspace model is designed around workspaces within the same Microsoft Entra tenant, while Azure Lighthouse delegated access does not provide the required cross-tenant Sentinel data integration in the Defender portal. Therefore, we stopped trying to connect law-triage-lab and law-customer1 through the unified Defender portal. 

- For the Customer 1 automation rule, the appropriate next step was to work directly in the Customer 1 tenant using `CamphorCust@CamphorCustomer1.onmicrosoft.com` and configure the automation rule against law-customer1. la-sentinel-relay-customer1 now appears.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/61.png" width="700" />
</p>

### Customer 1 automation rule verification
- The rule is Active, triggers when an incident is created, and its action is correctly configured as Run Logic App playbook: la-sentinel-relay-customer1. This confirms that the Customer 1 incident flow is connected to the dedicated relay rather than the MSSP relay. The expected workflow is now new incident in law-customer1 → automation rule → la-sentinel-relay-customer1 → HTTP POST to func-mssp-triage2 → Claude investigation → shadow-mode results returned to the incident. 

- The next step was to generate a fresh test incident and verify the complete flow.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/62.png" width="700" />
</p>

### End to end automation validation
- We then generated a fresh test incident in law-customer1 and confirmed that the automation pipeline executed successfully. The incident received the expected shadow-mode analysis comment and tags, confirming the complete flow. This successfully validated the end-to-end SOC automation pipeline.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/63.png" width="700" />
</p>

### Device enrollment and testing process
- We used a laptop to enroll it in Intune and generate device-related alerts for testing. The process involved joining the device to Microsoft Entra ID using a native Customer 1 account, confirming the device appeared as Microsoft Entra joined, and verifying Intune enrollment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/63.png" width="700" />
</p>

- We joined the laptop to Microsoft Entra ID using `analyst2@camphorcustomer1.onmicrosoft.com` and completed the Windows join process. After restarting the device, we verified that AzureAdJoined: Yes and MDM: Microsoft Intune confirmed successful Intune enrollment. The device was then available in the Intune and Microsoft security portals for further validation.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/68.png" width="700" />
</p>

### Defender for endpoint onboarding
- We downloaded the Microsoft Defender for Endpoint onboarding script and ran it on the enrolled laptop using an elevated administrator session. After onboarding completed, the device desktop-lrpasie appeared in the Defender device inventory with an Active health state and Full security operations status.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/70.png" width="700" />
</p>

### Potential DLL side-loading detection test
- We created a Microsoft Defender for Endpoint custom detection rule to identify potential DLL side-loading from user-writable locations such as AppData, Temp, Downloads, and Desktop. The rule queries DeviceImageLoadEvents and captures the loading process, DLL path, device, account, and related process details. 

- We then planned to download Spotify on the onboarded device as a controlled test, since its legitimate use of user-writable application paths could generate matching telemetry. The resulting alert would then be used to validate the Sentinel investigation and automation pipeline.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/4a50a550df9ef262d3b8e89b72d907b7313b678c/Images/71.png" width="700" />
</p>

### Troubleshooting customer 1 automation failure
- We got an error, the Logic App trigger was functioning, but the HTTP action returned 500 InternalServerError. The Function App response identified the underlying issue as 403 Forbidden when accessing the Customer 1 Sentinel incident through the Azure management API. The incident input and workspace details were correct, confirming that the issue was a permissions gap for the Function App's managed identity on rg-customer1-soc. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/error2.jpg" width="450" />
</p>

### End to end automation validation
- The next step was to grant the Function App appropriate Sentinel access and retest with a fresh run.

- The fresh Logic App runs completed successfully, confirming that the previous 403 Forbidden error had been resolved. The resulting incidents were processed automatically, with the DLL side-loading incident classified as a false positive, closed, and tagged auto-closed-fp.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/72a.png" width="700" />
</p>

### Freshservice intergration setup
- We began integrating Freshservice as the client escalation platform for suspicious Sentinel incidents. Freshservice was selected as the ticketing destination, using its REST API and API-key authentication. During registration, the service required a business email address, so we planned to use the existing camphorclavis.com domain to create a business-domain address through email forwarding.

- This would allow Freshservice verification messages to be forwarded to the existing Gmail inbox without requiring a separate mailbox or full email-hosting service.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/73.png" width="450" />
</p>

### Freshservice account setup
- The Camphor Clavis Freshservice organization was created at `camphorclavis.freshservice.com`, with James Ngetha configured as the Organization Admin. We then accessed the Freshservice administration area and identified that the AI Agents section was unrelated to the required setup. The next step was to locate the appropriate Agents and API Settings sections for the SOC integration.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/74.png" width="700" />
</p>

### Freshservice agent and API Key setup
- We added `camphor.customer1@outlook.com` to Freshservice as the Customer 1 Security Admin agent, using the Yogi Bear security-admin persona. Freshservice then confirmed that API key access had been enabled for the account and indicated that the key could be accessed through Profile Settings.

- Because the API key belongs to `camphor.customer1@outlook.com`, rather than the `contact@camphorclavis.com` organization-admin account, we needed to access Freshservice under the Customer 1 agent identity to retrieve the key.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/76.png" width="700" />
</p>

### Freshservice API access testing
- We retrieved the Freshservice API key from the Yogi Bear agent account and proceeded to test API access from Azure Cloud Shell while signed in through the MSSP tenant. The request to `https://camphorclavis.freshservice.com/api/v2/tickets` returned an access_denied response with the message “You are not authorized to perform this action.”

- This indicated that although the API key had been retrieved successfully, further investigation was required to determine why the Freshservice API request was not authorized before continuing with the Function App integration.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/77.png" width="700" />
</p>

### Freshservice API intergration access
- We identified Integration Users under Freshservice Admin → User Management as the appropriate method for automated API access. Instead of using a personal agent API key, we created a dedicated integration user for the MSSP automation and assigned the required ticket permissions. We then used the integration user's API key to test the Freshservice API. 

- The request to `https://camphorclavis.freshservice.com/api/v2/tickets` returned {"tickets":[]}, confirming that API authentication and access were working successfully.

### Freshservice configuration and function app preparation
- We identified Yogi Bear’s Freshservice responder ID as 41001012787 and prepared the required Function App settings: FRESHSERVICE_DOMAIN, FRESHSERVICE_API_KEY, and FRESHSERVICE_RESPONDER_ID. Because the newer Azure portal layout uses Environment variables instead of the previous Application Settings location, we added each setting using its Name and Value fields and saved the configuration.

- The next step is to update function_app.py with the Freshservice ticket-creation logic, using the current version of the file to ensure the changes are added correctly

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ce982c795c9a14ea6a9fea7ee30dd3c3771b1894/Images/78a.png" width="700" />
</p>

### Freshservice ticket creation intergration
- We reviewed the current function_app.py and integrated Freshservice ticket creation into the existing Sentinel auto-triage workflow. The changes added Freshservice environment variables, a create_freshservice_ticket() function, and the required take_action() and triage() updates. Tickets are created only when an incident is classified as Suspicious, keeping Freshservice escalation aligned with the escalation-required Sentinel tag.

- The ticket includes the investigation summary, key entities, recommendations, and correlation ID, while Freshservice failures are logged without interrupting the core Sentinel triage process. Before deployment, FRESHSERVICE_REQUESTER_EMAIL was added to the Function App environment variables.

### End to end freshservice escalation validation
- The end-to-end incident-response workflow was successfully validated when a Sentinel incident generated an automated Freshservice escalation. The incident was investigated and classified as Suspicious, with the analysis identifying multiple failed sign-ins followed by a successful authentication, Azure CLI usage, and an unmanaged, non-compliant Windows 10 device. 

- A Freshservice ticket was automatically created and assigned to Yogi Bear, with the investigation details, key entities, recommendations, and correlation ID included. The notification email was delivered successfully, confirming the complete Sentinel > Logic App > Function App > Freshservice escalation workflow.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ce982c795c9a14ea6a9fea7ee30dd3c3771b1894/Images/80.png" width="700" />
</p>






