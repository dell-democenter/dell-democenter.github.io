# MODULE 2 - MULTI FACTOR AUTHENTICATION USING RSA


## Verify RSA SecurID Configuration

```Powershell
Get-PPDMmfa_securids
```
![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/64ef92d2-132d-494c-8e47-43e85bb4f107)

Disable MFA
```Powershell
Get-PPDMmfa_securids | Set-PPDMmfa_securids -is_enabled:$false
```
At this Point, you cannot re-enable it as you will not have the token to access
![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/e2cadbda-0454-40fb-a9a4-f693928ce1f7)


## BYPASS MULTIFACTOR AUTHENTICATION
First, we need to get the IdentityProvider having the users we want to disable MFA for.
In Our Case it is the local Provider

```Powershell
Get-PPDMidentity_providers -type local
```
![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/c6347849-ecca-46db-b111-9cb9258e621f)

To disable MFA for a local user, we need to provide the accountname 

```Powershell
Get-PPDMidentity_providers -type local | Set-PPDMmfa_bypass_accounts -accountname admin
```

List the Account bypass for local user admin  
```Powershell
Get-PPDMmfa_bypass_accounts -filter 'selector eq "local" and subject eq "admin"'
```

Remove the BypPass for an account
```Powershell
Get-PPDMmfa_bypass_accounts -filter 'selector eq "local" and subject eq "admin"' |  Remove-PPDMmfa_bypass_accounts
```



 [<<Module 1](./Module_1.md) This Concludes Module 2



