# Active Directory Lab in Azure


## Overview

Built a Windows Server Active Directory domain in Microsoft Azure, promoting a VM to domain controller and managing identities with PowerShell. Created department OUs, user accounts, and security groups, then enforced a password policy and USB storage restriction through Group Policy.

Watch me build this lab here!
https://www.loom.com/share/157f0704b0e3455cbc04b104f2a24955 
________________________________________
## 1. Create the Azure VM
1.	Sign in to the Azure portal.
2.	Search for Virtual machines.
3.	Select Create > Azure virtual machine.
4.	Configure the VM:

| Setting        | Value                                 |
| -------------- | ------------------------------------- |
| Region         | East US                               |
| Image          | Windows Server 2025 Datacenter – Gen2 |
| Size           | Standard_B2s                          |
| Authentication | Password                              |
| Inbound ports  | RDP (3389)                            |
| OS disk        | Standard SSD                          |

5.	Select Review + create, then select Create.
6.	Wait for deployment to finish and start the VM.
7.	Select Connect > RDP, download the RDP file, and connect with the local Remote Desktop app.
8.	In Remote Desktop, enable Clipboard under Show Options > Local Resources before connecting.

<img width="975" height="721" alt="image" src="https://github.com/user-attachments/assets/a2e2db5d-cdb3-4610-ae23-3aa2ce47f1d4" />
<img width="975" height="197" alt="image" src="https://github.com/user-attachments/assets/dcac5b8e-11a4-419a-8c19-50002d637e24" />

## 2. Install AD DS and GPMC
1.	Log in to the Windows Server VM.
2.	Open Server Manager.
3.	Select Manage > Add Roles and Features.
4.	Choose Role-based or feature-based installation.
5.	Select the local server.
6.	Select Active Directory Domain Services.
7.	When prompted, select Add Features.
8.	Continue through the wizard and select Install.
9.	Open PowerShell as Administrator.
10.	Install Group Policy Management Console:
## PowerShell Command
Install-WindowsFeature -Name GPMC
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

<img width="733" height="345" alt="image" src="https://github.com/user-attachments/assets/285c423b-6da8-4a0b-beb7-0cf3f5a72f77" />

 <img width="897" height="555" alt="image" src="https://github.com/user-attachments/assets/fde5f595-7bff-474c-afb0-2d6680e96280" />

<img width="792" height="839" alt="image" src="https://github.com/user-attachments/assets/fb93b237-dd45-4ae6-bd8e-22939eb79a0a" />

# 3. Promote the Server
1.	In Server Manager, select the yellow notification flag.
2.	Select Promote this server to a domain controller.
3.	Select Add a new forest.
4.	Set the root domain name:
lab.local
5.	Create and securely record the DSRM password.
6.	Keep the default DNS and NetBIOS settings.
7.	Select Install.
8.	Allow the server to restart.
9.	Sign back in

The promotion wizard creates the forest and configures domain-controller options such as DNS and the DSRM password.

<img width="591" height="327" alt="image" src="https://github.com/user-attachments/assets/8d268545-3def-4fe2-89a8-1480e8f84ce6" />

## 4. Create OUs and Groups
1.	In Server Manager, select Tools > Active Directory Users and Computers.
2.	Expand the lab.local domain.
3.	Right-click the domain, choose New > Organizational Unit.
4.	Create these OUs:
•	IT
•	Finance
•	HR
•	Sales
•	Client Onboarding
5.	Inside each department OU, create a security group.
6.	Set Group scope to Global and Group type to Security.
PowerShell option:
<img width="975" height="149" alt="image" src="https://github.com/user-attachments/assets/1062cdb2-13f1-46ce-9f9c-87902ced383d" />

<img width="736" height="443" alt="image" src="https://github.com/user-attachments/assets/947c940c-9f2a-46c2-b8c0-58b2be02fada" />

<img width="689" height="369" alt="image" src="https://github.com/user-attachments/assets/087df8c1-a0e7-43d6-a063-94851bdd1632" />

## 5. Create Users
1.	Open PowerShell as Administrator.
2.	Run the entire script block below at once.
3.	Confirm each account appears in the appropriate OU.
4.	Confirm each account is a member of its department security group.
## Powershell
<img width="961" height="628" alt="image" src="https://github.com/user-attachments/assets/3daa94bd-ee87-4d7a-8a34-546b4684593a" />

<img width="975" height="416" alt="image" src="https://github.com/user-attachments/assets/67da38a3-4ba6-460b-baaa-9609025fc2c0" />
 
<img width="550" height="464" alt="image" src="https://github.com/user-attachments/assets/938f5e6d-53f9-4195-ada9-7d004771d167" />
<img width="375" height="105" alt="image" src="https://github.com/user-attachments/assets/b6cd37cf-38ef-4751-bb30-5d035be40499" />

## 6. Create the GPO
1.	In Server Manager, select Tools > Group Policy Management.
2.	Expand:

<img width="975" height="366" alt="image" src="https://github.com/user-attachments/assets/6664dfb6-48a2-4e3f-8ae5-c4fc49bd8fb6" />
 
3.	Right-click the IT OU.
4.	Select Create a GPO in this domain and Link it here.
5.	Name the GPO:
6.	Right-click the new GPO and select Edit.
GPOs can centrally manage computer and user settings, and they are normally processed at computer startup and user sign-in. 
________________________________________
7. Configure Password Policy
1.	In the Group Policy Management Editor, go to:
Computer Configuration
> Policies,
> Windows Settings,
> Security Settings,
> Account Policies,
> Password Policy
2.	Open Minimum password length.
3.	Set the value to 14 
4.	Open Password must meet complexity requirements.
5.	Select Enabled.
6.	Select Apply and OK.
<img width="975" height="421" alt="image" src="https://github.com/user-attachments/assets/cf465632-dbd3-43e7-8dd7-2e7c1f8a6ff3" />
 

## 8. Restrict USB Storage
1.	In the same GPO editor, go to:
Computer Configuration
> Policies,
> Windows Settings
> Administrative Templates,
> System,
> Removable Storage Access
2.	Open All Removable Storage classes: Deny all access.
3.	Select Enabled.
4.	Select Apply and OK.
<img width="975" height="555" alt="image" src="https://github.com/user-attachments/assets/f6bd4325-1b75-41ca-b494-ed9a387aacf3" />

________________________________________
