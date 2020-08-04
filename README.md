# Lab1-Firewall-block-the-inbound-traffic
## Instruction
### 1. Use azcopy copy following VHD to your own Storage Account
You can find your storage account URL by **Storage Accounts > Shared access signature > Generate SAS and connection string**. Add your container into the URL and use the following command.
```azcopy
azcopy copy "sourcevhdlink_below" "destination_yourStorageAccountURL"
```
### 2. Attach the broken disk to a rescue VM
> + Use storage explorer to clone the VHD before and rename to add '.vhd'.
> + Right click and create a snapshot.
> + Copy the snapshot and paste it to the container of the rescue VM.Then you can delete the snapshot created before if you want.
> + Connect the snapshot to the rescue VM as a disk: **Settings > Disks > Create and attach a new disk > edit**.
    - Source type: Storage blob
    Then click **Browse** to choose the snapshot.
    Click OK and Save.
### 3. Begin to fix the problem.
> + Start the rescue VM and connect to it. You can find a new disk in your computer.
> + Open powershell and paste the following command.

> [!NOTE]
> You need to check if the disk is F and if not, correct it in the command. Besides, each of these commands is going to require you to comfirm yes for if you really want to remove these registry keys. So for convenience, you can add '/f' at the end of them.
```Powershell
 reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
    
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet001\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile" /v DoNotAllowExceptions
    
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile" /v DoNotAllowExceptions
    REG DELETE "HKLM\BROKENSYSTEM\ControlSet002\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile" /v DoNotAllowExceptions
    
    reg unload HKLM\BROKENSYSTEM
```
### 4. Shut the VM down and detach the disk
> + Stop the rescue VM
> + Detach the disk: **Settings > Disks > Delete > Save**

The correct disk is in your rescue VM's container now.
