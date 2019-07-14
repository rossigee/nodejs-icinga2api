# icinga2
Icinga2 Remote api module. You can create, remove, update, etc of icinga2 objects

[Icinga2 api doc](https://docs.icinga.com/icinga2/latest/doc/module/icinga2/chapter/icinga2-api)

## Installation

    npm install icinga2-api
    
### Getting started
Example

1. Check if the host "4demo" exist in monitoring.
2. If not, create host "4demo".

## Updated
The parameters 'group' and 'onServer' was removed from methode 'createHost' and 'createService'

``` js
const icingaapi = require('icinga2-api');

var icingaServer = new icingaapi("icingas2server.local", "5665", "apiUser", "apiUserPass");
    icingaServer.getHostState("4demo", function (err, result) {
        if (err) {
            if (err.Statuscode == "404") {
                console.log("Host 4demo on monitoring was not found, create one");
                
                // Create a host on icinga2.
                icingaServer.createHost("passive-host", "4demo", "4Demo Server", function (err, result) {
                    if (err) {
                        console.error(err);
                    } else {
                        console.log("Host 4demo was created successfull");
                    }
                });
            }
            console.error(err);
        } else {
            console.log(result);
        }
    })
```
## Methods
 - getServices = function (callback)
 - getService = function (ServerName, ServiceName, callback)
 - getHosts = function (callback)
 - getHost = function (ServerName, callback)
 - getHostFiltered = function (filter, callback)
 - getServiceFiltered = function(filter, callback)
   * filter = 
      ```js
        {
            "filter": "service.vars.Backup_Server == servicename",
            "filter_vars": {
                "servicename": "backupServer"
            }
        }
      ```
 - getServiceWithState = function (state, callback)
    * state = 0/1/2
 - createHost = function (template, host, displayname, callback)
    * template - icinga2 host-template
    * onServer = "dockerHost", set a custom variable in icinga2
 - createService = function (template, host, service, displayname, callback)
 - createServiceCustom = function (serviceObj, host, service, callback)
    * serviceObj = 
        ```js
        var serviceObj = JSON.stringify({
            "templates": ["passive-service"],
            "attrs": {
                "display_name": "Service Top",
                "vars.group": "adito",
                "vars.server": "dockerdmz"
            }
        })
        ```
 - createHostCustom = function (hostObj, host, callback)
    * hostObj
        ```js
        var hostBody = JSON.stringify({
            "templates": ["passive-host"],
            "attrs": {
                "display_name": "4Demo Server",
                "vars.group": "adito",
                "vars.server": "dockerdmz"
            }
        })
        ```
 - createServiceCustom = function (serviceObj, host, service, callback)
     * serviceObj
        ```js
        var serviceBody = JSON.stringify({
            "templates": ["passive-service"],
            "attrs": {
                "display_name": "Service on 4Demo Server",
            }
        })
 - deleteHost = function (host, callback)
 - deleteService = function (service, host, callback)
 - setHostDowntime = function (dObj, hostname, callback)
    * dObj (downtime object)
        ```js
            var dObj = {
                'type': 'Host',
                'start_time': actTime,
                'end_time': endtime,
                'author': 'icingaadmin',
                'comment': "Downtime for Backup"
            }
        ```
 - setFilteredDowntime = function (dFilter, callback)
    * dFilter (example: set downtime for all server where custom variable server = dockerhost)
        ```js 
            var setDownTimeObject = {
                'type': 'Host',
                'start_time': actTime,
                'end_time': endtime,
                'author': 'icingaadmin',
                'comment': "Downtime for Backup",
                'filter': 'host.vars.server == server',
                'filter_vars': {
                    'server': "dockerhost"
                }
            }
        ```
 - removeFilteredDowntime = function (dFilter, callback)
    * dFilter(remove all Downtimes with autor "icingaadmin" and custom vars server "dockerhost"
        ```js
            var removeDownTimeObject = ({
                "type": "Downtime",
                "filter": "downtime.author == filterAuthor && host.vars.server == filteredHost",
                "filter_vars": {
                    "filterAuthor": "icingaadmin",
                    "filteredHost": "dockerhost"
                }
            })
        ```
 - disableHostNotification = function (hostname, callback)
 - setHostState = function (host, hostState, StateMessage, callback)
 - setServiceState = function (service, host, serviceState, serviceMessage, callback)
 - getHostState = function (hostName, callback)
 - getServiceTemplates = function(callback)
 - checkExistServiceTemplate = function(name, callback)
 - getHostTemplates = function(callback)
 - checkExistHostTemplate = function(name, callback)
 - setServicePerfdata = function (service, server, state, output, perfarr, callback)
    * perfarr (Data array) - show process-check-result in [icinga2 api doc](https://docs.icinga.com/icinga2/latest/doc/module/icinga2/chapter/icinga2-api)
        ```js
            var perfdataArr = ["Memory in %=" + memUsedinPerc + "%;" + aditoMemWarn + ";" + aditomemErr + ";0"]
        ```
 - setHostPerfdata = function (server, state, output, perfarr, callback)
 - updateHostAttr = function (hostObj, host, callback)
    * hostObj
        ```js
        var hostObj = JSON.stringify({
            "templates": [icingaConf.templatehost],
            "attrs": {
                "display_name": "Adito Server Public",
                "vars.group": "adito",
                "vars.AditoServerName": "aditoServer",
                "vars.AditoServerVersion": "4.6.34",
                "vars.AditoStartupTime": "Mo.",
                "vars.CPUs": "4"
            })
        ```
 - updateServiceAttr = function (serviceObj, host, service, callback)

## Examples
Check [test.js](https://github.com/aditosoftware/nodejs-icinga2api/blob/master/test.js) for more details