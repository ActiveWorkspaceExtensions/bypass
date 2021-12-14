# Active Workspace Bypass Customization
## Version 1.1 Release Notes
- Added command to the Global Navigation toolbar
### Images
- Added supporting icon images to [image](https://github.com/ActiveWorkspaceExtensions/bypass/tree/main/src/bypass/src/image) which will be included in deployment.
### Commands View Model
#### Commands
- enableBypassCommand  ![image](https://user-images.githubusercontent.com/44880206/146063865-dae7e6fc-7f6c-49bc-bc7f-dcd3df9d15a0.png)
- disableBypassCommand  ![image](https://user-images.githubusercontent.com/44880206/146063737-1128c872-0d4f-484b-8cdc-de89d7f30fc7.png)


#### Command Handlers
- Handlers call an action to either enable or disable bypass.
#### Command Placements
- Both enable/disable bypass are located on the aw_globalNavigationbar uiAnchor.
#### Actions
- **enableBypass:** SOA service to setUserSessionState value to 1 and subsequent event to reload page allowing refresh of ViewModel object permissions. 

- **disableBypass:** SOA service to setUserSessionState value to 1 and subsequent event to reload page allowing refresh of ViewModel object permissions.
#### On Event
``` json
    "onEvent": [
        {
            "eventId": "enableBypass.reload",
            "action": "reloadPage"
        },
        {
            "eventId": "disableBypass.reload",
            "action": "reloadPage"
        }
    ]
```
#### Conditions
```json
"conditions": {
    "showEnableBypass": {
        "expression": " ctx.userSession.props.group_name.dbValue=='dba' && ctx.userSession.props.role.uiValue=='DBA' && ctx.userSession.props.fnd0bypassflag.dbValues[0] === '0'"
    },
    "showDisableBypass": {
        "expression": "ctx.userSession.props.group_name.dbValue=='dba' && ctx.userSession.props.role.uiValue=='DBA' && ctx.userSession.props.fnd0bypassflag.dbValues[0] === '1'"
        }
    }
```

___
## Overview
Customization to disable the rule tree. This gives unrestricted access to objects in Active Workspace.

## Deployment Instructions
### Source Code Deployment
**Copy** and **Paste** the **\src** directory into **%TC_ROOT%\aws2\stage**, then run the following commands in a Teamcenter command terminal:
``` cmd
cd %TC_ROOT%\aws2\stage
initenv.cmd
awbuild.cmd
```
___
### Stylesheet Modifications
Modify the Awp0UserSummary stylesheet using either the Rich Client or XRTEditor in Active Workspace.

**A sample of this [stylesheet](https://github.com/ActiveWorkspaceExtensions/bypass/blob/main/Awp0UserSummary.xml) has been included in the solution media.**

Add the following section to the stylesheet to expose the Bypass Flag checkbox in the User summary stylesheet.
``` html
<section titleKey="Bypass" >
    <htmlPanel declarativeKey="bypass" />
</section>
```
___
## Validation
- Login to the Active Workspace Client after successfully running the awbuild.cmd.
- Find an object which the logged in user does not have write access to **(Example in video of an object with *Obsolete* status)**
- Verify that the **Edit** &rarr; **Start Edit** command is not available when selecting the object.
- Select on the <img src="./typePerson48.svg" width="24" height="24"> icon in the bottom left corner of the client, then select **Profile**.
- Select the aw-checkbox labeled **Bypass Flag** to enable the bypass feature.
- Navigate back to the object and verify **Edit** &rarr; **Start Edit** is now available and changes can be made to the object properties.
- To disable Bypass, navigate back to Profile and uncheck the aw-togglebutton.

https://user-images.githubusercontent.com/44880206/145828924-706769a4-443f-497a-91e5-f349b4fe0f37.mp4



___
## Solution Definition

### View
#### Elements
- aw-panel
- aw-panel-section
- aw-togglebutton
- visible-when
#### Conditions
- **Group**: dba (ctx.userSession.props.role.uiValue=='DBA')
- **Role**: DBA (ctx.userSession.props.role.uiValue=='DBA')
- **Location**: User Profile (ctx.userSession.props.user.dbValue==ctx.selected.uid)

### View Model
#### Imports
``` json
"js/visible-when.directive",
"js/aw-togglebutton.directive",
"js/aw-panel.directive",
"js/aw-panel-section.directive"
```
#### Actions
``` json
"toggle": {
    "actionType": "TcSoaService",
    "serviceName": "Core-2007-12-Session",
    "method": "setUserSessionState",
    "inputData": {
        "pairs": [{
            "name": "fnd0bypassflag",
            "value": "{{(ctx.userSession.props.fnd0bypassflag.dbValue) ? '1' : '0'}}"
        }]
    }
}
```
#### SOA
![image](https://user-images.githubusercontent.com/44880206/145596241-9ad84fa1-9f0a-4426-bd93-fcfd94616250.png)




