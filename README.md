# node-meraki-dashboard
A slightly opinionated node.js client library for using the Meraki Dashboard API.

## Table Of Contents

* [Documentation](#documentation)
  * [Admins](#admins)
  * [Clients](#clients)
  * [Config templates](#config-templates)
  * [Devices](#devices)
  * [Group policies](#group-policies)
  * [MX L3 Firewall](#mx-l3-firewall)
  * [MR L3 Firewall](#mr-l3-firewall)
  * [Networks](#networks)
  * [Organizations](#organizations)
  * [Phone assignments](#phone-assignments)
  * [Phone callgroups](#phone-callgroups)
  * [Phone contacts](#phone-contacts)
  * [Phone numbers](#phone-numbers)
  * [SAML roles](#saml-roles)
  * [SM](#sm)
  * [SSIDs](#ssids)
  * [Static routes](#static-routes)
  * [Switch ports](#switch-ports)
  * [VLANs](#vlans)
* [Examples](#examples)
  * [Using Promises (normal functions)](#using-promises-normal-functions)
  * [Using Promises (arrow functions)](#using-promises-arrow-functions)
  * [Using Async/Await](#using-asyncawait)

## Major changes when upgrading to v1.0.0
* The base url is now `api.meraki.com` from `dashboard.meraki.com` (see #2)
* Functions that took only one parameter for the `params` object are streamlined. The affected functions are as follows:
  * dashboard.clients
    * list
    * getPolicy
  * dashboard.devices
    * list
    * lldp_cdp_info
  * dashboard.networks.listAirMarshalScanResults

## Documentation
Official Documentation: https://dashboard.meraki.com/api_docs

All functions return a promise, which either resolves to the data received, or rejects with an error.

### Admins
```javascript
// List the dashboard administrators in this organization.
Promise<Array> dashboard.admins.list(String organization_id)

// Create a new dashboard administrator.
Promise<Object> dashboard.admins.create(String organization_id, Object params)

// Update an administrator.
Promise<Object> dashboard.admins.update(String organization_id, String admin_id, Object params)

// Revoke all access for a dashboard administrator within this organization.
dashboard.admins.revoke(String organization_id, String admin_id)
```

### Clients
```javascript
// List the clients of a device, up to a maximum of a month ago. The usage of each client is returned in kilobytes.
// If the device is a switch, the switchport is returned; otherwise the switchport field is null.
// The timespan must be less than or equal to 2592000 seconds (or one month).
Promise<Array> dashboard.clients.list(String serial, Number timespan)

// Return the policy assigned to a client on the network. The timespan must be less than or equal to 2592000 seconds (or one month).
Promise<Object> dashboard.clients.getPolicy(String network_id, String client_mac, Number timespan)

// Update the policy assigned to a client on the network.
Promise<Object> dashboard.clients.updatePolicy(String network_id, String client_mac, Object params)

// Return the splash authorization for a client, for each SSID they've associated with through splash.
Promise<Object> dashboard.clients.getSplashAuth(String network_id, String client_mac)

// Update a client's splash authorization.
Promise<Object> dashboard.clients.updateSplashAuth(String network_id, String client_mac, Object params)
```

### Config templates
```javascript
// List the configuration templates for this organization.
Promise<Array> dashboard.config_templates.list(String organization_id)

// Remove a configuration template.
dashboard.config_templates.remove(String organization_id, String template_id)
```

### Devices
```javascript
// List the devices in a network.
Promise<Array> dashboard.devices.list(String network_id, Number timespan)

// Return a single device.
Promise<Object> dashboard.devices.get(String network_id, String serial)

// Return an array containing the uplink information for a device.
Promise<Array> dashboard.devices.getUplinkInfo(String network_id, String serial)

// Update the attributes of a device.
Promise<Object> dashboard.devices.update(String network_id, String serial, Object params)

// Claim a device into a network.
dashboard.devices.claim(String network_id, Object params)

// Remove a single device.
dashboard.devices.remove(String network_id, String serial)

// List LLDP and CDP information for a device. The timespan must be less than or equal to 2592000 seconds (or one month).
Promise<Object> dashboard.devices.lldp_cdp_info(String network_id, String serial, Number timespan)
```

### Group policies
```javascript
// List the group policies in a network.
Promise<Array> dashboard.group_policies.list(String network_id)
```

### Hotspot 2.0 (Only applies to the Meraki DevNet Sandbox)
```javascript
// Return the Hotspot 2.0 settings for an SSID.
Promise<Object> dashboard.hotspot_v2.getSettings(String network_id, Number ssid)

// Update the Hotspot 2.0 settings for an SSID.
Promise<Object> dashboard.hotspot_v2.updateSettings(String network_id, Number ssid, Object params)
```

### MR L3 Firewall
```javascript
// Return the L3 firewall rules for an SSID on an MR network.
Promise<Array> dashboard.mr_l3_firewall.getRules(String network_id, Number ssid)

// Update the L3 firewall rules of an SSID on an MR network.
Promise<Array> dashboard.mr_l3_firewall.updateRules(String network_id, Number ssid, Object params)
```

### MX L3 Firewall
```javascript
// Return the L3 firewall rules for an MX network.
Promise<Array> dashboard.mx_l3_firewall.getRules(String network_id)

// Update the L3 firewall rules of an MX network.
Promise<Array> dashboard.mx_l3_firewall.updateRules(String network_id, Object params)
```

### MX VPN Firewall
```javascript
// Return the firewall rules for an organization's site-to-site VPN.
Promise<Array> dashboard.mx_vpn_firewall.getRules(String organization_id)

// Update firewall rules of an organization's site-to-site VPN.
Promise<Array> dashboard.mx_vpn_firewall.updateRules(String organization_id, Object params)
```

### MX Cellular Firewall
```javascript
// Return the firewall rules for an organization's site-to-site VPN.
Promise<Array> dashboard.mx_cellular_firewall.getRules(String network_id)

// Update firewall rules of an organization's site-to-site VPN.
Promise<Array> dashboard.mx_cellular_firewall.updateRules(String network_id, Object params)
```

### Networks
```javascript
// List the networks in an organization.
Promise<Array> dashboard.networks.list(String organization_id)

// Return a network.
Promise<Object> dashboard.networks.get(String network_id)

// Update a network.
Promise<Object> dashboard.networks.update(String network_id, Object params)

// Create a network.
Promise<Object> dashboard.networks.create(String organization_id, Object params)

// Delete a network.
dashboard.networks.delete(String network_id)

// Bind a network to a template.
dashboard.networks.bindToTemplate(String network_id)

// Unbind a network from a template.
dashboard.networks.unbindFromTemplate(String network_id)

// Return the site-to-site VPN settings of a network. Only valid for MX networks.
Promise<Object> dashboard.networks.getSiteToSiteVpn(String network_id)

// Update the site-to-site VPN settings of a network. Only valid for MX networks in NAT mode.
Promise<Object> dashboard.networks.updateSiteToSiteVpn(String network_id, Object params)

// The traffic analysis data for this network. Traffic Analysis with Hostname Visibility must be enabled on the network.
Promise<Array> dashboard.networks.getTrafficData(String network_id, Object params)

// List the access policies for this network. Only valid for MS networks.
Promise<Array> dashboard.networks.listAccessPolicies(String network_id)

// List Air Marshal scan results from a network. The timespan must be less than or equal to 2592000 seconds (or one month).
Promise<Array> dashboard.networks.listAirMarshalScanResults(String network_id, Number timespan)

// Return the Bluetooth settings for a network. Bluetooth settings must be enabled on the network.
Promise<Object> dashboard.networks.getBluetoothSettings(String network_id)

// Update the Bluetooth settings for a network. See the docs page for Bluetooth settings.
Promise<Object> dashboard.networks.updateBluetoothSettings(String network_id, Object params)
```

### Organizations
```javascript
// List the organizations that the user has privileges on.
Promise<Array> dashboard.organizations.list()

// Return an organization.
Promise<Object> dashboard.organizations.get(String organization_id)

// Update an organization.
Promise<Object> dashboard.organizations.update(String organization_id, Object params)

// Create a new organization.
Promise<Object> dashboard.organizations.create(Object params)

// Create a new organization by cloning the addressed organization.
Promise<Object> dashboard.organizations.clone(String organization_id, Object params)

// Claim a device, license key, or order into an organization. When claiming by order, all devices and licenses in
// that order will be claimed; licenses will be added to the organization and devices will be placed in the organization's
// inventory. These three types of claims are mutually exclusive and cannot be performed in one request.
dashboard.organizations.claimDevice(String organization_id, Object params)

// Return the license state for an organization.
Promise<Object> dashboard.organizations.getLicenseState(String organization_id)

// Return the inventory for an organization.
Promise<Array> dashboard.organizations.getInventory(String organization_id)

// Return the SNMP settings for an organization.
Promise<Object> dashboard.organizations.getSnmpSettings(String organization_id)

// Update the SNMP settings for an organization.
Promise<Object> dashboard.organizations.updateSnmpSettings(String organization_id, Object params)

// Return the third party VPN peers for an organization.
Promise<Array> dashboard.organizations.getThirdPartyVpnPeers(String organization_id)

// Update the third party VPN peers for an organization.
Promise<Array> dashboard.organizations.updateThirdPartyVpnPeers(String organization_id, Object params)
```

### Phone assignments
```javascript
// List all phones in a network and their contact assignment.
Promise<Array> dashboard.phone_assignments.list(String network_id)

// Return a phone's contact assignment.
Promise<Object> dashboard.phone_assignments.get(String network_id, String serial)

// Assign a contact and number(s) to a phone.
Promise<Object> dashboard.phone_assignments.assign(String network_id, String serial, Object params)

// Remove a phone assignment (unprovision a phone).
dashboard.phone_assignments.delete(String network_id, String serial)
```

### Phone callgroups
```javascript
// List all call groups in a network.
Promise<Array> dashboard.phone_callgroups.list(String network_id)

// Show a call group's details.
Promise<Object> dashboard.phone_callgroups.get(String network_id, String call_group_id)

// Create a new call group.
Promise<Object> dashboard.phone_callgroups.create(String network_id, Object params)

// Update a call group's details. Only submit parameters you would like to update. Omitting any parameters will leave them as-is.
Promise<Object> dashboard.phone_callgroups.update(String network_id, String call_group_id, Object params)

// Delete a call group.
dashboard.phone_callgroups.delete(String network_id, String call_group_id)
```

### Phone contacts
```javascript
// List the phone contacts in a network.
Promise<Array> dashboard.phone_contacts.list(String network_id)

// Add a contact.
Promise<Object> dashboard.phone_contacts.add(String network_id, String name)

// Update a phone contact. Google Directory contacts cannot be modified.
Promise<Object> dashboard.phone_contacts.update(String network_id, String contact_id, String name)

// Delete a phone contact. Google Directory contacts cannot be removed.
dashboard.phone_contacts.delete(String network_id, String contact_id)
```

### Phone numbers
```javascript
// List all the phone numbers in a network.
Promise<Array> dashboard.phone_numbers.listAll(String network_id)

// List the available phone numbers in a network.
Promise<Object> dashboard.phone_numbers.listAvailable(String network_id)
```

### SAML roles
```javascript
// List the SAML roles for this organization.
Promise<Array> dashboard.saml_roles.list(String organization_id)

// Return a SAML role.
Promise<Object> dashboard.saml_roles.get(String organization_id, String role_id)

// Update a SAML role.
Promise<Object> dashboard.saml_roles.update(String organization_id, String role_id, Object params)

// Create a SAML role.
Promise<Object> dashboard.saml_roles.create(String organization_id, Object params)

// Remove a SAML role.
dashboard.saml_roles.delete(String organization_id, String role_id)
```

### SM

#### Cisco Clarity
```javascript
// Create a new profile containing a Cisco Clarity payload.
Promise<Object> dashboard.sm.cisco_clarity.createProfile(String network_id, Object params)

// Update an existing profile containing a Cisco Clarity payload.
Promise<Object> dashboard.sm.cisco_clarity.updateProfile(String network_id, String profile_id, Object params)

// Add a Cisco Clarity payload to an existing profile.
Promise<Object> dashboard.sm.cisco_clarity.addPayload(String network_id, String profile_id, Object params)

// Get details for a Cisco Clarity payload.
Promise<Object> dashboard.sm.cisco_clarity.getPayloadDetails(String network_id, String profile_id)

// Delete a Cisco Clarity payload. Deletes the entire profile if it's empty after removing the payload.
Promise<Object> dashboard.sm.cisco_clarity.deletePayload(String network_id, String profile_id)
```

#### Cisco Umbrella
```javascript
// Create a new profile containing a Cisco Umbrella payload.
Promise<Object> dashboard.sm.cisco_umbrella.createProfile(String network_id, Object params)

// Update an existing profile containing a Cisco Umbrella payload.
Promise<Object> dashboard.sm.cisco_umbrella.updateProfile(String network_id, String profile_id, Object params)

// Add a Cisco Umbrella payload to an existing profile.
Promise<Object> dashboard.sm.cisco_umbrella.addPayload(String network_id, String profile_id, Object params)

// Get details for a Cisco Umbrella payload.
Promise<Object> dashboard.sm.cisco_umbrella.getPayloadDetails(String network_id, String profile_id)

// Delete a Cisco Umbrella payload. Deletes the entire profile if it's empty after removing the payload.
Promise<Object> dashboard.sm.cisco_umbrella.deletePayload(String network_id, String profile_id)
```

#### Cisco Polaris
```javascript
// Create a new Polaris app.
Promise<Object> dashboard.sm.cisco_polaris.createApp(String network_id, Object params)

// Update an existing Polaris app.
Promise<Object> dashboard.sm.cisco_polaris.updateApp(String network_id, String app_id, Object params)

// Get details for a Cisco Polaris app if it exists.
Promise<Object> dashboard.sm.cisco_polaris.getAppDetails(String network_id, String bundle_id)

// Delete a Cisco Polaris app.
Promise<Object> dashboard.sm.cisco_polaris.deleteApp(String network_id, String app_id)
```

#### Misc. Stuff
```javascript
// List the devices enrolled in an SM network with various specified fields and filters.
Promise<Object> dashboard.sm.listDevices(String network_id)

// Add, delete, or update the tags of a set of devices.
Promise<Object> dashboard.sm.editTags(String network_id, Object params)

// Modify the fields of a device.
Promise<Object> dashboard.sm.editFields(String network_id, Object params)

// Lock a set of devices.
Promise<Object> dashboard.sm.lockDevices(String network_id, Object params)

// Wipe a device.
Promise<Object> dashboard.sm.wipeDevice(String network_id, Object params)

// Force check-in a set of devices.
Promise<Object> dashboard.sm.forceCheckInDevices(String network_id, Object params)

// Move a set of devices to a new network.
Promise<Object> dashboard.sm.moveDevices(String network_id, Object params)

// List all the profiles in the network.
Promise<Object> dashboard.sm.listProfiles(String network_id)
```

### SSIDs
```javascript
// List the SSIDs in a network.
Promise<Array> dashboard.ssids.list(String network_id)

// Return a single SSID.
Promise<Object> dashboard.ssids.get(String network_id, String ssid)

// Update the attributes of an SSID.
Promise<Object> dashboard.ssids.update(String network_id, String ssid, Object params)
```

### Static routes
```javascript
// List the static routes for this network.
Promise<Array> dashboard.static_routes.list(String network_id)

// Return a static route.
Promise<Object> dashboard.static_routes.get(String network_id, String sr_id)

// Update a static route.
Promise<Object> dashboard.static_routes.update(String network_id, String sr_id, Object params)

// Add a static route.
Promise<Object> dashboard.static_routes.add(String network_id, Object params)

// Delete a static route from a network.
dashboard.static_routes.delete(String network_id, String sr_id)
```

### Switch ports
```javascript
// List the switch ports for a switch.
Promise<Array> dashboard.switch_ports.list(String serial)

// Return a switch port.
Promise<Object> dashboard.switch_ports.get(String serial, Number port_number)

// Update a switch port.
Promise<Object> dashboard.switch_ports.update(String serial, Number port_number, Object params)
```

### VLANs
```javascript
// List the VLANs for this network.
Promise<Array> dashboard.vlans.list(String network_id)

// Return a VLAN.
Promise<Object> dashboard.vlans.get(String network_id, String vlan_id)

// Update a VLAN.
Promise<Object> dashboard.vlans.update(String network_id, String vlan_id, Object params)

// Add a VLAN.
Promise<Object> dashboard.vlans.add(String network_id, Object params)

// Delete a VLAN from a network.
dashboard.vlans.delete(String network_id, String vlan_id)
```

## Examples
### Using Promises (normal functions)
```javascript
var dashboard = require('node-meraki-dashboard')(apiKey)
dashboard.organizations.list()
  .then(function(data) {
    console.log(data)
  })
  .catch(function(error) {
    console.log(error)
  });
```

### Using Promises (arrow functions)
```javascript
var dashboard = require('node-meraki-dashboard')(apiKey)
dashboard.organizations.list()
  .then(data => console.log(data))
  .catch(error => console.log(error));
```

### Using Async/Await
```javascript
var dashboard = require('node-meraki-dashboard')(apiKey)

(async function() {
  try {
    var orgList = await dashboard.organizations.list();
    console.log(orgList);
  } catch (error) {
    console.log(error);
  }
})();
```