# xmcapi
Simple implementation of Python REST API client for Extreme Networks Management Center NBI

## Requrements
Both Python 2 and 3 are supported (tested on 2.7.18 and 3.9.0)

## Features
* Support for user credentials (HTTP Basic Authentication)
* Support for OAuth Authorization (HTTP Bearer Authorization)

## Testing
* Test with `python xmc_api.py`

## Usage
You can use `xmcapi` as external library in your project and import its's 
`emc_nbi` object. In addition you can make your own fake `xmclib.py` module with 
`emc_vars` dict to emulate XMC Scripting engine behavior which could be useful 
for local scripts testing. Example below:
```
### xmclib.py
emc_vars = {
    'serverIP': '10.100.0.40',  # XMC IP address
    'serverVersion': 'server version',
    'serverName': 'server host name',
    'time': 'current date at server (yyyy-MM-dd)',
    'date': 'current time at server (HH:mm:ss z)',
    'userName': 'root',  # XMC user name
    'userDomain': 'XMC user domain name',
    'auditLogEnabled': 'true/false if audit log is supported',
    'scriptTimeout': 'max script timeout in secs',
    'scriptOwner': 'scripts owner',
    'deviceName': 'DNS name of selected device',
    'deviceIP': '10.100.0.158',  # 'IP address of the selected device',
    'deviceId': 'device DB ID',
    'deviceLogin': 'login user for the selected device',
    'devicePwd': 'login password for the selected device',
    'deviceSoftwareVer': 'software image version number on the device',
    'deviceType': 'device type of the selected device',
    'deviceSysOid': 'device system object id',
    'deviceVR': 'device virtual router name',
    'cliPort': 'telnet/ssh port',
    'isExos': 'true/false. Is this device an EXOS device?',
    'family': 'device family name',
    'vendor': 'vendor name',
    'deviceASN': 'AS number of the selected device',
    'port': 'selected ports',
    'vrName': 'selected port(s) VR name',
    'ports': 'all device ports',
    'accessPorts': 'all ports which have config role access',
    'interSwitchPorts': 'all ports which have config role interswitch',
    'managementPorts': 'all ports which have config role management',
    'client_id': 'oSfXvoUrfV',  # XMC API Key
    'secret': '1a83fb17-c2bd-46ae-9ee1-9095533740cb',  # XMC API Key secret
    'password': 'password' # XMC user password
}
```
Your script file:
```
### test_query.py
from __future__ import print_function  # for Python 2.x
from xmcapi import emc_nbi
from xmclib import emc_vars

test_query = '''
{
    network {
        siteByLocation(location: "<location>") {
            vlans {
                name
                vid
            }
        }
    }
}
'''
qry_result = emc_nbi.query(test_query.replace(
    '<location>', '/World', 1))
for vlan in qry_result['network']['siteByLocation']['vlans']:
    print(vlan['name'], vlan['vid'])
```
Run the code: `python test_query.py`
