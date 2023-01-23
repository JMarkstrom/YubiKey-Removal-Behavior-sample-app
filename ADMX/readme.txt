LICENSE:
--------
Created by: Jonas Markström
Last Updated: 2022-12-21
License: GNU

DISCLAIMER:
-----------
This feature is provided "as-is" without any warranty of any kind, either expressed or implied.

NOTE:
-----
This Administrative template (ADMX) adds the option to enable (default) or disable YubiKey removal behavior by setting a central GPO.
The template is designed to augment the existing credentialprovider.admx with an additional setting. 

INSTRUCTIONS:
-------------
1.) Copy the ADMX file (YubiKeyRemovalBehavior.admx) to the following location on your domain controller / server: C:\Windows\PolicyDefinitions
2.) Copy the ADML language file (YubiKeyRemovalBehavior.adml) to the following location: C:\Windows\PolicyDefinitions\en-US
3.) Restart the GPO Editor (if open)
4.) The settings are found when expanding (a GPO on:) 'Computer Configuration' > 'Policies' > 'Administrative Templates'> ' System' > 'Logon'

NOTE: Since the setting applies to the computer object, computers must have read access to the GPO.
