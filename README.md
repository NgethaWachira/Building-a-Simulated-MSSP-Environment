# Building-a-Simulated-MSSP-Environment

## Objective
The objective of this project was to design and build a simulated Managed Security Service Provider (MSSP) environment from the ground up. I wanted hands-on experience with the multi-tenant Microsoft security stack, including Azure Lighthouse, Microsoft Sentinel, Microsoft Defender XDR and Entra ID Governance, the same tools real MSSPs use to protect multiple client organizations. Alongside the environment, I set out to create an AI-powered SOC automation pipeline using Claude. When a Sentinel incident fires, the pipeline runs KQL investigations, reaches an evidence-based verdict and escalates suspicious findings to a ticketing system for client follow-up. The project covered the full lifecycle of an MSSP engagement: onboarding a client tenant, setting up cross-tenant delegated access, tuning detection rules, enrolling devices into Intune and Defender for Endpoint, and building a scalable model for granting analysts response permissions. The broader aim was to deepen my practical understanding of how modern security operations work, and to explore how AI can support analysts in handling incidents more efficiently.

### Tools Used
<div>
  <img src="https://img.shields.io/badge/-Microsoft%20Azure-0078D4?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Microsoft Azure">
  <img src="https://img.shields.io/badge/-Microsoft%20Sentinel-8DD3C7?&style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Sentinel">
  <img src="https://img.shields.io/badge/-Microsoft%20Defender%20Portal-E81123?&style=for-the-badge&logo=microsoft&logoColor=white" alt="Microsoft Defender Portal">
  <img src="https://img.shields.io/badge/-Azure%20Log%20Analytics-1F77B4?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Log Analytics">
  <img src="https://img.shields.io/badge/-Azure%20Function%20App-FF7F0E?&style=for-the-badge&logo=azurefunctions&logoColor=white" alt="Azure Function App">
  <img src="https://img.shields.io/badge/-Azure%20App%20Service-2CA02C?&style=for-the-badge&logo=azure&logoColor=white" alt="Azure App Service">
  <img src="https://img.shields.io/badge/-Azure%20Logic%20Apps-D62728?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Logic Apps">
  <img src="https://img.shields.io/badge/-Azure%20Managed%20Identity-9467BD?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Managed Identity">
  <img src="https://img.shields.io/badge/-Azure%20Entra%20ID-8C564B?&style=for-the-badge&logo=microsoft&logoColor=white" alt="Azure Entra ID">
  <img src="https://img.shields.io/badge/-Azure%20Key%20Vault-E377C2?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Key Vault">
  <img src="https://img.shields.io/badge/-Azure%20Application%20Insights-7F7F7F?&style=for-the-badge&logo=microsoftazure&logoColor=white" alt="Azure Application Insights">
  <img src="https://img.shields.io/badge/-Azure%20ARM%20API-BCBD22?&style=for-the-badge&logo=azuredevops&logoColor=white" alt="Azure Resource Manager API">
  <img src="https://img.shields.io/badge/-Anthropic%20Claude-17BECF?&style=for-the-badge&logo=anthropic&logoColor=white" alt="Anthropic Claude">
  <img src="https://img.shields.io/badge/-Anthropic%20API-AEC7E8?&style=for-the-badge&logo=anthropic&logoColor=white" alt="Anthropic API">
  <img src="https://img.shields.io/badge/-Python%203.11-FFDD57?&style=for-the-badge&logo=python&logoColor=black" alt="Python 3.11">
  <img src="https://img.shields.io/badge/-Azure%20Functions%20Core%20Tools-98DF8A?&style=for-the-badge&logo=azurefunctions&logoColor=white" alt="Azure Functions Core Tools">
  <img src="https://img.shields.io/badge/-Azure%20CLI%20(az)-C5B0D5?&style=for-the-badge&logo=azuredevops&logoColor=white" alt="Azure CLI">
  <img src="https://img.shields.io/badge/-Git-F7B6D2?&style=for-the-badge&logo=git&logoColor=white" alt="Git">
  <img src="https://img.shields.io/badge/-PowerShell-C49C94?&style=for-the-badge&logo=powershell&logoColor=white" alt="PowerShell">
  <img src="https://img.shields.io/badge/-Command%20Prompt%20(CMD-FB8072)?&style=for-the-badge&logo=windows&logoColor=white" alt="Command Prompt">
  <img src="https://img.shields.io/badge/-Azure%20DevOps-BEBADA?&style=for-the-badge&logo=azuredevops&logoColor=white" alt="Azure DevOps">
  <img src="https://img.shields.io/badge/-Azure%20Pipelines-FCCDE5?&style=for-the-badge&logo=azurepipelines&logoColor=white" alt="Azure Pipelines">
  <img src="https://img.shields.io/badge/-requests-80B1D3?&style=for-the-badge&logo=python&logoColor=white" alt="requests">
  <img src="https://img.shields.io/badge/-KQL%20(Query%20Language)-FDB462?&style=for-the-badge&logo=azuredataexplorer&logoColor=white" alt="KQL">
  <img src="https://img.shields.io/badge/-ARM%20API%20(REST)-B3DE69?&style=for-the-badge&logo=azure&logoColor=white" alt="ARM API REST">
</div>

### SOC Automation Architecture
<br>
<p align="center">
  <img src="https://github.com/NgethaWachira/Building-a-Simulated-MSSP-Environment/blob/d6f0567d5d628fd04f89ef6e27e50da0d4d0a991/Images/camphor-clavis-soc-automation-architecture.png" width="700" />
</p>

### Steps
## Domain Registration and MSSP Foundation
- I registered the MSSP domain camphorclavis.com through Porkbun. At this stage, no web hosting or separate email hosting is required. The domain is primarily used for identity and Azure/Entra ID configuration, with Porkbun providing the required DNS management. The initial setup involves adding the TXT record provided by Entra ID to verify ownership of the domain. Later, if email services are required, Microsoft 365 can provide Exchange Online mailboxes using the verified domain, with the required MX, SPF, and DKIM records configured through Porkbun. Web hosting is optional and is not required for the MSSP lab.

<p align="center">
  <img src="https://github.com/NgethaWachira/Azure-Security-and-Simulation/blob/50030693511ae30bd53520e104dde01e51351ce0/Images/Subscription%20role%20assignment.PNG" width="700" />
</p>
