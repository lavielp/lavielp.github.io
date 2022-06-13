# Secrets Management PowerShell

## Situation

A homegrown secrets management solution called Get-StoredCredential has been overtaken by a new
PowerShell module called Microsoft.PowerShell.SecretManagement.  The module has several community
extensions that integrate with existing password management solutions including Last Pass.  Given
the homegrown solution has been overtaken by the circumstances of a vendor provided solution, it is
time to replace the Get-StoredCredential with the COTS solution.

## Target

Install the SecretsManagement module
Select a Password Management extension
Modify modules with new module calls

## Proposal

Transition over to the Secrets Management module with LastPass vault backing it.


## Process

1. Install SecretsManagement module

    ``` PowerShell
    Install-Module -Name Microsoft.PowerShell.SecretManagement -Repository PSGallery
    ```

1. Install the SecretStore module for a local vault, and register it but not as the default

    ``` PowerShell
    Install-Module -Name Microsoft.PowerShell.SecretStore -Repository PSGallery
    Register-SecretVault -Name SecretStore -ModuleName Microsoft.PowerShell.SecretStore
    ```
