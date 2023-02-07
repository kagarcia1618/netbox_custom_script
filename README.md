# Netbox Custom Script
This is a repository of my NetBox custom scripts for the community.

## Device Type Provisioning
If you have the following use cases, this NetBox custom script might be of help to you:
- Automated NetBox device type record importing or updating from within NetBox custom script UI
- Import new record or update existing record based on [NetBox devicetype library](https://github.com/netbox-community/devicetype-library) templates
- Import or update only selected hardware models only if found in NetBox devicetype library templates

Tested working on:
- NetBox v3.4.2 (Native Linux installation)
- Python 3.9.9

***Note:*** *This guide was made in the assumption that you have installed your NetBox following the [NetBox Installation Guide](https://docs.netbox.dev/en/stable/installation/).*

### Procedure
***Note:*** Steps 1 to 3 are mandatory as I have introduced here a custom class object and a jinja template on a separate directory. Please be guided accordingly.
1. Clone this repository into default NetBox script root directory.
   ```
   cd /opt/netbox/netbox/scripts && git clone https://github.com/kagarcia1618/netbox_custom_script.git
   ```
2. Update the default scripts root directory to the cloned git directory.
   ```
   SCRIPTS_ROOT = '/opt/netbox/netbox/scripts/netbox_custom_script'
   ```
3. Restart NetBox system process.
   ```
   sudo systemctl restart netbox netbox-rq
   ```
4. Go to your scripts page and click the link to the script. You should be able to see something like this if everything works fine:

   ![image](https://user-images.githubusercontent.com/17977336/217207228-4c4e6670-dfd3-4da2-a49e-38875f6209ee.png)
   
5. You can now create a yaml file of the hardware model/s that you want import or update in your NetBox records. Please refer to the yaml file content format below:
   ```
   ---
   - vendor: Cisco
     part_numbers:
       - N9K-C93120TX
       - N9K-C93216TC-FX2
   - vendor: Arista
     part_numbers:
       - DCS-7010T-48
   ```
   `vendor` should be in title case of the vendor/manufacturer name
   
   `part-numbers` is a list that should contain at least a single or multiple partnumber item/s in all caps format

6. Ticking the `Update Existing Record` will allow the script to perform the following operation to your existing devicetype records:
   - **Import** existing devicetype object's related attributes (interfacetemplate, consoleport and powerport) record based on NetBox devicetype-library template of the same model.
   - **Update** existing devicetype object's stored and related attributes (interfacetemplate, consoleport and powerport) record based on NetBox devicetype-library template of the same model.
   - **Delete** existing devicetype object's related attributes (interfacetemplate, consoleport and powerport) record based on NetBox devicetype-library template of the same model. If you have an existing interfacetemplate that have a different naming convention with the devicetype-library template, this record will be deleted and imported with a new one based from devicetype-library template.
   
7. Finally, run the script without committing changes first. An output summary will be shown as to what record changes will be made to your NetBox's records.
8. Once you have reviewed the summary report and you are okay with the changes to be made, you may now run the script with commit changes set to `True` to update your records.

This is the initial release of this script and I would be glad to hear your comments, suggestions or feedback.
