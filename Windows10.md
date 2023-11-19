<h1>Building a Windows 10 VM from scratch</h1>
This setup-process presumes that you have already installed some sort of virtualisation program already ( i.e VMware or Virtualbox or others)

From Microsoft Windows (https://www.microsoft.com/en-au/software-download/windows10), you'll want to grab a Windows 10 ISO in order to install a VM through your virtualisation software.

Afterwards, you can add that Win10 ISO onto a new VM through your virtualisation software (each software may have slightly different steps but the process is similiar) and wait for the VM to finish building.

<h2>Using STIG tool to make the Windows VM ML1 E8 Compliant</h2>

Credits to https://github.com/simeononsecurity/Windows-Optimize-Harden-Debloat-GUI for the STIG tool, this tool will save us time in manually configuring each policy manually and configure them all at once for us. 

Make sure you keep this file zipped and away from your main system as it can wipe important configurations if executed on your main system

(Note after running this tool **INSIDE OF YOUR WIN10 VM**, you'll want to create a new user login either through User Access Control or your virtualisation program to log back in)

![Audit_Install](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/63d5663a-d9aa-4349-aeb3-60a5fb83d51d)

<h2>Checking STIG installation</h2>

After a restart, we can begin to check if the STIG is working and we will be checking this through unknown scripts, password policy and user access control (UAC).

To check if unknown scripts are being blocked on our VM, we will create our own simple script to trying running through Windows PowerShell (PS)

Create a text document and we'll want to type

``` echo "Hello World"```

and click on "Save As..."
![Save As Script](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/8c08d6c8-127c-4dac-a1d4-b28c174e0844)

and name it something simple like ```Test_1.ps1```, .ps1 is needed as it recongnises the file as a PS script, under "Save as Type", click on "All files"

![Save as Config](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/9cb9065b-ec66-4d47-afbd-2ab5b3d48c8d)

We can then open up PS through the start menu and typing "Powershell", dragging our script file in and executing it and it should be blocked by policy 

![Script blocked](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/c301d40b-737c-4093-9fbe-74e22ef5f0f8)

This test ensures we cannot run unauthorised scripts on our VM as per STIG config.

<h3>User Access Control and Password Policy</h3>
Right now, after STIG configuration and logging back in, your local Windows 10 VM account should still be in administrator mode and be able to add new users.

In start menu, type user and access "Add, edit, or remove other users" 

![User Creation](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/cee86760-4c1e-4a7e-99cc-4ecc219e7c06)

We will try creating an account with a relatively weak but long password ```abcd1234```

![Password Policy abcd1234](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/e0b7e607-559a-4a0c-8579-49c61bd252e9)

With password policy configured properly, even though we are administrator status, we cannot create this account as the password is "too weak" by our STIG standards. 

![ABCD1234 password policy](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/b7879b8f-670e-444f-bc55-83a3c267df1e)

However, when try a more complicated password such as ```!#%&(Test24680```, we are able to create this account as it is complex enough by our standards

![Password Policy !#% (Test24680](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/ad985533-497f-4f86-a517-11561db23713)
![User Added after successful password policy](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/110ab5da-1185-46bb-91b0-d4d0c3bfbd89)

We can then switch to this newly created user and try to change UAC settings and we'll be blocked as per policy 


![UAC Blocked](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/76edf912-1ad0-4e23-9473-7a725ee62fea)
![User Perm Blocked](https://github.com/BYeungCyberSec/MockAudit/assets/150320582/f66bed26-383a-4b0b-a8d1-c1b4c8a32cb4)

We can also see on this local account, we cannot create new users as this function has been disabled for non-admin users.
