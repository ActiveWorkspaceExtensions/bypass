---
Active Workspace Bypass Customization

---


# Active Workspace Bypass Deployment Instructions
## Source Code Deployment
**Copy** and **Paste** the **\src\bypass** directory into **%TC_ROOT%\aws2\stage**, then run the following commands in a Teamcenter command terminal:
``` cmd
cd %TC_ROOT%\aws2\stage
initenv.cmd
awbuild.cmd
```
___
## Stylesheet Modifications
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
- Select on the user avatar icon in the bottom left corner of the client, then select **Profile**.
- Select the checkbox labeled Bypass Flag to enable the bypass feature.
- 
___
## Solution Definition

