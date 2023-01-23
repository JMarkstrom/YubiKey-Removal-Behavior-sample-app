# YubiKey Removal Behavior

### Table Of Contents
  * [About](https://github.com/JMarkstrom/YubiKey-Removal-Behavior/blob/main/README.md#about)
  * [Installation](https://github.com/JMarkstrom/YubiKey-Removal-Behavior/blob/main/README.md#installation)
  * [Usage](https://github.com/JMarkstrom/YubiKey-Removal-Behavior/blob/main/README.md#usage)
  * [Roadmap](https://github.com/JMarkstrom/YubiKey-Removal-Behavior/blob/main/README.md#roadmap)
  * [Contributing](https://github.com/JMarkstrom/YubiKey-Removal-Behavior/blob/main/README.md#contributing)
## About
The YubiKey Removal Behavior **sample** application is inspired by the native Windows "Smart Card Removal Behavior" feature and extends a similar level of control to FIDO2 Security Keys (YubiKeys) by locking a compatible Windows workstation when a YubiKey is removed. It does this by monitoring for YubiKey removal events and checking the value of the ```isEnabled``` registry key. If the value is set to ```1```, the code will lock the workstation.

**DISCLAIMER**: This application was created for demonstration purposes only. It is _not_ a product of Yubico and as such this application and accompanying Microsoft Administrative Templates (ADMX) are provided "as-is" without any warranty of any kind, either expressed or implied.

## Installation
Run the provided MSI (no interaction is required) and reboot the computer for changes to take effect.

💡The YubiKey Removal Behavior application is supported on 64 bit Windows 10 and Windows 11. For the application to work the target system must also have been configured for [Security Key sign-in](https://swjm.blog/three-ways-of-enforcing-security-key-sign-in-on-windows-10-windows-11-4f0f27227372).

Here is an example of a running the installer from command line: 

```bash
msiexec /i /qn "YubiKey Removal Behavior Tool.msi"
```
The Microsoft Installer (MSI) will install the application in the ```C:\Program Files\Yubico\YubiKey Removal Behavior\``` directory. Additionally, it will create two registry entries: 

1.  ```isEnabled``` with a default value of ```1``` under ```HKLM\Software\Policies\Yubico\Lock workstation on YubiKey removal\```. This entry is later controlled via Group Policy (GPO).
2. Reference to the ```YubiKeyRemovalBehavior.exe```executable is added to ```HKLM\Software\Microsoft\Windows\CurrentVersion\Run```. This entry makes sure that the tool is started on login.

**Note**: All registry keys are created in context of the machine in order to enforce functionality for all users.

## Usage

### Lock workstation on YubiKey removal
By default (no configuration required), the YubiKey Removal Behavior application will lock the workstation on YubiKey removal. This behavior can be turned OFF/ON using Group Policy (GPO), directly in host registry or using an MDM such as **Microsoft Endpoint Manager** / Intune (see roadmap).

#### Administrative template (ADMX)
The accompanying administrative template (ADMX) adds the option to _enable_ (default) or _disable_ YubiKey removal behavior by setting a central (or local) GPO. The template is designed to augment the existing ```credentialprovider.admx``` with an additional setting. To use this template:

1. Copy the ADMX file ```YubiKeyRemovalBehavior.admx``` to location: ```C:\Windows\PolicyDefinitions``` the on your domain controller (DC) / member server
2. Copy the ADML file ```YubiKeyRemovalBehavior.adml``` to location: ```C:\Windows\PolicyDefinitions\en-US```
3. Open the GPO editor (restart if open previously) and navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **System** > **Logon**
4. Double-click on 'YubiKey removal behavior' and toggle policy: ```Enabled``` (ON) or ```Disabled``` (OFF)
5. Click **Apply** and **OK**.

**Note**: Since the setting applies to the computer object, computers must have read access to the GPO.


### Disable the functionality
The application can be "disabled" by changing the ```isEnabled``` registry key to ```Disabled```
 using Group Policy (GPO) or by editing the host registry directly. Note that toggling the registry setting does not deactivate or uninstall the application -it simply stops the workstation from locking on YubiKey removal, but the application still runs(!)

### Uninstall
The application can be uninstalled from **Add/Remove Programs**, using **GPO** or **MDM**.

Here is an example of uninstalling from command line: 

```bash
msiexec /qn /x "YubiKey Removal Behavior Tool.msi"
```

## Roadmap
Possible improvements includes:
- Provide script or instruction to toggle ```isEnabled``` with Microsoft Endpoint Manager
- Using variables and/or relative paths in the installer (paths, registry keys)
- Reducing overall footprint / size of of application
- Making the application launch sooner at logon (run as a service?)
- Code signing

## Contributing
Any help on the above (see roadmap) is welcome. 
