# About
Configurations to enable developer services, NavUserPassword authentication, and session timeout settings in Business Central.

<br>

# Enable Developer Services

#### Enable Developer Services.
```PowerShell
Set-NAVServerConfiguration -ServerInstance <ServerInstanceName> -KeyName DeveloperServicesEnabled -KeyValue true
```

#### Enable Debugging
```PowerShell
Set-NAVServerConfiguration -ServerInstance <ServerInstanceName> -KeyName EnableDebugging -KeyValue true
```

#### Restart the server instance.
```PowerShell
Restart-NAVServerInstance -ServerInstance <ServerInstanceName>
```

<br>

# NavUserPassword authentication

#### Create a user account.
```PowerShell
New-NAVServerUser -ServerInstance <ServerInstanceName> -UserName <UserName> -Password (Read-Host "Enter Password" -AsSecureString)
```

#### Set permissions.
```PowerShell
New-NAVServerUserPermissionSet -PermissionSetId SUPER -ServerInstance <ServerInstanceName> -UserName <UserName>
```
#### Set the NAVServerConfiguration.
```PowerShell
Set-NAVServerConfiguration -ServerInstance <ServerInstanceName>  -KeyName ClientServicesCredentialType -KeyValue NavUserPassword
```

#### Restart the server instance.
```PowerShell
Restart-NAVServerInstance -ServerInstance <ServerInstanceName>
```

#### Configure Web Server components.
```PowerShell
Set-NAVWebServerInstanceConfiguration -WebServerInstance <ServerInstanceName> -KeyName ClientServicesCredentialType -KeyValue NavUserPassword
```

#### Restart IIS service.
```PowerShell
iisreset /restart
```

<br>

# Session Timeout Settings

Specifies the amount of time that a connection between the Business Central Web client and the Business Central Server can remain idle before the session is stopped.

<br>

- Time span format: `[dd.]hh:mm:ss[.ff]`
    +  `dd`: days
    +  `hh`: hours
    +  `mm`: minutes
    +  `ss`: seconds
    +  `ff`: fractions of a second

<br>

By default, the `ClientServicesIdleClientTimeout` setting is set to `MaxValue`, which means no time limit, and the `SessionTimeout` setting is `00:20:00` (20 minutes). This means that when client connection is inactive, a session will close after 20 minutes.

<br>

Set new session timout
```PowerShell
Set-NAVWebServerInstanceConfiguration -WebServerInstance <ServerInstanceName> -KeyName SessionTimeout -KeyValue <timeout>
```