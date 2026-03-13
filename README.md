# wazuh-ntfy
NTFY integration for Wazuh. notifications
- Create issues if you encounter bugs or have questions
- If you have any improvements you can create a pr

## Creating custom integrations
-   Copy the script `custom-ntfy` to `/var/ossec/integrations/`   
    (Example: `sudo nano /var/ossec/integrations/custom-ntfy`)
-   Modify access flags: `sudo chmod 750 /var/ossec/integrations/custom-ntfy`
-   Change owner of script: `sudo chown root:wazuh /var/ossec/integrations/custom-ntfy`

## Customization
You can change the variable `icon_url` in the script to an url for the Wazuh icon. \
The icon will be displayed in the NTFY phone app. \
**Note**: Only **JPEG** and **PNG** are supported. \
The default value of **None** just omits the image.

Example (In the **custom-ntfy** file, line 8):
```python
# User setting
icon_url = "https://example.com/wazuh.png"
```

## Adding integration to manager ossec.conf (in Wazuh)
1. Open your wazuh web page
2. Go to **Server management** > **Settings** > **Edit configuration**
3. Add a config like below

Example configs (Same indentation as the **global** snippet): \
Change the **hook_url** and **api_key** (mandatory) and optionally the **level** and **rule_id**
```xml
  <integration>
    <name>custom-ntfy</name>
    <hook_url>https://ntfy.example.com/wazuh</hook_url>
    <api_key>VERYSECRETKEY</api_key>
    <alert_format>json</alert_format>
    <level>8</level>
  </integration>
  
  <integration>
    <name>custom-ntfy</name>
    <hook_url>https://ntfy.example.com/wazuh</hook_url>
    <api_key>VERYSECRETKEY</api_key>
    <alert_format>json</alert_format>
    <rule_id>504, 506, 503</rule_id>
  </integration>
```

## Special features
You can add option parameters to my script. \
There are following options:
- Exclude host from generating notifications (blacklist) \
`<options>{"agentname_exclusion": ["host1","host2"]}</options>`
- Exclude rule ids from generation notifications (blacklist) \
`<options>{"ruleid_exclusion": ["1234","2345"]}</options>`
- Only include mentioned hosts in notifications (whitelist) \
`<options>{"agentname_inclusion": ["host1","host2"]}</options>`
- Only include mentioned rule ids in notifications (whitelist) \
`<options>{"ruleid_inclusion": ["1234","2345"]}</options>`

**Example**: All hosts should generate alerts at **level 9** except for **host-32** and **host-24**. Those should only trigger at **level 12**. Following config will do just that:

```xml
  <integration>
    <name>custom-ntfy</name>
    <hook_url>https://ntfy.example.com/wazuh</hook_url>
    <api_key>VERYSECRETKEY</api_key>
    <alert_format>json</alert_format>
    <level>9</level>
    <options>{"agentname_exclusion": ["host-32","host-24"]}</options>
  </integration>

  <integration>
    <name>custom-ntfy</name>
    <hook_url>https://ntfy.example.com/wazuh</hook_url>
    <api_key>VERYSECRETKEY</api_key>
    <alert_format>json</alert_format>
    <level>12</level>
    <options>{"agentname_inclusion": ["host-32","host-24"]}</options>
  </integration>
```
