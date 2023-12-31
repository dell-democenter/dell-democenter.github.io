# Connect to PPDM and Explore the Status

## Connect to the System

```Powershell
Connect-PPDMsystem -fqdn ppdm-01.demo.local -trustCert
```

use *admin* and *Password123!* for connection

![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/29fc50fe-4b30-459d-9644-7e8f4434b125)

The Credentials are now stored in your Powershell session.  
The Bearer Token we use for subsequent Authentication will expire in 900 seconds. However, PPDM-pwsh wil calculate the Token Lifespan upon API Calls, and use the Refresh token to generate a new Token

![Alt text](image-46.png)

The refresh token will expire after 24hrs, so after 24 Hours your session will be disconnected and you have to connect again :

![Alt text](image-48.png)

If you get disconnected from PPDM, just re-run *Connect-PPDMsystem -fqdn ppdm-01.demo.local -trustCert* without getting asked for Credential  
*Hint: use ctrl-R to get the Command from search

## Review the Job State

Review the state of Protection jobs with the *Get-PPDMactivities* function
The Default will filter last 24hrs, the switch -days can specify duration in days

```Powershell
Get-PPDMactivities -PredefinedFilter PROTECTION_JOBS | ft
```

![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/113b95a6-97b4-4528-9afe-debbf4329742)

Review the state of Asset jobs with the *Get-PPDMactivities* function

```Powershell
Get-PPDMactivities -PredefinedFilter ASSET_JOBS 
```

![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/f63f36ca-4f99-4cef-8de4-113f4339ebf4)  

Review the state of System jobs with the *Get-PPDMactivities* function

```Powershell
Get-PPDMactivities -PredefinedFilter SYSTEM_JOBS 
```

![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/0e276da6-78f1-4615-a645-f1324f46c5e5)

From one of the last outputs, select a specific ID to view:
Review the PPDM Components state

```Powershell
Get-PPDMactivities -id <your acticity id from above>
```

## Review the Components ( similar to Support --> System Health View)

Review the PPDM Components state

```Powershell
Get-PPDMcomponents | ft
```

![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/8255007/486887d7-5d49-4bf4-a000-99274118d5f8)

Review the PPDM Activity Metrics

```Powershell
Get-PPDMactivity_metrics
```

![Alt text](./images/image-4.png)

Review the PPDM State Metrics

```Powershell
Get-PPDMactivity_metrics -states
```

![Alt text](./images/image-3.png)

## TLDR

[TLDR](./scripts/Module_1.ps1)

[<<Module 0](./Module_0.md) This Concludes Module 1 [Module 2](./Module_2.md)
