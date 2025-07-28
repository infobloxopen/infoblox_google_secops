# Infoblox Google SecOps Playbooks

This repository contains the sample playbooks that uses the **Infoblox Threat Defense with DDI** integration in **Google SecOps SOAR** platform to gather intel and perform steps based on this intel.

These automated playbooks can be used as a reference to generate a custom workflow to fulfill your needs.

## Import Playbooks

Importing a playbook into any SecOps instance is a very straight-forward task:

- Download the `.zip` file of the playbook that needs to be imported. This file would also contain all the blocks that are used in the playbook.
  ![Download ZIP](<./screenshots/Download ZIP.png>)

- Open the SecOps instance. From the sidebar, navigate to **Response > Playbooks** section.
  ![Sidebar - Playbooks](<./screenshots/Playbook Section.png>)

- Click on the **three dots** icon at the top of the Playbooks, and then click on **Import**.
  ![Import Playbook](<./screenshots/Import Playbook.png>)

- Select the `.zip` file downloaded earlier.

The playbooks stored in the file would be imported. We can access these under the **Imported Playbooks** folder. These playbooks and blocks can be moved to any other folder of our choice.

You can also download the [All Playbook Bundle](<./All Infoblox Playbooks.zip>) which includes all the Infoblox playbooks and blocks. 

**Important:** Before using any playbook with an alert, review its default settings (triggers, action parameters, etc.) to ensure they align with your needs.

## Documentation

- [Playbooks](#playbooks)
  - [Block Indicators](#block-indicators)
  - [Unblock Indicators](#unblock-indicators)
  - [Vulnerability Management Scan](#vulnerability-management-scan)
  - [IP Lookup](#ip-lookup)
  - [Host Lookup](#host-lookup)
  - [URL Lookup](#url-lookup)
  - [MAC Lease Lookup](#mac-lease-lookup)
  - [Incident Response on SOC Insight](#incident-response-on-soc-insight)
- [Blocks](#blocks)
  - [Case Updation Block](#case-updation-block)
  - [Indicator Input Block](#indicator-input-block)
  - [Multi Indicator Input Block](#multi-indicator-input-block)
  - [Block Indicators Block](#block-indicators-block)
  - [Unblock Indicator Block](#unblock-indicators-block)
  - [IP Lookup Block](#ip-lookup)
  - [Host Lookup Block](#host-lookup-block)
  - [URL Lookup Block](#url-lookup-block)
  - [MAC Lease Lookup Block](#mac-lease-lookup-block)
  - [Incident Response on SOC Insight Block](#incident-response-on-soc-insight-block)
- [Known Issues](#known-issues)

---

## Playbooks

Playbooks are a feature in Google Security Operations (SecOps) that can be used to automatically perform actions on alerts/cases when triggered. Playbooks can be used to: Retrieve information about alerts, Remediate threats, Create tickets, and Manage toxic combinations and IAM recommendations.

**Points to Note:**

- `.zip` file for every playbook contains the playbook along with all the blocks used in that playbook.
- Each playbook requires a trigger, which decides when the playbook would be run. For some of the playbooks created, the Trigger is set as **All**. Users can update the Trigger conditions based on the requirements.
- As the trigger of every playbook is set to run on all ingested alerts into SecOps instance, the imported playbooks are disabled by default. User can easily enable any playbook by simply cliking on the toggle button beside the playbook name. ([Reference](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/whats-on-the-playbooks-screen#:~:text=At%20the%20top%20segment%20of%20the%20playbook%20designer%20pane%2C%20you%20can%20use%20the%20horizontal%20toggling%20button%20to%20enable%20or%20disable%20the%20playbook.))

### Block Indicators 

This playbook used to automate the blocking of a user-specified domain or IP by adding it to the Infoblox default block list and removing it from the default allow list, ensuring no policy conflicts. 

![Playbook - Block Indicator](<./screenshots/playbooks/Block Indicators - Infoblox.png>)

**Flow**:

- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- Block Indicators Block (block)

  - This block is used to Block a user-specified indicators by adding it to the Infoblox default block list and removing it from the default allow list [here](#block-indicators-block).
  

---

### Unblock Indicators

This playbook used to automate the unblocking of a user-specified domain or IP by adding it to the Infoblox default slow list and removing it from the default block list, ensuring no policy conflicts. [Download](<./playbooks/Unblock Indicators.zip>)

![Playbook - Unblock Indicator](<./screenshots/playbooks/Unblock Indicators - Infoblox.png>)

**Flow**:

- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- Unblock Indicators Block (block)

  - This block is used to Unblock a user-specified indicators by adding it to the Infoblox default allow list and removing it from the default block list  [here](#unblock-indicators-block).
  
---

### Vulnerability Management Scan

This playbook used to automatically trigger vulnerability scans based on newly detected devices or security events. Use Infoblox data (IP, MAC, Host, or URL) to enrich, prioritize, and contextualize vulnerability management tasks. [Download](<./playbooks/Vulnerability Management Scan.zip>)

![Playbook - Vulnerability Management Scan](<./screenshots/playbooks/Vulnerability Management Scan -  Infoblox.png>)

**Trigger**:
- This playbook can be triggered and attached to alerts containing **Infoblox: DNS** in the name (ingested by the Infoblox - DNS Security Events Connector).

**Flow**:

- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- Indicator Type ? (condition)

  - This is a condition is used to decide the Lookup according to the input value of indicator. (IP, MAC, URL, Host)

- IP Lookup Block (block)

  - This block is used Pulls Dossier, TIDE, and Asset data for any given indicator input(IP) [here](#ip-lookup-block).

- MAC Lease Lookup Block (block)

  - This block is used to Lookup MAC lease information from Infoblox for any given indicator input(MAC) [here](#mac-lease-lookup-block).
  
- URL Lookup Block (block)

  - This block is used to Lookup URL information from Infoblox for any given indicator input(URL) [here](#url-lookup-block).

- Host Lookup Block (block)

  - This block is used to Pulls Dossier, TIDE, and Asset data for any given Host for any given indicator input(Host) [here](#host-lookup-block).

- Case Updation Block (block)

  - This block is used to add comment and change priority of the case. This block gets executed only for the IP/URL/Host indicator input [here](#case-updation-block).

---

### IP Lookup

This playbook used to automate the enrichment of IP addresses by leveraging Infoblox threat intelligence and asset information, enabling rapid and comprehensive insights for security events. [Download](<./playbooks/IP Lookup.zip>)

![Playbook - IP Lookup](<./screenshots/playbooks/IP Lookup - Infoblox.png>)

**Flow**:
- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- IP Lookup Block (block)

  - This block is used Pulls Dossier, TIDE, and Asset data for any given indicator input(IP) [here](#ip-lookup-block).

### Host Lookup

This playbook used to automate the enrichment of Host by leveraging Infoblox threat intelligence and asset information, enabling rapid and comprehensive insights for security events. [Download](<./playbooks/Host Lookup.zip>)

![Playbook - Host Lookup](<./screenshots/playbooks/Host Lookup - Infoblox.png>)

**Flow**:
- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- Host Lookup Block (block)

  - This block is used to Pulls Dossier, TIDE, and Asset data for any given Host for any given indicator input(Host) [here](#host-lookup-block).

### URL Lookup

This playbook used to automate gathering of intelligence using Dossier and TIDE APIs to provide deep context and threat assessment for the URL. [Download](<./playbooks/URL Lookup.zip>)

![Playbook - URL Lookup](<./screenshots/playbooks/URL Lookup - Infoblox.png>)

**Flow**:
- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- URL Lookup Block (block)

  - This block is used to Lookup URL information from Infoblox for any given indicator input(URL) [here](#url-lookup-block).

### MAC Lease Lookup

This playbook used to automate retrieval of MAC lease information from Infoblox, enriching the incident with network context and device assignment details. [Download](<./playbooks/MAC Lease Lookup.zip>)

![Playbook - MAC Lease Lookup](<./screenshots/playbooks/MAC Lease Lookup - Infoblox.png>)

**Flow**:
- Indicator Input Block (block)

  - This block is used to get the indicator input from the User [here](#indicator-input-block).

- MAC Lease Lookup Block (block)

  - This block is used to Lookup MAC lease information from Infoblox for any given indicator input(MAC). [here](#mac-lease-lookup-block)


### Incident Response on SOC Insight

This playbook used to automate SOC incident response with severity-based notifications and ServiceNow incident creation, enriched with asset insights. [Download](<./playbooks/Incident Response on SOC Insight.zip>)

![Playbook - Incident Response on SOC Insight](<./screenshots/playbooks/Incident Response on SOC Insight  -  Infoblox.png>)

**Trigger**:
- This playbook can be triggered and attached to alerts containing **Infoblox: Insight** in the name (ingested by the Infoblox - SOC Insights Connector).

**Flow**:
- Indicator Input Block (block)

  - This block is used to get the Insight ID input from the User [here](#is-ip-suspicious).

- Incident Response on SOC Insight Block
  - This block is used to identify the suspiciousness of the insight from its events and creates a Service Now Incident if the insight event found to be suspicious [here](#incident-response-on-soc-insight-block).

---

## Blocks

A block is a re-usable set of actions and conditions that can be used in multiple playbooks. This acts as a wrapper for performing some set of actions, that are often performed in multiple playbooks.

### Block Indicators Block

This block is used to Block a user-specified indicators by adding it to the Infoblox default block list and removing it from the default allow list. [Download](<./blocks/Block Indicators Block.zip>)

![Block - Block Indicators Block](<./screenshots/blocks/Block Indicator Block.png>)

**Input:**

Indicator Input - Indicators (domains, IPs) from the user or automation. These are the items to be blocked. 

**Flow:**

- Get Custom List (action)
  - This action retrieves the current contents and ID of the default block list from Infoblox. 
  - This action is used to fetch the default block list before attempting to add new indicators in the block list. 

- Update Custom List Items (action)
  - This action adds the provided indicators to the default block list in Infoblox. 
  - This action takes the indicators from step 1(Indicator Input) and the block list ID from step 2(Get Custom List), then updates the block list to include the new indicators (if they are not already present).

- Get Custom List (action)
  - This action retrieves the current contents and ID of the default allow list from Infoblox. 
  - This action is used to fetch the default allow list before attempting to add new indicators in the allow list. 

- Update Custom List Items (action)
  - This action removes the provided indicators from the default allow list in Infoblox. 
  - This action takes the indicators from step-1(Indicator Input) and the allow list ID from step 4(Get Custom List), then removes the indicators from the allow list (if they are present), preventing policy conflicts. 

---

### Unblock Indicators Block

This block unblocks a user-specified indicators by adding it to the Infoblox default allow list and removing it from the default block list. [Download](<./blocks/Unblock Indicator Block.zip>)

![Block - Unblock Indicators Block](<./screenshots/blocks/Unblock Indicator Block.png>)

**Input:**

Indicator Input - Indicators (domains, IPs) from the user or automation. These are the items to be unblocked. 

**Flow:**

- Get Custom List (action)
  - This action retrieves the current contents and ID of the default allow list from Infoblox. 
  - This action is used to fetch the default allow list before attempting to add new indicators in the allow list. 

- Update Custom List Items (action)
  - This action adds the provided indicators to the default allow list in Infoblox. 
  - This action takes the indicators from step 1(Indicator Input) and the allow list ID from step 2(Get Custom List), then updates the allow list to include the new indicators (if they are not already present).

- Get Custom List (action)
  - This action retrieves the current contents and ID of the default block list from Infoblox. 
  - This action is used to fetch the default block list before attempting to add new indicators in the block list. 

- Update Custom List Items (action)
  - This action removes the provided indicators from the default block list in Infoblox. 
  - This action takes the indicators from step-1(Indicator Input) and the block list ID from step 4(Get Custom List), then removes the indicators from the block list (if they are present), preventing policy conflicts. 

---

### IP Lookup Block

This block is used Pulls Dossier, TIDE, and Asset data for any given indicator input(IP). [Download](<./blocks/IP Lookup Block.zip>)

![Block - IP Lookup Block](<./screenshots/blocks/IP Lookup Block.png>)

**Input:**

Indicator Input - IP Indicators from the user or automation.

**Flow:**

- Indicator Threat Lookup With TIDE (action)
  - This action looks up threat intelligence details for an IP indicator using Infoblox TIDE. 

- Initiate Indicator Intel Lookup with Dossier (action)
  - This action collects IP Indicator responses from the Infoblox Dossier. 

- IP Lookup (action)
  - This action retrieves IP address information from Infoblox. 

**Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use. 

---


### Host Lookup Block

This block is used to Pulls Dossier, TIDE, and Asset data for any given Host for any given indicator input(Host). [Download](<./blocks/Host Lookup Block.zip>)

![Block - Host Lookup Block](<./screenshots/blocks/Host Lookup Block.png>)

**Input:**

Indicator Input - Host Indicators from the user or automation.

**Flow:**

- Indicator Threat Lookup With TIDE (action)
  - This action looks up threat intelligence details for an Host indicator using Infoblox TIDE.

- Initiate Indicator Intel Lookup with Dossier (action)
  - This action collects Host Indicator responses from the Infoblox Dossier.

- Host Lookup (action)
  - This action retrieves Host information from Infoblox. 

**Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use. 

---


### URL Lookup Block

This block is used to Lookup URL information from Infoblox for any given indicator input(URL). [Download](<./blocks/URL Lookup Block.zip>)

![Block - URL Lookup Block](<./screenshots/blocks/URL Lookup Block.png>)

**Input:**

Indicator Input - URL Indicators from the user or automation.

**Flow:**

- Indicator Threat Lookup With TIDE (action)
  - This action looks up threat intelligence details for an URL indicator using Infoblox TIDE. 

- Initiate Indicator Intel Lookup with Dossier (action)
  - This action collects URL Indicator responses from the Infoblox Dossier.

**Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use. 

---

### MAC Lease Lookup Block

This block is used to Lookup MAC lease information from Infoblox for any given indicator input(MAC). [Download](<./blocks/MAC Lease Lookup Block.zip>)

![Block - MAC Lease Lookup Block](<./screenshots/blocks/MAC Lease Lookup Block.png>)

**Input:**

Indicator Input - URL Indicators from the user or automation.

**Flow:**

- DHCP Lease Lookup (action)
  - This action looks up DHCP lease information for a given MAC address. 

**Note:** Response from the action will be saved to the widgets. Will be shown	 to the Alert Overview page inside Google SecOps Case for later use. 

---


### Incident Response on SOC Insight Block

This block is used to identify the suspiciousness of the insight from its events and creates a Service Now Incident if the insight event found to be suspicious. [Download](<./blocks/Incident Response on SOC Insight Block.zip>)

![Block - Incident Response on SOC Insight Block](<./screenshots/blocks/Incident Response on SOC Insight Block.png>)

**Input:**

Insight Input - Insight id from the user or automation. 

**Flow:**

- Get SOC Insight Events (action)
  - Collects the relevant security events from the given insight id. 

- Get SOC Insight Assets (action)
  - Collects the relevant asset data from the given insight id. 

- Get SOC Insight Indicators (action)
  - Collects the relevant indicators from the given insight id. 

- Get SOC Insight Comments (action)
  - Collects the relevant comments from the given insight id. 

- Suspicious Event Insight (condition)
  - After performing the insight lookup, the playbook checks if a threatLevelis available in the results. 
  - If the `threatLevel` is `High` or `Medium`, it indicates a high-risk or confirmed suspicious event. 

- Change Case Priority to Critical (action)
  - When the `threatLevel` value is `High` or `Medium`, this action automatically updates the case priority to **Critical** in Siemplify. 

- ServiceNow Create Incident (action)
  - When the `threatLevel` value is `High` or `Medium`, this action automatically creates a ServiceNow ticket for further investigation. 

- Change Case Priority to Informative (action)
  - When the `threatLevel` is `Low`, this action automatically updates the case priority to **Informative** in Siemplify. 

**Note:** 
  - All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use.
  - This playbook requires the ServiceNow Integration configured.

--- 
### Case Updation Block

This block is used to add comment and change priority of the case in case the input `threat_level` value exceeds the configured threshold. [Download](<./blocks/Case Updation Block.zip>)

![Block - Case Updation Block](<./screenshots/blocks/Case Updation Block.png>)

**Input:**

Threat Level - Threat Level value from the user or automation. 

**Flow:**

- Check Threat Level (condition)
  - This is a condition that determines if the input Threat Level value is exceeds the configured threshold or not.
  - If the it exceeds the threshold, the branch with adding comment and changing the case priority to Critical will be executed.

- Add Comment in Case (action)
  - Add a relevant comment to the case for indicating the threatLevel impact.

- Change Case Priority to Critical (action)
  - This action automatically updates the case priority to **Critical** in Siemplify. 

---

### Indicator Input Block

This block is used to collect the Indicator Input. [Download](<./blocks/Indicator Input Block.zip>)

![Block - Indicator Input Block](<./screenshots/blocks/Indicator Input.png>)

**Input:**

Indicator - Indicator value from the user or automation. 

**Output:**
`Indicator` value

---

### Multi Indicator Input Block

This block is used to collect Multiple Indicators (IP, MAC, URL, Host). [Download](<./blocks/Multi Indicator Input Block.zip>)

![Block - Multi Indicator Input Block](<./screenshots/blocks/Multi Indicator Input.png>)

**Input:**

- IP Address - IP Address value from the user or automation.
- MAC Address - MAC Address value from the user or automation. 
- URL/Domain - URL/Domain value from the user or automation. 
- Hostname - Hostname value from the user or automation. 

**Flow:**

- Create JSON (action)
  - This action (XMLTOJson) is a part of [Functions power-up](https://cloud.google.com/chronicle/docs/soar/marketplace-and-integrations/power-ups/functions#xmltojson) provided by SecOps.
  - This action converts the XML created from the input values to JSON, which is then used in other actions.

**Output:**

Returns the generated JSON output.

---

### Known Behaviours
- Currently, only the view from the 1st attached playbook will be set for the **Alert Overview** ([Reference](https://www.googlecloudcommunity.com/gc/SecOps-SOAR/Playbook-Widget-Behavior-in-Alert-Overview/td-p/888644)). For example, if a playbook is already attached to an alert, then the alert overview page will only reflect the view generated by that playbook. Hence, it is advised to enable only the required playbooks. Make sure that the trigger for the playbook is not set to All. Instead, the trigger should be set such that the playbook must only run of relevant set of alerts.


## Troubleshooting

In case of any failures when running any playbook, we can debug the same by running the playbook in the Simulator Mode. For more details, please refer to this guide: [Google SecOps playbooks - Simulator](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/working-with-playbook-simulator).

## References

- Google SecOps playbooks - [Documentation](https://cloud.google.com/chronicle/docs/secops/google-secops-soar-toc#work-with-playbooks)
- Create views with playbooks - [Documentation](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/define-customized-alert-views-from-playbook-designer)
- Limitations around for-loop usage in playbooks:
  - [Cycle over a list](https://www.googlecloudcommunity.com/gc/SOAR-Forum/Cycling-over-a-list-in-a-playbook/m-p/642352)
  - [Iterate through a JSON list](https://www.googlecloudcommunity.com/gc/SOAR-Forum/Iterate-through-a-json-list/m-p/821359#M2891)
  - [Looping in Playbooks](https://www.googlecloudcommunity.com/gc/SOAR-Forum/Loop-in-a-PB-through-a-list-fetched-using-http-get-connector/m-p/639073)
  - [Functions that require single input](https://www.googlecloudcommunity.com/gc/SOAR-Forum/Functions-that-require-single-input/m-p/639027)
- Infoblox Threat Defense with DDI SecOps SOAR Integration - [User Guide](https://docs.infoblox.com/space/DeploymentGuideTDDDI4GoogleSecOpsSOAR/1570111546/Introduction)
