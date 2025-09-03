# Infoblox NIOS Google SecOps Playbooks

This repository contains sample playbooks that uses the **Infoblox NIOS (On-Prem)** integration in **Google SecOps SOAR** platform. These playbooks automate enrichment, response, and asset management actions using Infoblox NIOS data, enabling rapid and comprehensive insights for security events and network operations.

These automated playbooks can be used as a reference to generate a custom workflow to fulfill your needs.


## Import Playbooks

Importing a playbook into any SecOps instance is straightforward:

- Download the `.zip` file of the playbook that needs to be imported. This file would also contain all the blocks that are used in the playbook.
  ![Download ZIP](<./screenshots/Download ZIP.png>)

- Open the SecOps instance. From the sidebar, navigate to **Response > Playbooks** section.
    ![Sidebar - Playbooks](<./screenshots/Playbook Section.png>)

- Click on the **three dots** icon at the top of the Playbooks, and then click on **Import**.
  ![Import Playbook](<./screenshots/Import Playbook.png>)

- Select the `.zip` file downloaded earlier.

The playbooks stored in the file would be imported. We can access these under the **Imported Playbooks** folder. These playbooks and blocks can be moved to any other folder of our choice.

You can also download the [All Playbook Bundle](<./All Infoblox NIOS Playbooks.zip>) which includes all the Infoblox playbooks and blocks. 

**Important:** Before using any playbook with an alert, review its default settings (triggers, action parameters, etc.) to ensure they align with your needs.

---
## Documentation

- [Playbooks](#playbooks)
  - [Block Indicators](#block-indicators)
  - [Unblock Indicators](#unblock-indicators)
  - [IP Lookup](#ip-lookup)
  - [Host Lookup](#host-lookup)
  - [MAC Lease Lookup](#mac-lease-lookup)
  - [Bring Asset Data Back to Infoblox](#bring-asset-data-back-to-infoblox)
- [Blocks](#blocks)
  - [Indicator Input](#indicator-input)
  - [Indicator Input Block-Unblock](#indicator-input-block-unblock)
  - [Get RP Zone](#get-rp-zone)
  - [Validate and Search Indicator](#validate-and-search-indicator)
- [Known Behaviours](#known-behaviours)
---
## Playbooks

Playbooks are a feature in Google Security Operations (SecOps) that can be used to automatically perform actions on alerts/cases when triggered. Playbooks can be used to: Retrieve information about alerts, Remediate threats, Create tickets, and Manage toxic combinations and IAM recommendations.

**Points to Note:**

- `.zip` file for every playbook contains the playbook along with all the blocks used in that playbook.
- Each playbook requires a trigger, which decides when the playbook would be run. For some of the playbooks created, the Trigger is set as **All**. Users can update the Trigger conditions based on the requirements.
- The imported playbooks are disabled by default. User can easily enable any playbook by simply cliking on the toggle button beside the playbook name. ([Reference](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/whats-on-the-playbooks-screen#:~:text=At%20the%20top%20segment%20of%20the%20playbook%20designer%20pane%2C%20you%20can%20use%20the%20horizontal%20toggling%20button%20to%20enable%20or%20disable%20the%20playbook.))
  

### Block Indicators  

This playbook is used to automate the blocking of malicious domains and IP addresses in Infoblox Response Policy Zones (RPZ) using RPZ rule creation and management. [Download](<./playbooks/Block Indicator.zip>)

![Playbook - Block Indicator](<./screenshots/playbooks/Block Indicator.png>)

**Flow:**

- **Indicator Input Block-Unblock (block)**
  - This block collects the indicator (domain or IP) to be blocked, along with the name of the Response Policy Zone (RPZ) [here](#indicator-input-block-unblock).

- **Get RP Zone (block)**
  - The block checks if the specified RP Zone exists in Infoblox NIOS. 
  - If the zone does not exist, it is automatically created with default configuration else it will return the existing zone details [here](#get-rp-zone).

- **Validate and Search Indicator (block)**
  - Determines the indicator type (domain or IP).
  - Searches for an existing RPZ rule for the indicator in the specified RP Zone.
  - Returns the rule details if found, or indicates that no rule exists [here](#validate-and-search-indicator).

- **Condition: Does the RPZ Rule Exist? (condition)**
   - If the rule exists, proceed to canonical value check.
   - If the rule does not exist, proceed to **Create a new RPZ CNAME Rule**.

- **Canonical Value Check (if rule exists)**
    - If `canonical=""` (empty):
       - The indicator is already blocked.
       - Notify the user that the indicator is already blocked.
    - If `canonical!=""` (not empty):
       - The rule exists but is not currently blocked.
       - Proceed to **Update the RPZ CNAME Rule**.

- **Update RPZ CNAME Rule (action)**
   - Updates the existing RPZ CNAME rule for the indicator, setting `canonical=""` to convert it to a blocking rule.
   - Confirms that the indicator is now blocked.

- **Create RPZ CNAME Rule (action)**
   - Creates a new RPZ CNAME rule for the indicator with `canonical=""`, ensuring the indicator is blocked and will return an NXDOMAIN response.

- **Add Comment in Case (action)**
   - Based on the response (already blocked, blocked, or updated), writes a relevant comment in the Google SecOps case for audit and analyst visibility.

---

### Unblock Indicators

This playbook is used to automate the unblocking of a user-specified domain or IP by removing the blocking rule from the Infoblox RPZ, ensuring the indicator is unblocked. [Download](<./playbooks/Unblock Indicator.zip>)

![Playbook - Unblock Indicator](<./screenshots/playbooks/Unblock Indicator.png>)

#### Flow

- **Indicator Input Block-Unblock (block)**
   - This block collects the indicator (domain or IP) to be blocked, along with the name of the Response Policy Zone (RPZ) [here](#indicator-input-block-unblock).

- **Validate and Search Indicator (block)**
   - Determines the indicator type (domain or IP).
   - Searches for an existing RPZ rule for the indicator in the specified RP Zone.
   - Returns the rule details if found, or indicates that no rule exists [here](#validate-and-search-indicator).

- **Does the RPZ Rule Exist? (condition)**
   - If the rule does not exist:
     - The indicator is already unblocked.
     - Notify the user that the indicator is already unblocked.
   - If the rule exists, proceed to **Delete RPZ Rule**.

- **Delete RPZ Rule (action)**
   - Deletes the RPZ rule for the indicator (domain or IP) from the specified RP Zone.
   - Confirms that the indicator is now unblocked.

- **Add Comment in Case (action)**
   - Based on the outcome (already unblocked or now unblocked), writes a relevant comment in the Google SecOps case for audit and analyst visibility.
   - Ensures all actions are traceable in the incident timeline.

---

### IP Lookup

Automate the enrichment of IP addresses by leveraging Infoblox NIOS and asset information, enabling rapid and comprehensive insights into IP data. [Download](<./playbooks/IP Lookup.zip>)

![Playbook - IP Lookup](<./screenshots/playbooks/IP Lookup.png>)

**Flow:**

- **IP Input (block)**
  - This block collects the IP address from the user or automation [here](#indicator-input).

- **IP Lookup (action)**
  - Returns IPAM information for the provided IP address from Infoblox NIOS.

- **List Network Info (action)**
  - Retrieves the network information associated with the given IP address from Infoblox NIOS.

> **Note:** All responses from actions are saved to widgets and shown in the Alert Overview page inside Google SecOps Case for later use.

---

### Host Lookup

Automate the enrichment of Host by leveraging Infoblox NIOS and asset information, enabling rapid and comprehensive insights into host data. [Download](<./playb  ooks/Host Lookup.zip>)

![Playbook - Host Lookup](<./screenshots/playbooks/Host Lookup.png>)

**Flow:**

- **Host Input (block)**
  - This block collects the host name from the user or automation [here](#indicator-input).

- **List Host Info (action)**
  - Returns host information for the provided host name from Infoblox NIOS.

> **Note:** All responses from actions are saved to widgets and shown in the Alert Overview page inside Google SecOps Case for later use.

---

### MAC Lease Lookup

Automate retrieval of MAC lease information from Infoblox, enriching the incident with network context and device assignment details. [Download](<./playbooks/MAC Lease Lookup.zip>)

![Playbook - MAC Lease Lookup](<./screenshots/playbooks/MAC Lease Lookup.png>)

**Flow:**

- **MAC Input (block)**
  - This block collects the MAC address from the user or automation [here](#indicator-input).

- **DHCP Lease Lookup (action)**
  - Returns DHCP lease information for the provided MAC address from Infoblox NIOS.

- **List Host Info (action)**
  - Retrieves host records from Infoblox including hostname, associated IPv4/IPv6 addresses (A/AAAA records), PTR records, DNS view information, and any configured extensible attributes.

> **Note:** All responses from actions are saved to widgets and shown in the Alert Overview page inside Google SecOps Case for later use.

---

### Bring Asset Data Back to Infoblox

Automate the creation of host records in Infoblox NIOS using asset data, ensuring network inventory accuracy and proper IP address management. [Download](<./playbooks/Bring Asset Data Back to Infoblox.zip>)

![Playbook - Bring Asset Data Back to Infoblox](<./screenshots/playbooks/Bring Asset Data Back to Infoblox.png>)

**Flow:**
- **Create Host Record (action):** Collects required asset information (hostname, IP addresses, view, comments, etc.) and Adds a host record with A/AAAA and optional PTR entries in Infoblox.

> **Note:** Response from the action is saved to widgets and shown in the Alert Overview page inside Google SecOps Case for later use.

---

## Blocks

A block is a re-usable set of actions and conditions that can be used in multiple playbooks. This acts as a wrapper for performing some set of actions, that are often performed in multiple playbooks.



### Indicator Input

This block is used to collect the Indicator Input. [Download](<./blocks/Indicator Input Block.zip>)

![Block - Indicator Input](<./screenshots/blocks/Indicator Input.png>)

**Input:**

- **Indicator** - Indicator value from the user or automation. 

**Output:**
- `Indicator` value

---
### Indicator Input Block-Unblock

This block is used to collect an indicator (domain, IP, etc.) and RP Zone for use in block/unblock playbooks. [Download](<./blocks/Indicator Input Block-Unblock Block.zip>)

![Block - Indicator Input Block-Unblock](<./screenshots/blocks/Indicator Input Block-Unblock.png>)

**Input:**

- **Indicator:** Indicator value (domain or IP) from the user or automation.
- **RP Zone:** Name of the Response Policy Zone (RPZ) from the user or automation.

**Flow:**

- **Create JSON (action)**
  - This action (XMLTOJson) is a part of [Functions power-up](https://cloud.google.com/chronicle/docs/soar/marketplace-and-integrations/power-ups/functions#xmltojson) provided by SecOps.
  - This action converts the XML created from the input values to JSON, which is then used in other actions.

**Output:**

- Returns the generated `JSON output (root)` containing both the indicator and RP zone values.

---

### Get RP Zone

This block is used to check if a specified Response Policy Zone (RPZ) exists in Infoblox NIOS, and create it if it does not exist. [Download](<./blocks/Get RP Zone Block.zip>)

![Block - Get RP Zone](<./screenshots/blocks/Get RP Zone.png>)

**Input:**

- **RP Zone:** The name of the Response Policy Zone (RPZ) to check or create. This value is provided by the user or automation.

**Flow:**


- **Get RP Zone Details (action)**
  - Retrieves configuration details for the specified RPZ from Infoblox NIOS.

- **Check RP Zone Exists**
  - Checks if the specified Response Policy Zone (RPZ) exists in Infoblox NIOS.

- **Create Response Policy Zone (action)**
  - If the RPZ does not exist, creates a new Response Policy Zone to define custom DNS responses.

---

### Validate and Search Indicator

This block is used to validate an indicator (IP address or domain) and search for the corresponding RPZ rule in Infoblox NIOS. [Download](<./blocks/Validate and Search Indicator Block.zip>)

![Block - Validate and Search Indicator](<./screenshots/blocks/Validate and Search Indicator.png>)

**Input:**

- **Indicator:** The value to be validated and searched (IP address or domain), provided by the user or automation.
- **RP Zone:** The name of the Response Policy Zone (RPZ) to search within.

**Flow:**

- **Detect IP Type (action)**
   - Uses the Detect IP Type action to determine if the indicator is an IPv4 address, IPv6 address, or domain.

- **Is IPv4 or IPv6 Address (condition)**
   - If the indicator is an IP address (IPv4/IPv6):
     - Uses **Search RPZ Rule IPAddress** to look for an RPZ rule matching the indicator in the specified RP Zone.
   - If the indicator is a Domain:
     - Uses **Search RPZ Rule Domain** to look for an RPZ rule matching the domain in the specified RP Zone.

- **Search for RPZ Rule (action)**
   - Searches for specific RPZ rule, returns details if found, or indicates that no rule exists for the indicator.

- **Return Result as JSON**
   - The search result (whether a rule is found or not) is returned as a JSON object, which can be used in downstream playbook actions for further logic or decision-making.

---

## Known Behaviours
- Currently, only the view from the 1st attached playbook will be set for the **Alert Overview** ([Reference](https://www.googlecloudcommunity.com/gc/SecOps-SOAR/Playbook-Widget-Behavior-in-Alert-Overview/td-p/888644)). For example, if a playbook is already attached to an alert, then the alert overview page will only reflect the view generated by that playbook. Hence, it is advised to enable only the required playbooks. Make sure that the trigger for the playbook is not set to All. Instead, the trigger should be set such that the playbook must only run of relevant set of alerts.

---
## Troubleshooting

In case of any failures when running any playbook, we can debug the same by running the playbook in the Simulator Mode. For more details, please refer to this guide: [Google SecOps playbooks - Simulator](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/working-with-playbook-simulator).

---
## References

- [Infoblox NIOS Documentation](https://docs.infoblox.com/space/NIOS/35400616/NIOS?attachment=https://docs.infoblox.com/download/attachments/15433781/NIOS%2520WAPI%25209.x%2520Reference%2520Guide.pdf&type=application/pdf&filename=PDF)
- [Google SecOps Playbooks Documentation](https://cloud.google.com/chronicle/docs/secops/google-secops-soar-toc#work-with-playbooks)

---

For questions or support, please contact your Infoblox or Google SecOps administrator.
