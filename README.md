# Active Workspace Bypass Customization

## Deployment Instructions
### Source Code Deployment
**Copy** and **Paste** the **\src\bypass** directory into **%TC_ROOT%\aws2\stage**, then run the following commands in a Teamcenter command terminal:
``` cmd
cd %TC_ROOT%\aws2\stage
initenv.cmd
awbuild.cmd
```
___
### Stylesheet Modifications
Modify the Awp0UserSummary stylesheet using either the Rich Client or XRTEditor in Active Workspace.

**A sample of this stylesheet has been included in the solution media.**

``` html
<section titleKey="Bypass" >
    <htmlPanel declarativeKey="bypass" />
</section>
```
___
## Validation
- Login to the Active Workspace Client after successfully running the awbuild.cmd.
- Find an object which the logged in user does not have write access to **(Example in video of an object with Obsolete status)**
- Verify that the **Edit** &rarr; **Start Edit** command is not available when selecting the object.
- Select on the <img src="./typePerson48.svg" width="24" height="24"> icon in the bottom left corner of the client, then select **Profile**.
- Select the aw-checkbox labeled **Bypass Flag** to enable the bypass feature.
- Navigate back to the object and verify **Edit** &rarr; **Start Edit** is now available and changes can be made to the object properties.
- To disable Bypass, navigate back to Profile and uncheck the aw-checkbox.

https://user-images.githubusercontent.com/44880206/145594000-9b6e747a-3392-471e-a905-2b43470cf55b.mp4


___
## Solution Definition

### View
#### Elements
- aw-checkbox
- visible-when
#### Conditions
- **Group**: dba (ctx.userSession.props.role.uiValue=='DBA')
- **Role**: DBA (ctx.userSession.props.role.uiValue=='DBA')
- **Location**: User Profile (ctx.userSession.props.user.dbValue==ctx.selected.uid)

### Viewmodel
#### Imports
``` json
"js/aw-checkbox.directive",
"js/visible-when.directive"
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


