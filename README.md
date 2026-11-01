# Building-a-Simulated-MSSP-Environment

## Objective
The objective of this project was to design and build a simulated Managed Security Service Provider (MSSP) environment from the ground up, with a focus on replicating the architecture and operational processes used by real-world MSSPs to manage security across multiple client organizations. The environment incorporated the Microsoft security stack, including Azure Lighthouse, Microsoft Sentinel, Microsoft Defender XDR and Entra ID Governance, the same tools real MSSPs use to protect multiple client organizations. </br> </br>
Alongside the environment, I set out to create an AI-powered SOC automation pipeline using Claude. When a Sentinel incident fires, the pipeline runs KQL investigations, reaches an evidence-based verdict and escalates suspicious findings to a ticketing system for client follow-up. The project covered the full lifecycle of an MSSP engagement: onboarding a client tenant, setting up cross-tenant delegated access, tuning detection rules, enrolling devices into Intune and Defender for Endpoint, and building a scalable model for granting analysts response permissions. <br></br> 
The broader aim was to demonstrate how modern security operations can be integrated into an MSSP model, while exploring how AI can support analysts by making incident investigation and response more efficient.

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
- I registered the MSSP domain camphorclavis.com through Porkbun. At this stage, no web hosting or separate email hosting is required. The domain is primarily used for identity and Azure/Entra ID configuration, with Porkbun providing the required DNS management. The initial setup involves adding the TXT record provided by Entra ID to verify ownership of the domain.

- Later, if email services are required, Microsoft 365 can provide Exchange Online mailboxes using the verified domain, with the required MX, SPF, and DKIM records configured through Porkbun. Web hosting is optional and is not required for the MSSP lab.

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
- I monitored the project costs to avoid unexpected charges. Sentinel ingestion was kept at a low level across the tenant, while Microsoft 365 E5 licenses were used only where needed. Budget alerts were configured in Azure Cost Management so i could receive notifications before costs increased significantly. Once the required testing was completed, unnecessary ingestion and licenses were paused rather than running continuously.  

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/8548bbb1506cff6b49917caa373a64dab2075365/Images/5.png" width="350" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/8548bbb1506cff6b49917caa373a64dab2075365/Images/6.png" width="350" />
</p>

### Consolidating to just Customer 1
- After encountering repeated Microsoft restrictions while creating additional customer tenants, i decided to consolidate the lab into Customer 1. The plan was to use this single tenant to host all required telemetry sources, including Microsoft Defender XDR, Cloud Apps, Office 365, Entra ID identity data, and Intune. This simplified the environment while still providing diverse data for testing the SOC automation and investigation pipeline.

### “No business presence” blocked all self-service purchasing
- A newly created Entra ID tenant (with no Microsoft 365 service ever purchased) could not be used to buy Microsoft 365 E5 directly — the retail storefront requires an existing commercial relationship (“business presence”) before it will sell into that tenant, but establishing that presence normally requires making a purchase, creating a circular blocker.
 
- Resolution: A small, self-service-eligible product (Microsoft 365 Business Basic, later superseded by an E3 trial) was purchased first specifically to establish business presence, after which the admin center’s internal purchase catalog exposed E5 as purchasable, resolving the deadlock.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/11.png" width="700" />
</p>

- The required Microsoft 365 E5 licenses were assigned with the recommended services enabled. After the licenses were assigned, the Microsoft Defender portal was opened and confirmed to be fully accessible, including Endpoints, Email & collaboration, Identities, Cloud Apps, and Sentinel integration. Entra ID was also checked to confirm that the E5 identity features were available. No additional configuration was needed at this stage.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/4e61c1b52c793ab9893cfd679f32db459db77d45/Images/10.png" width="700" />
</p>

### Retail commerce silently created a second, unintended tenant
- The E3 trial flow created a new tenant instead of adding the service to the existing Customer 1 tenant. Because the new tenant was different, the existing Azure PAYG subscription was not initially available under the new tenant.
  
- The solution was to use Change directory to move the existing Azure subscription into the newly created tenant. This effectively consolidated the Azure subscription and Microsoft 365 business services into the new tenant, rather than creating another separate environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/12a.jpg" width="375" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d5d842e691757a95ebcdbd8b0ec1d6ab269e3fc4/Images/18.png" width="250" />
</p>

### Creating the Log Analytics workspace + Sentinel
- Next i created the Log Analytics workspace, configuring basic cost-control tags, and then enabled Microsoft Sentinel on that workspace. The final state was law-customer1, in resource group rg-customer1-soc, located in South Africa North, under the Camphor Customer 1 subscription. Once the workspace appeared in the Sentinel creation page, it was ready to have Sentinel enabled and configured.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/dc9b4b454ffd209e9fdb7baf3e469caf45eaf6bd/Images/24.png" width="450" />
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
- After connecting the required data sources, i configured Microsoft Sentinel Analytics rules to detect suspicious activity and generate incidents. From the available rule templates, i enabled selected rules for Microsoft Defender XDR, Microsoft Entra ID, and Microsoft 365, including the Defender XDR rule that creates Sentinel incidents from Defender alerts, sign-ins to disabled accounts, anomalous or impossible-travel sign-ins, malicious inbox rules, unusual file activity, and changes to authentication methods for privileged accounts. These rules provided the initial detection baseline for the SOC automation pipeline.

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
- I identified the built-in Azure roles required for the MSSP to work with Customer 1’s Sentinel environment. These included Microsoft Sentinel Contributor, Microsoft Sentinel Responder, and Reader. These roles were included in the Lighthouse delegation so the MSSP tenant could access and manage the required Sentinel resources.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/37.png" width="700" />
</p>

#### Group creation
- I then created a security group named Camphor Clavis SOC Analysts in Entra ID. I added the required MSSP analyst account to the group and copied the group’s Object ID from its Overview page. This Object ID was then used in the Lighthouse delegation template so access could be assigned to the group instead of an individual user.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/38.png" width="700" />
</p>

#### Custom deployment
- I signed into the Customer 1 tenant and opened Deploy a custom template in the Azure portal. I pasted the Lighthouse ARM template into the editor, replaced the placeholder with the Camphor Clavis SOC Analysts group Object ID, selected the Camphor Customer 1 subscription, and deployed the template. This published the Lighthouse delegation from Customer 1 to the MSSP tenant, allowing the assigned MSSP group to access the customer environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/40.png" width="390" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/42.png" width="315" />
</p>

### Lighthouse verification
- I signed into the MSSP tenant as `mssptriagelabke@outlook.com` and checked Service providers / My customers, but Customer 1 was not listed. I then signed back into the Customer 1 tenant and opened Subscriptions > Deployments > CustomDeployment-20260912132812 to verify both the registrationDefinition and registrationAssignment, which showed provisioningState: Succeeded. 

- I then returned to the MSSP tenant and used the direct Customer 1 subscription URL and Subscriptions page to confirm whether the delegated Camphor Customer 1 subscription and law-customer1 workspace were accessible.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/43.png" width="700" />
</p>

### Workspace Manager
- I went from the MSSP Sentinel workspace law-triage-lab to Configuration > Workspace Manager (Preview) and found that Workspace Manager was not yet enabled on the workspace. The page confirmed that at least two Sentinel workspaces, Microsoft Sentinel Contributor access on the central and member workspaces, and Azure Lighthouse for cross-tenant management were required.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/44.png" width="700" />
</p>

- Since the Add workspace button was grayed out, we followed “enable workspace manager in your Microsoft Sentinel settings” to enable Workspace Manager on law-triage-lab before adding law-customer1 as the member workspace.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/45.png" width="700" />
</p>

- I managed to add the law-customer1 workspace successfully to Workspace Manager, with the page confirming “1 Members.” I then returned to the Sentinel workspace view and confirmed that law-customer1 was now registered as a managed member workspace under law-triage-lab. This completed the Workspace Manager configuration needed to manage both the MSSP workspace and the Customer 1 workspace from the central environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/46.png" width="700" />
</p>

### Current MSSP environment status
- At this stage, i have successfully connected the MSSP tenant and Customer 1 through Azure Lighthouse. Microsoft Sentinel was enabled in both environments, with XDR, Entra ID, and Microsoft 365 connectors and analytics rules configured on the Customer 1 side.

- Test activity was also flowing into Sentinel, and law-customer1 has been successfully added to Workspace Manager under the MSSP workspace law-triage-lab, establishing the foundation for centralized management of the customer environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/47.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ade60d734982e74814540da0c002c39ab7225afc/Images/48.png" width="700" />
</p>

### Customer 1 sentinel relay setup
- Lighthouse does not provide a centralized notification mechanism for new Sentinel incidents. Because the “When a Microsoft Sentinel incident is created” trigger is scoped to the customer’s Sentinel workspace, each client requires a small relay configuration consisting of a Logic App and Sentinel automation rule in that client’s environment.

- The investigation and triage logic remains centralized in the MSSP’s existing func-mssp-triage2 Function App, which can be reused across all customers. From law-customer1, i navigated to creating the Logic App from there.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/90d9d5db2d2a934f4d5840aafac83efca3987a04/Images/49.png" width="700" />
</p>

### Deployment option - consumption plan
- The hosting option for the Customer 1 relay is Consumption. This matches the existing la-sentinel-relay in rg-triage-lab and the deployment pattern used for the MSSP pipeline. Consumption provides a fully managed, multi-tenant Logic App with pay-per-operation pricing, making it suitable for an incident relay that runs only when Sentinel incidents are created. 

- So i created la-sentinel-relay-customer1 using the Consumption plan in rg-customer1-soc, alongside law-customer1, while keeping the centralized investigation logic in func-mssp-triage2.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/50.png" width="700" />
</p>

### Gap in the delegation scope
- I identified that the Customer 1 subscription was missing the required Microsoft.Web and Microsoft.Logic resource providers, and the MSSP delegation did not include Logic App Contributor permissions. I therefore updated the existing Azure Lighthouse delegation template by adding the Logic App Contributor role alongside the existing Sentinel Contributor, Sentinel Responder, and Reader roles. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/51.png" width="700" />
</p>

- This is a one-time subscription-level prerequisite that must be completed by the Customer 1 native administrator, `CamphorCust@CamphorCustomer1.onmicrosoft.com`. After registration, i returned to the MSSP account and continue creating the Logic App. The updated template was deployed to Camphor Customer 1 using the same mspOfferName, updating the existing delegation rather than creating a duplicate.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/52.png" width="700" />
</p>

- So to recap the full picture with both identities' correct roles:

`CamphorCust@CamphorCustomer1.onmicrosoft.com (Customer 1's native admin) > publishes/updates the Lighthouse delegation template, and is the only one who can register resource providers (Microsoft.Web, Microsoft.Logic) on that subscription`

`mssptriagelabke@outlook.com (our MSSP tenant) > is the one who receives that delegated access, and uses it to create the Logic App, configure the Sentinel trigger, and eventually run the automation`

### Customer 1 logic app realy
- At this stage, i deployed la-sentinel-relay-customer1 in rg-customer1-soc, using the Consumption plan in South Africa North, alongside the existing MSSP relay la-sentinel-relay. I now proceeded with configuring the Sentinel trigger on la-sentinel-relay-customer1 and select law-customer1 as the monitored workspace.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/53.png" width="700" />
</p>

### Selecting the sentinel trigger
- In the Logic App designer, i selected the Microsoft Sentinel incident trigger. This is the appropriate trigger for the automation because the workflow is designed to start when a Sentinel incident is created and then send the incident to the centralized triage function for investigation. 

- The Microsoft Sentinel alert trigger is intended for individual alerts, while Microsoft Sentinel entity is designed for entity-focused workflows. I used Microsoft Sentinel incident and proceeded with the MSSP account `mssptriagelabke@outlook.com` to configure the connection and select law-customer1 as the monitored workspace.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/54.png" width="700" />
</p>

### Configuring the sentinel connection
- For the Microsoft Sentinel incident trigger, i selected OAuth authentication and used the MSSP analyst account `mssptriagelabke@outlook.com`. The Tenant ID was left blank or set to the MSSP tenant ID 2947e56e-1901-4f69-9fc0-f9c8a7ce2cc5, since the connection authenticates the MSSP account while Azure Lighthouse provides access to the delegated Customer 1 resources. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/55.png" width="700" />
</p>

- The connection completed successfully, showing “Connected to `live.com#mssptriagelabke@outlook.com`”, confirming that the OAuth connection was established with the correct account. I could then continue to the trigger parameters and select Camphor Customer 1 > rg-customer1-soc > law-customer1.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7ae5b0eb564d44729636ea65c153b54cc6493227/Images/56.png" width="700" />
</p>

### Preparing the HTTP action
- Because the newer Microsoft Sentinel trigger does not require the subscription, resource group, or workspace to be selected during trigger configuration, the trigger setup was completed once the OAuth connection showed “Connected to `live.com#mssptriagelabke@outlook.com`”. I then moved to the HTTP action, which will send the incident data from la-sentinel-relay-customer1 to the existing func-mssp-triage2 Function App. 

- Customer 1’s workspace ID was retrieved as 1c913899-5837-4bcd-9eda-316fddf4dff8. The remaining value needed to complete the HTTP request is the SentinelTriage function key, which was retrieved from Cloud Shell by telling Azure CLI which Azure subscription to work in before running the command.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/438888442273abb2bc4e7a1927cd3fc78ce7f2af/Images/57.png" width="700" />
</p>

### Configuring the HTTP action
- I configured the HTTP action in la-sentinel-relay-customer1 to send a POST request to the existing func-mssp-triage2 Function App. The request uses the SentinelTriage function endpoint, with Content-Type: application/json, and passes the Sentinel incident ID, Customer 1 workspace ID 1c913899-5837-4bcd-9eda-316fddf4dff8, customer identifier customer1, and the triggering alert rule name in the request body. 

- The Logic App was then saved so it could forward Customer 1 incidents to the centralized MSSP triage function.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/438888442273abb2bc4e7a1927cd3fc78ce7f2af/Images/59.png" width="700" />
</p>

### Sentinel workspace model
- Microsoft Defender portal’s primary/secondary Sentinel workspace model is designed around workspaces within the same Microsoft Entra tenant, while Azure Lighthouse delegated access does not provide the required cross-tenant Sentinel data integration in the Defender portal. Therefore, i stopped trying to connect law-triage-lab and law-customer1 through the unified Defender portal. 

- For the Customer 1 automation rule, the appropriate next step was to work directly in the Customer 1 tenant using `CamphorCust@CamphorCustomer1.onmicrosoft.com` and configure the automation rule against law-customer1. la-sentinel-relay-customer1 now appears.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/61.png" width="700" />
</p>

### Customer 1 automation rule verification
- The rule is Active, triggers when an incident is created, and its action is correctly configured as Run Logic App playbook: la-sentinel-relay-customer1. This confirms that the Customer 1 incident flow is connected to the dedicated relay rather than the MSSP relay. The expected workflow is now new incident in law-customer1 > automation rule > la-sentinel-relay-customer1 > HTTP POST to func-mssp-triage2 > Claude investigation > shadow-mode results returned to the incident. 

- The next step was to generate a fresh test incident and verify the complete flow.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/62.png" width="700" />
</p>

### End to end automation validation
- I then generated a fresh test incident in law-customer1 and confirmed that the automation pipeline executed successfully. The incident received the expected shadow-mode analysis comment and tags, confirming the complete flow. This successfully validated the end-to-end SOC automation pipeline.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/63.png" width="700" />
</p>

### Device enrollment and testing process
- I used a laptop to enroll it in Intune and generate device-related alerts for testing. The process involved joining the device to Microsoft Entra ID using a native Customer 1 account, confirming the device appeared as Microsoft Entra joined, and verifying Intune enrollment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/63.png" width="700" />
</p>

- Joined the laptop to Microsoft Entra ID using `analyst2@camphorcustomer1.onmicrosoft.com` and completed the Windows join process. After restarting the device, i verified that AzureAdJoined: Yes and MDM: Microsoft Intune confirmed successful Intune enrollment. The device was then available in the Intune and Microsoft security portals for further validation.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/68.png" width="700" />
</p>

### Defender for endpoint onboarding
- I downloaded the Microsoft Defender for Endpoint onboarding script and ran it on the enrolled laptop using an elevated administrator session. After onboarding completed, the device desktop-lrpasie appeared in the Defender device inventory with an Active health state and Full security operations status.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/02ade3e429530a7817d7fbd25ff43486e108106a/Images/70.png" width="700" />
</p>

### Potential DLL side-loading detection test
- I created a Microsoft Defender for Endpoint custom detection rule to identify potential DLL side-loading from user-writable locations such as AppData, Temp, Downloads, and Desktop. The rule queries DeviceImageLoadEvents and captures the loading process, DLL path, device, account, and related process details. 

- Downloaded Spotify on the onboarded device as a controlled test, since its legitimate use of user-writable application paths could generate matching telemetry. The resulting alert would then be used to validate the Sentinel investigation and automation pipeline.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/4a50a550df9ef262d3b8e89b72d907b7313b678c/Images/71.png" width="700" />
</p>

### Troubleshooting customer 1 automation failure
- I got an error, the Logic App trigger was functioning, but the HTTP action returned 500 InternalServerError. The Function App response identified the underlying issue as 403 Forbidden when accessing the Customer 1 Sentinel incident through the Azure management API. The incident input and workspace details were correct, confirming that the issue was a permissions gap for the Function App's managed identity on rg-customer1-soc. 

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
- Next was integrating Freshservice as the client escalation platform for suspicious Sentinel incidents. Freshservice was selected as the ticketing destination, using its REST API and API-key authentication. During registration, the service required a business email address, so i used the existing camphorclavis.com domain to create a business-domain address through email forwarding.

- This would allow Freshservice verification messages to be forwarded to the existing Gmail inbox without requiring a separate mailbox or full email-hosting service.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/73.png" width="450" />
</p>

### Freshservice account setup
- The Camphor Clavis Freshservice organization was created at `camphorclavis.freshservice.com`, with James Ngetha configured as the Organization Admin. I then accessed the Freshservice administration area and identified that the AI Agents section was unrelated to the required setup. The next step was to locate the appropriate Agents and API Settings sections for the SOC integration.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/74.png" width="700" />
</p>

### Freshservice agent and API Key setup
- I added `camphor.customer1@outlook.com` to Freshservice as the Customer 1 Security Admin agent, using the Yogi Bear security-admin persona. Freshservice then confirmed that API key access had been enabled for the account and indicated that the key could be accessed through Profile Settings.

- Because the API key belongs to `camphor.customer1@outlook.com`, rather than the `contact@camphorclavis.com` organization-admin account, we needed to access Freshservice under the Customer 1 agent identity to retrieve the key.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/76.png" width="700" />
</p>

### Freshservice API access testing
- I retrieved the Freshservice API key from the Yogi Bear agent account and proceeded to test API access from Azure Cloud Shell while signed in through the MSSP tenant. The request to `https://camphorclavis.freshservice.com/api/v2/tickets` returned an access_denied response with the message “You are not authorized to perform this action.”

- This indicated that although the API key had been retrieved successfully, further investigation was required to determine why the Freshservice API request was not authorized before continuing with the Function App integration.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/931bbdc91e12ff695d9476741cd1cef319734140/Images/77.png" width="700" />
</p>

### Freshservice API intergration access
- I identified Integration Users under Freshservice Admin > User Management as the appropriate method for automated API access. Instead of using a personal agent API key, i created a dedicated integration user for the MSSP automation and assigned the required ticket permissions. I then used the integration user's API key to test the Freshservice API. 

- The request to `https://camphorclavis.freshservice.com/api/v2/tickets` returned {"tickets":[]}, confirming that API authentication and access were working successfully.

### Freshservice configuration and function app preparation
- I identified Yogi Bear’s Freshservice responder ID as 41001012787 and prepared the required Function App settings: FRESHSERVICE_DOMAIN, FRESHSERVICE_API_KEY, and FRESHSERVICE_RESPONDER_ID. Because the newer Azure portal layout uses Environment variables instead of the previous Application Settings location, i added each setting using its Name and Value fields and saved the configuration.

- The next step is to update function_app.py with the Freshservice ticket-creation logic, using the current version of the file to ensure the changes are added correctly

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ce982c795c9a14ea6a9fea7ee30dd3c3771b1894/Images/78a.png" width="700" />
</p>

### Freshservice ticket creation intergration
- I reviewed the current function_app.py and integrated Freshservice ticket creation into the existing Sentinel auto-triage workflow. The changes added Freshservice environment variables, a create_freshservice_ticket() function, and the required take_action() and triage() updates. Tickets are created only when an incident is classified as Suspicious, keeping Freshservice escalation aligned with the escalation-required Sentinel tag.

- The ticket includes the investigation summary, key entities, recommendations, and correlation ID, while Freshservice failures are logged without interrupting the core Sentinel triage process. Before deployment, FRESHSERVICE_REQUESTER_EMAIL was added to the Function App environment variables.

### End to end freshservice escalation validation
- The end-to-end incident-response workflow was successfully validated when a Sentinel incident generated an automated Freshservice escalation. The incident was investigated and classified as Suspicious, with the analysis identifying multiple failed sign-ins followed by a successful authentication, Azure CLI usage, and an unmanaged, non-compliant Windows 10 device. 

- A Freshservice ticket was automatically created and assigned to Yogi Bear, with the investigation details, key entities, recommendations, and correlation ID included. The notification email was delivered successfully, confirming the complete Sentinel > Logic App > Function App > Freshservice escalation workflow.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/ce982c795c9a14ea6a9fea7ee30dd3c3771b1894/Images/80.png" width="700" />
</p>

### Defender XDR RBAC roles
- While configuring the planned analyst response access, the expected Defender XDR Roles section was not visible in the initial settings area. I identified that the relevant location was Permissions and roles, rather than the general Defender XDR settings page.

- The investigation then focused on whether Microsoft Defender for Endpoint unified RBAC was already active in Customer 1 and where the available Roles section was exposed. The next step was to locate the Roles page or use the Defender portal search to identify the correct RBAC configuration area.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bc1bd9dbf0218ba3a412ecebb8f9bca89255d1cc/Images/82.png" width="700" />
</p>

### Analyst response actions
- I planned to extend the MSSP environment with controlled analyst response actions and scalable client access. In Customer 1, analysts would receive scoped Defender XDR permissions for actions such as device isolation and an Entra ID role such as Helpdesk Administrator for password resets.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bc1bd9dbf0218ba3a412ecebb8f9bca89255d1cc/Images/83.png" width="700" />
</p>

### SOC analyst response group assignment
- I created the SOC-Analysts-Response security group and used it as the required assignment for the Defender response role. This group provides a central way to manage the analysts who will receive the scoped response permissions.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bc1bd9dbf0218ba3a412ecebb8f9bca89255d1cc/Images/86.png" width="700" />
</p>

### Entra entitlement management
- Entra Entitlement Management would use a Connected Organization, catalog, and Access Package to provide a structured way for MSSP analysts to request access. This would allow access to be approved and time-limited, reducing the need to manually invite and manage each analyst individually.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bc1bd9dbf0218ba3a412ecebb8f9bca89255d1cc/Images/87.png" width="700" />
</p>

### Role assignable security group configuration
- I found that SOC-Analysts-Response was created as a standard Security group and therefore was not available when assigning the Entra Helpdesk Administrator role. The group needed to be created with Microsoft Entra roles can be assigned to this group enabled at creation time.

- I therefore created a new role-assignable SOC-Analysts-Response group, re-pointed the Defender custom role assignment to the new group, and then used the same group for the Entra password-reset role. This ensured both role assignments were managed through a single group.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/01d9f41f60a76ceed15503d4f86e7ad4fc6ac8fb/Images/84a.png" width="700" />
</p>

### Entra entitlement management configuration
- I registered Camphor Clavis MSSP as a Connected Organization with the description “MSSP partner tenant providing SOC analyst services for Camphor Customer 1” and set its state to Configured. Then created the SOC Access catalog, added the SOC-Analysts-Response group as a resource, and created the Customer 1 SOC Response Access package. 

- The package was configured for external self-service requests, approval, requestor justification, and time-limited access with lifecycle controls.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/01d9f41f60a76ceed15503d4f86e7ad4fc6ac8fb/Images/88.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/01d9f41f60a76ceed15503d4f86e7ad4fc6ac8fb/Images/92.png" width="700" />
</p>

### Testing customer 1 SOC response access
- I tested the Entra Entitlement Management workflow using the MSSP analyst identity `analyst1@camphorclavis.com`. The analyst signed in to myaccess.microsoft.com, but the general Access Packages view showed no available packages because it was displaying packages from the analyst’s home tenant. I therefore identified the need to use the specific My Access portal link for Customer 1 SOC Response Access from the Customer 1 tenant.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/01d9f41f60a76ceed15503d4f86e7ad4fc6ac8fb/Images/95.png" width="700" />
</p>

- The analyst would then submit a request with the justification: “Requesting SOC analyst response access to support ongoing security monitoring and incident response for Camphor Customer 1.”

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/01d9f41f60a76ceed15503d4f86e7ad4fc6ac8fb/Images/96.png" width="700" />
</p>

### Access package approval troubleshooting
- I attempted to approve the Customer 1 SOC Response Access request submitted by `analyst1@camphorclavis.com`. The pending request was visible in the Customer 1 Entitlement Management interface, but there was no Approvals option in the available Access Management navigation, and the request itself was not directly clickable. 

- Then determined that the approval workflow needed to be accessed through the My Access portal using the Customer 1 administrator identity `CamphorCust@CamphorCustomer1.onmicrosoft.com`, where the pending request should be available for approval.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/01d9f41f60a76ceed15503d4f86e7ad4fc6ac8fb/Images/97.png" width="700" />
</p>

### Tenant signin branding and identification
- I configured Microsoft Entra Company Branding for Customer 1 to display the Camphor Customer 1 name and branding on the Microsoft sign-in page. Tested the branding with the native `CamphorCust@CamphorCustomer1.onmicrosoft.com` account and separately considered the experience for guest analysts such as `analyst1@camphorclavis.com`. 

- Testing confirmed that Company Branding applies to the Microsoft sign-in page but does not show the customer’s branding inside the Microsoft Defender portal yet.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/63effc045fd4c60398cc4f64709914c15ad2e90a/Images/99a.png" width="450" />
</p>

### Azure key vault setup
- The Function App secrets are not yet stored in Azure Key Vault. Since I prefer using the Azure Portal, I switch to the portal-based approach and created the Key Vault under the Camphor Clavis MSSP subscription in rg-triage-lab

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/6cab1a3742b445279c98c5de5ec86fc65315abbd/Images/100.png" width="500" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/6cab1a3742b445279c98c5de5ec86fc65315abbd/Images/101.png" width="500" />
</p>

### Key valut secret permissions
- After creating the Key Vault, I attempted to add the anthropic-api-key secret but receive an “The operation is not allowed by RBAC” error. The Key Vault uses Azure RBAC, so creating the vault does not automatically give me permission to create secrets.

- I resolved this by assigning myself, `mssptriagelabke@outlook.com`, the Key Vault Secrets Officer role under the vault's Access control (IAM). After the role assignment propagates, I can create and manage the required secrets. The Function App will later receive the narrower Key Vault Secrets User role so it can only read the secrets.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/9901e1cca670fec2e3381f937dae437f60898d49/Images/102a.png" width="500" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/9901e1cca670fec2e3381f937dae437f60898d49/Images/103.png" width="500" />
</p>

### Function app key vault access
- I assigned the Key Vault Secrets User role to the func-mssp-triage2 Function App's managed identity. This gives the Function App read-only access to the secrets stored in the Key Vault without permission to create, modify, or delete them. 

- With the role assignment completed, the Function App is ready to use the Key Vault secrets through Key Vault references instead of storing the API keys directly in its environment variables.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/4e61c1b52c793ab9893cfd679f32db459db77d45/Images/104a.png" width="700" />
</p>

### Key vault migration validation
- I updated the ANTHROPIC_API_KEY and FRESHSERVICE_API_KEY environment variables in func-mssp-triage2 to use unversioned Azure Key Vault references, allowing the Function App to use the latest version of each secret automatically. We then tested the Function App using a known Customer 1 incident through Azure Cloud Shell.

- The test completed successfully, confirming that the Function App could retrieve the vaulted credentials and continue the full investigation workflow, including generating KQL queries and reaching an automated verdict.

## Conclusion
- This project delivered a working, end-to-end simulation of an MSSP’s core technical operations: multi-tenant delegated access, a fully licensed and instrumented client environment, an AI-driven investigation and triage pipeline validated against real security incidents, a genuine client-facing escalation workflow, real endpoint enrollment, and a governed, scalable model for granting analysts the access they need across client tenants.</br></br>
<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/850b42063ad5c02fc0cbdab1263bb0ef89269599/Images/Done%20and%20Done.png" width="500" />
</p>

</br></br>
```text
██████   ██   ██   █████   ███████  ███████    ██████
██   ██  ██   ██  ██   ██  ██       ██              ██
██████   ███████  ███████  ███████  █████       █████
██       ██   ██  ██   ██       ██  ██         ██
██       ██   ██  ██   ██  ███████  ███████    ███████
```

## Hardening of a Microsoft 365 and Entra ID Tenant
Hardened a Microsoft 365 tenant end to end across Entra ID (PIM, Conditional Access, break-glass), Intune, Defender for Endpoint and Office 365, Sentinel, SSO/SCIM and email authentication, then benchmarked it against CIS with Maester and documented it in runbooks and policies

### Emergency Access Accounts and Break-Glass Configuration
- I configured a dedicated break-glass accounts, bg-emer01, with permanent Global Administrator privileges to ensure emergency access to the Customer 1 tenant if normal administrator accounts become unavailable.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/51fa0a427dc28f87eac60e1f7e465612ca1a5d2b/Images/109.png" width="430" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/51fa0a427dc28f87eac60e1f7e465612ca1a5d2b/Images/111.png" width="270" />
</p>

- I also created the CA-Exclude-EmergencyAccess security group, added account to it, and used it to exclude them from Conditional Access policies. I configured phishing-resistant MFA using FIDO2 security keys and stored the credentials and keys separately.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/51fa0a427dc28f87eac60e1f7e465612ca1a5d2b/Images/112.png" width="295" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/51fa0a427dc28f87eac60e1f7e465612ca1a5d2b/Images/116.png" width="405" />
</p>

- To monitor their use, i configured Microsoft Entra diagnostic logs to flow into Microsoft Sentinel and created two high-severity analytics rules to detect break-glass account sign-ins and changes to the emergency accounts or exclusion group. Finally, i tested both accounts and verified that the expected alerts appeared in Sentinel.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/51fa0a427dc28f87eac60e1f7e465612ca1a5d2b/Images/117.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/51fa0a427dc28f87eac60e1f7e465612ca1a5d2b/Images/118.png" width="700" />
</p>

### Privileged Identity Management and Just-in-Time Access
- I configured Microsoft Entra Privileged Identity Management (PIM) to reduce the need for permanent administrative access. I set the Global Administrator role to require MFA, justification, and approval, with a maximum activation period of four hours.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/3aad5abfd02867ebcbfdf7d62bef79c1d1ff0ebb/Images/120.png" width="700" />
</p>

- I then made CamphorCust eligible for Global Administrator, tested the activation and approval workflow, and removed its permanent active assignment only after confirming that activation worked.  I also separated routine administration from emergency access by making CamphorCust eligible for Security Administrator

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/3aad5abfd02867ebcbfdf7d62bef79c1d1ff0ebb/Images/123.png" width="700" />
</p>

### Configuring Just-in-Time Sentinel Access
- I confirmed that the Customer 1 subscription was visible in Azure resources and selected the resource group containing the Sentinel workspace to keep permissions scoped to the required environment. I then made CamphorCust permanently eligible for Microsoft Sentinel Contributor and Microsoft Sentinel Responder, configuring both roles to require MFA and justification

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/b783227ea48d38f2e3e8cbb14a7883e5b511bd2f/Images/124.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/b783227ea48d38f2e3e8cbb14a7883e5b511bd2f/Images/126.png" width="700" />
</p>

### Replacing Security Defaults with Conditional Access Policies
- I prepared the Customer 1 tenant to use Microsoft Entra Conditional Access for more controlled protection. I disabled Security Defaults to allow custom Conditional Access policies to be configured, selecting the option indicating that the organization planned to use Conditional Access.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/b783227ea48d38f2e3e8cbb14a7883e5b511bd2f/Images/127.png" width="700" />
</p>

### Deploying Conditional Access Policies
- I then created six policies, CA001 to CA006, covering legacy authentication blocking, MFA for administrative roles, phishing-resistant MFA for administrators, MFA for all users, sign-in risk, and user risk. I enabled CA001 and CA002 to enforce the initial protections, while keeping CA003 to CA006 in report-only mode to assess their impact before enforcement. 

- I excluded the CA-Exclude-EmergencyAccess group from the policies to prevent the emergency accounts from being locked out.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/b783227ea48d38f2e3e8cbb14a7883e5b511bd2f/Images/129.png" width="700" />
</p>

### Integrating Microsoft Intune with Microsoft Defender for Endpoint
- I completed the connection between Microsoft Intune and Microsoft Defender for Endpoint to enable device-risk sharing and stronger compliance enforcement. The Intune connection initially remained unavailable because I had enabled the setting in Defender but had not saved the preferences. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/749f1dd1d976d6f8801c57d50ecb35e070df5daf/Images/132.png" width="700" />
</p>

- After saving the change and refreshing the Intune connector, the connection status changed to Available which was earlier grayed out. This established the integration needed for Intune to evaluate Defender device-risk information.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/131a.png" width="700" />
</p>

### Creating the Windows Compliance Pilot Group
- With the Intune and Defender for Endpoint integration established, I created the Intune-Compliance-Windows-Pilot security group using assigned membership and added desktop-lrpasie as its member. This group provided a dedicated scope for testing Windows compliance requirements on the pilot device before extending the policies to other devices.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/133.png" width="700" />
</p>

### Configuring the Windows Compliance Baseline
-  Next, i configured the WIN-Compliance-Baseline policy for Windows 10 and later devices. Enforcing essential security requirements, including BitLocker, Secure Boot, code integrity, TPM, firewall, and Microsoft Defender Antivirus protection. I also set the minimum operating system version to 10.0.19045 and the Microsoft Defender for Endpoint machine risk threshold to Medium. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/135.png" width="700" />
</p>

### Validating Device Compliance and Investigating Configuration Errors
-  Most security requirements were passing, but BitLocker, Secure Boot, and Code Integrity were returning errors linked to Windows Device Health Attestation (DHA). The errors, 2016345708 (SyncML 404: requested target not found), indicated that Intune could not retrieve the device's health-attestation report, rather than confirming that these security features were disabled.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/137.png" width="700" />
</p>

### Identifying and Correcting Windows Security Compliance Failures
- I confirmed that the device had genuine security configuration issues rather than a reporting problem. PowerShell showed that Secure Boot was disabled, while the C: drive was fully decrypted, meaning BitLocker encryption was not enabled. Code Integrity was also affected because its health assessment depended on the Secure Boot chain. I established that Secure Boot needed to be enabled before starting BitLocker encryption

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/138.png" width="700" />
</p>

### Enabling Secure Boot
- I accessed the HP BIOS Setup using the startup menu rather than F9, which only opens the temporary boot device options. I used Esc during startup, followed by F10 to enter BIOS Setup, and located the Secure Boot configuration. I enabled Secure Boot, disabled Legacy Support where required, and saved the changes

### Preparing BitLocker Encryption Through Intune
- I then created the WIN-BitLocker-Baseline policy under Intune > Endpoint security > Disk encryption, configuring silent device encryption and recovery-key backup to Microsoft Entra ID. I also configured the policy to prevent BitLocker from starting until the recovery information had been stored, and assigned it to Intune-Compliance-Windows-Pilot.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/139.png" width="700" />
</p>

### Correcting Pilot Group Membership and Stale Device Records
- I identified that the current device record had not yet been added to Intune-Compliance-Windows-Pilot. This meant that the compliance and BitLocker policies were not being evaluated against the correct device object. I corrected the group membership by selecting the device with Entra ID and removed outdated device entries.

- This ensured that policy assignments targeted the correct enrolled device.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/140.png" width="700" />
</p>

### Starting and Monitoring BitLocker Encryption
- After correcting the device membership and synchronizing it with Intune, i confirmed that BitLocker encryption had started successfully. An elevated PowerShell check returned EncryptionInProgress at 80%, using the XtsAes256 encryption method specified by the Intune policy. The ProtectionStatus was still Off because encryption had not yet completed.

- Synchronization delivered the policy, while Windows handled encryption separately in the background

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/141.jpeg" width="700" />
</p>

### Completing BitLocker Encryption and Confirming Recovery-Key Backup
- BitLocker encryption completed successfully on DESKTOP-LRPASIE. PowerShell showed FullyEncrypted, 100% encryption, ProtectionStatus On, and XtsAes256. I also verified that the recovery key had been backed up to Entra ID before encryption finished with the drive identified as the operating system drive.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/142.jpeg" width="700" />
</p>

### Compliance Status Updates
- Following synchronization, Intune’s device overview confirmed DESKTOP-LRPASIE as Compliant, the individual policy results confirmed WIN-Compliance-Baseline as compliant, and the Default Device Compliance Policy also showed Compliant. CA007 – Require compliant device is also collecting results in report-only mode.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/141.png" width="700" />
</p>

### Configuring the Microsoft Defender Antivirus Security Baseline
- I created the WIN-Defender-AV-Baseline policy in Microsoft Intune using the Microsoft Defender Antivirus profile and assigned it to Intune-Compliance-Windows-Pilot. This established a consistent antivirus protection baseline for the Windows pilot device.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/144.png" width="700" />
</p>

### Configuring the Windows Attack Surface Reduction Baseline
- I created the WIN-ASR-Baseline policy in Intune using the Windows Attack Surface Reduction Rules profile and assigned it to Intune-Compliance-Windows-Pilot. I configured three key protections to Block, this provided immediate protection against selected high-risk techniques while allowing me to review activity from the other rules through Defender reports and DeviceEvents before increasing enforcement.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/145.png" width="700" />
</p>

### Validating Microsoft Defender Antivirus and ASR Policies
- I verified that Microsoft Defender was active on DESKTOP-LRPASIE, with tamper protection enabled, real-time protection running, and AMRunningMode set to Normal. I also inspected the applied ASR settings and confirmed that the three intended protections—vulnerable signed drivers, LSASS credential theft, and WMI event subscription persistence—were set to Block, while the remaining rules were generally in Audit mode. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/146.jpeg" width="700" />
</p>

### Configuring Defender for Office 365 Email Protection
- Building on the endpoint security controls, i strengthened email protection in the Customer 1 tenant, `CamphorCustomer1.onmicrosoft.com`, using Microsoft Defender for Office 365 preset security policies. 

- I applied the Standard protection preset to all recipients and the Strict protection preset specifically to CamphorCust. I also configured impersonation protection for CamphorCust, analyst1, and analyst2, and added `CamphorCustomer1.onmicrosoft.com` as a protected domain. This helped protect user mailboxes against phishing, impersonation, malicious attachments, and other email-based threats.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/146.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/147.png" width="700" />
</p>

### Enabling Safe Attachments and Safe Links
- Extended Microsoft Defender for Office 365 protection beyond Exchange Online by enabling Safe Attachments for SharePoint, OneDrive, and Teams, activating Safe Documents, preventing users from bypassing Protected View for malicious files, and ensuring Safe Links covered Office applications. This strengthened protection against malicious links and files across Microsoft 365 services.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/148.png" width="700" />
</p>

### Configuring DKIM and DMARC Email Authentication
- I reviewed email authentication for `CamphorCustomer1.onmicrosoft.com`. Microsoft already managed SPF for the onmicrosoft.com domain, while DKIM signing was handled through Microsoft 365. I then added the DMARC TXT record through the Microsoft 365 admin center, using _dmarc as the record name and setting the policy to p=reject, with aggregate reports directed to `CamphorCust@CamphorCustomer1.onmicrosoft.com`.

- This instructed receiving mail systems to reject messages claiming to originate from the domain when they failed the required authentication and alignment checks. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/149.png" width="700" />
</p>

### Testing Email Authentication and Defender Protection
- After completing the configuration, I tested email communication between CamphorCust, and an external Gmail mailbox. The inbound test message from `mesainenterprisesltd@gmail.com` to `analyst2@camphorcustomer1.onmicrosoft.com` passed SPF, DKIM, and DMARC checks and was delivered to the Inbox. I also confirmed that the outbound test email reached Gmail, with its authentication results available. 

- These tests provided evidence that email authentication and delivery were working, while the Defender preset policies provided additional protection against malicious and impersonation-based email threats.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/0d3ff0898f82236b57a31f30d973ec1a8835cc75/Images/150.png" width="700" />
</p>

###  Microsoft Entra ID single sign-on (SSO) through SAML, alongside automatic user provisioning
- AWS IAM Identity Center stood out as a suitable demonstration option because it supports SAML and SCIM integration without an additional service charge, making it useful for demonstrating centralized identity management between AWS and Microsoft Entra ID.

### Configuring AWS Account
- I began preparing the AWS environment for integration with Microsoft Entra ID. I upgraded the AWS plan to the paid pay-as-you-go option and selected the recommended $20 monthly spending limit during the initial upgrade process. This provided a spending safeguard while the account was still operating in its simplified mode.

- Next step was to activate AWS advanced features using a dedicated management account email and team name to enable IAM Identity Center and support Entra ID integration. This required careful planning because switching to an external identity provider is irreversible and disables Builder ID as the team's sign-in method.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/393964ad8af608d6180859e7483ad41238027eaa/Images/151.png" width="700" />
</p>

### Establishing Initial AWS Security and Cost Controls
- The initial security steps included securing the AWS management account root user with a YubiKey as a passkey. I also identified a zero-spend budget as the immediate cost-monitoring control to configure after advanced feature activation, since the initial spending limit would no longer apply. These controls established the foundation for the next phase.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/393964ad8af608d6180859e7483ad41238027eaa/Images/152.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/393964ad8af608d6180859e7483ad41238027eaa/Images/153.png" width="700" />
</p>

### Reviewing the AWS IAM Identity Center Configuration
- Reviewed the AWS IAM Identity Center settings and confirmed the instance details, including its ID, organization, region, creation date, and registered delegated administrator. Verified that AWS Builder ID was the current identity source. Initiated the migration to Microsoft Entra ID by selecting Change identity source.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/393964ad8af608d6180859e7483ad41238027eaa/Images/155.png" width="700" />
</p>

### Selecting Microsoft Entra ID as the External Identity Provider
- Selected External identity provider as the new identity source for AWS IAM Identity Center to prepare for Microsoft Entra ID integration. Reviewed AWS’s warning that the change is permanent and cannot be reversed to AWS Builder ID before proceeding.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/393964ad8af608d6180859e7483ad41238027eaa/Images/156.png" width="700" />
</p>

### Collecting AWS Service Provider Metadata
- Accessed the AWS service provider metadata under the IPv4-only tab and downloaded the metadata file containing the required SAML configuration details, including the AWS access portal URL, Assertion Consumer Service (ACS) URL, and issuer URL. The downloaded file will be used to configure the AWS IAM Identity Center enterprise application in Microsoft Entra ID.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/393964ad8af608d6180859e7483ad41238027eaa/Images/157.png" width="700" />
</p>

### Configuring SAML Authentication in Microsoft Entra ID
- Uploaded the AWS service provider metadata file to configure SAML authentication in the Microsoft Entra ID enterprise application. Reviewed the Basic SAML Configuration, including the identifier, reply URL, and claims mapping for givenname, surname, mail, and UPN. Also verified that the token-signing certificate was active and valid. I also downloaded the Federation Metadata XML from Entra for use in the AWS configuration.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d9c6f151337792fc3761a1c032cc4d1a7e3072f6/Images/158a.png" width="700" />
</p>

### Completing the AWS Identity Source Change
- Uploaded the Microsoft Entra Federation Metadata XML file (entra-metadata.xml) to AWS IAM Identity Center and confirmed the identity source change by typing ACCEPT. AWS confirmed that existing users, groups, permission sets, and assignments would be retained. Noted that users must have matching usernames in Microsoft Entra ID to sign in and that MFA enforcement would now be managed by Microsoft Entra ID instead of AWS.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d9c6f151337792fc3761a1c032cc4d1a7e3072f6/Images/159.png" width="700" />
</p>

### Verifying the External Identity Provider Configuration
- Verified that AWS IAM Identity Center successfully switched to External IdP, with SAML 2.0 configured for authentication and SCIM for user provisioning. Selected Actions > Manage provisioning to begin SCIM setup and generated the SCIM endpoint URL and access token required to configure automated provisioning in the Microsoft Entra ID enterprise application.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d9c6f151337792fc3761a1c032cc4d1a7e3072f6/Images/160.png" width="700" />
</p>

### Enabling SCIM Provisioning in AWS
- Enabled automatic SCIM provisioning in AWS IAM Identity Center and confirmed that the connection addresses (IPv4 and dual-stack) were available. Generated an active access token, which acts as a secure key allowing Microsoft Entra ID to communicate with AWS. The endpoint and access token were ready to set up automatic user and group creation and updates between Microsoft Entra ID and AWS.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/161.png" width="700" />
</p>

### Connecting AWS SCIM Provisioning to Microsoft Entra ID
- Configured SCIM provisioning in the Microsoft Entra ID enterprise application by entering the AWS IPv4 SCIM endpoint as the Tenant URL and the active access token as the Secret Token. Selected Test Connection to verify communication between Entra ID and AWS, then selected Create to save the provisioning configuration.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/162.png" width="700" />
</p>

### Assigning Users to the AWS Enterprise Application
- After configuring the SCIM connection, I opened the Users and groups section of the AWS IAM Identity Center enterprise application in Microsoft Entra ID. I assigned the two existing lab analyst accounts, Homer Simpson (analyst2) and Tom Jerry (analyst1), directly to the application. This ensured that both users were included in the assigned scope for automatic provisioning to AWS.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/163.png" width="700" />
</p>

### Testing On-Demand Provisioning for Tom Jerry
- With Tom Jerry assigned to the AWS enterprise application, I used Provision on demand in Microsoft Entra ID to test the SCIM integration. All four provisioning stages—Import, Scope, Match, and Action—completed successfully, and the account `analyst1@CamphorCustomer1.onmicrosoft.com` was created in AWS IAM Identity Center.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/164.png" width="700" />
</p>

- I also verified the default attribute mappings: userName was mapped to UPN, email to mail, externalId to the Entra object ID (33b9817d-...), and active to Not(IsSoftDeleted). This confirmed that user provisioning was working as expected.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/165.png" width="700" />
</p>

### Verifying Provisioned Users in AWS IAM Identity Center
- I reviewed the Users section in AWS IAM Identity Center and confirmed that both lab analyst accounts had been created successfully through SCIM provisioning: analyst1@... (Tom Jerry) and analyst2@... (Homer Simpson). I also identified a third account, `mesainenterprisesltd@gmail.com` (camphor-lab), which was listed as Manually created. 

- This was an existing account retained from the earlier AWS Builder ID configuration, rather than an account created through Entra provisioning.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/166.png" width="700" />
</p>

### Verifying Microsoft Entra ID Sign-In to AWS
- I also assigned `CamphorCust@CamphorCustomer1.onmicrosoft.com` to the AWS IAM Identity Center enterprise application using the same process. After completing the identity provider and provisioning configuration, I verified the SAML sign-in integration using CamphorCust@CamphorCustomer1.onmicrosoft.com. The AWS access portal opened successfully and displayed the AWS account access application. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/167.png" width="700" />
</p>

- I also checked the Microsoft Entra My Apps dashboard for Camphor Cust and confirmed that the AWS IAM Identity Center application tile was available. This verified that the AWS application was accessible through the Entra identity environment.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/168.png" width="700" />
</p>

### Creating the AWS Read-Only IAM Role
- To prepare read-only access for the SOC analysts, i created the IAM role ReadOnlyAccess in AWS account 811405442773 and attached the AWS-managed ReadOnlyAccess policy. I configured the role with a maximum session duration of one hour to limit how long an assumed session could remain active.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/e1e2917334aa557255f57e044912d902ef001c6c/Images/169.png" width="700" />
</p>

### Creating the Windows Secure Score Quick-Win Policy
- Building on the existing Intune security configuration, which included WIN-ASR-Baseline, WIN-BitLocker-Baseline, and WIN-Defender-AV-Baseline, I added another Settings Catalog policy named WIN-SecureScore-QuickWins. 

- I configured it for Windows 10 and later and included four security improvements: enabling Local Security Authority (LSA) protection, automatically denying elevation requests from standard users, disabling administrator account enumeration during elevation, and turning off AutoPlay for all drives. I assigned the policy to the existing Intune-Compliance-Windows-Pilot group.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7d32114c44491075de6e1b6eaf1c4f9d9c881b8e/Images/170.png" width="385" />
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/7d32114c44491075de6e1b6eaf1c4f9d9c881b8e/Images/171.png" width="315" />
</p>

### Creating the Firewall Policy to Block mshta.exe
- Created a Windows Firewall policy named WIN-Firewall-Block-mshta in Intune to strengthen endpoint security. Added two enabled outbound blocking rules targeting both the 64-bit and 32-bit versions of mshta.exe. Applied the rules to Domain, Private, and Public network profiles to block all outbound network connections from these executables and assigned the policy to Intune-Compliance-Windows-Pilot.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/c03a7a75501b21acc23cb3addaea181a603818bc/Images/172.png" width="700" />
</p>

### Configuring LAPS for the Local Administrator Account
- I configured Windows Local Administrator Password Solution (LAPS) to manage the local administrator account on DESKTOP-LRPASIE. I identified that the device had two separate accounts: .\Kings, a local Windows account, and analyst2 (Homer Simpson), an Entra ID account. 

- Since LAPS manages local accounts, I selected Kings as the account whose password would be automatically rotated and backed up to Microsoft Entra ID. This provided a controlled way to retrieve the local administrator password when maintenance was required.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/c03a7a75501b21acc23cb3addaea181a603818bc/Images/173.png" width="700" />
</p>

### Removing Everyday User Administrative Privileges
- After confirming that LAPS was working and that I could sign in using .\Kings with its managed password, i created an Intune Account Protection policy named WIN-Local-Admins-Baseline. I configured the local Administrators group using Add (Replace) and included only Kings and the built-in Administrator account. I assigned the policy to Intune-Compliance-Windows-Pilot. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/c03a7a75501b21acc23cb3addaea181a603818bc/Images/177.png" width="700" />
</p>

- This replaced the existing local Administrators group membership and removed analyst2 from local administrator privileges while retaining the managed local account for administrative tasks. I verified the membership using the elevated PowerShell command net localgroup administrators.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/c03a7a75501b21acc23cb3addaea181a603818bc/Images/175.png" width="700" />
</p>

- Verified that Microsoft Entra LAPS was successfully managing the local administrator account Kings on DESKTOP-LRPASIE. Also opened the password recovery panel in Entra ID and confirmed that the managed password could be retrieved, with the recovery action recorded for auditing.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/c03a7a75501b21acc23cb3addaea181a603818bc/Images/176.png" width="700" />
</p>

### Enabling Windows Update Reporting
- With LAPS and local administrator restrictions in place, i moved to endpoint patch management. I first enabled Windows update reporting under Intune > Tenant administration > Connectors and tokens > Windows data by turning on the diagnostic data feature and saving the configuration. This enabled Intune to collect the Windows update information needed to monitor patch status and generate update compliance reports.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/178.png" width="700" />
</p>

### Creating the Windows Update Ring
- Created an Intune update policy called WIN-UpdateRing-Pilot and assigned it to the Intune-Compliance-Windows-Pilot group. Configured devices to automatically download and install Windows, Microsoft, and driver updates without delays or the option to pause them. Set updates and restarts to happen outside working hours. 

- Also configured deadlines so that quality updates must be installed within 3 days and feature updates within 7 days, with an extra 1-day grace period before automatic restart. Disabled the Windows 11 upgrade because the device did not meet the required hardware specifications.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/179.png" width="700" />
</p>

### Creating the SharePoint and OneDrive Sharing Detection Rule
- Created a Microsoft Sentinel Near-Real-Time (NRT) analytics rule named SharePoint/OneDrive sharing activity to monitor file-sharing activities. Configured it to detect sharing invitations, links, and anonymous sharing across SharePoint and OneDrive.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/179.png" width="700" />
</p>

### Verifying SharePoint and OneDrive Sharing Restrictions
- Reviewed OneDrive sharing settings using the analyst2 account before testing the detection rule. Confirmed that anonymous (Anyone) sharing links were disabled, with sharing restricted to people in the organization, existing users, or specifically selected individuals. Verified that the default permission was Can view and that link expiration was available.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/181.png" width="700" />
</p>

### Testing External Sharing and Validating the Controls
- Created a Word document named Sharing test in OneDrive to test the external-sharing restrictions. Confirmed that sharing with a new Gmail address was blocked because only existing guest users were allowed. Successfully tested sharing with an approved existing guest account using Can view permission.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/182.png" width="700" />
</p>

### Creating a Dynamic Group for Guest Access Reviews
- To prepare for regular reviews of external user access, i created a dynamic security group in Microsoft Entra ID named All-Guest-Users. I configured it with Dynamic User membership and used the rule (user.userType -eq "Guest") so that all guest accounts would automatically become members. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/183.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/184.png" width="700" />
</p>

### Configuring the Guest Access Review
- Created a quarterly guest access review called Quarterly guest access review in Microsoft Entra ID to regularly check who should still have access to the All-Guest-Users group.

- Assigned CamphorCust as the main reviewer and analyst2 as the backup. Set the review to happen every three months, with three days allowed to complete it. Also enabled automatic application of review decisions, required reviewers to explain their decisions, and added a feature to highlight guest users who had not signed in for 30 days.

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/186.png" width="700" />
</p>

### Reviewing Guest Accounts
- CamphorCust was able to access My Access → Access reviews to review guest accounts and choose Approve or Deny, providing a reason for each decision. For example, the mssptriagelabke account could be approved because it was needed for SOC automation testing, while unused guest accounts could be denied. 

<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/187.png" width="700" />
  <br>
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/bcfd86b366858619ba37747f7b2b1d9aec87339f/Images/188.png" width="700" />
</p>

















