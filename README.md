<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Group Policy inside Active Directory (Azure)</h1>
This demonsration outlines configuring Group Policy Objects within Active Directory. <p></p>
A Group Policy Object(GPO) is a centralized management feature in Windows networks that allows administrators to define and enforce specific configurations, security settings, and behaviors for users and computers within an Active Directory (AD) domain<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2025
- Windows 11 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Log into Domain Controller VM
- Navigating to Account Lockout Settings
- Attempt to log into client VM repeatedly with the wrong password
- Monitor and observe changes and implementation

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/de90e823-da2e-4300-8829-1691fe2e942b" />
</p>
<p>
Here we located the default domain policy in Active Directory. To reach this, I head to Forest > Domains > mydomain.com and then within mydomain.com, we right-clicked Default Domain Policy and clicked edit.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/828b2921-58d4-41cf-aeca-c643d7ddca3c" />
</p>
<p>
Next, I navigated to Account Lockout Policy. To reach this in the Group Policy Management Editor, I expand the following tree:
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy. <p> 
  Then I configured the Account lockout policies: 
  </p> **Account Lockout Duration: 30 minutes** (This setting specifies that when an account is locked out due to multiple failed login attempts, it will remain locked for 30 minutes. This helps to allow legitimate users regain access after a reasonable amount of time and also helps prevent malicious logon attempts) 
  </p> **Account Lockout Threshold: 5 Invalid Logon Attempts** (This setting is set to 5 because if an attacker attempts to guess passwords, they will be blocked after 5 failed attempt. At the same time, this threshold is set high enough to minimize disruptions for users who might occasionally mistype their password.)
  </p> **Allow Administrator Account Lockout: Enabled** (This setting ensures that even administrative accounts are subject to the account lockout policy. This adds an extra layer of security to priviledged accounts)
  </p> **Reset Account Lockout Counter After: 10 Minutes** (If no further failed login attempts are made within 10 minutes, the counter for failed attempts resets to zero.)
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/959d1ecc-e34b-4385-a172-89d2fe019cdd" />
</p>
<p>
Then, inside the command prompt, I typed the command, gpupdate /force. This ensures that the new policies are applied immediately and consistently across the domain.
</p>
<br />
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/e8cc4aa3-1a28-4508-9168-d74d432d9f8a" />
<img width="600" alt="image" src="https://github.com/user-attachments/assets/944bc8be-55e8-4128-a36c-cf22a2fd4547" />

</p>
<p>
Then I switched to my client-1 VM and logged in as fac.new, the client account I created, with the incorrect password repeatedly until it locks me out.
</p>
<br />
</p>
<br />


<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/a69a7759-8d42-4bfb-abd8-6abba44fc42c" />
</p>
<p>
Next, inside my domain controller, I searched for the user, went into properties account then unlocked the account for our user to log in.
</p>
<br />
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/b83dd0b8-b3b1-43d8-b176-bf090549772f" />
</p>
<p>
If we want to change his account password, I right-click on his account and click edit password.
</p>
<br />
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/3fa3a253-8419-411e-9f46-accb83193b8d" />
  <img width="600" alt="image" src="https://github.com/user-attachments/assets/dd77b9e4-5bef-4fc4-a62b-44ed88e426d3" />
</p>
<p>
If I need to disable a client's account for security purposes, I will right-click their account inside Active Directory and click disable account. Disabling accounts is recommended on unused accounts such as employees on extended leave (maternity leave, medical leave, sabbaticals) or if an account shows signs of compromise. This adds an extra layer of security without having to delete the entire account.
</p>
<br />
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/5309a4da-0df8-4e53-985e-408f554eebf8" />
</p>
<p>
Here, we could look in the event viewer security logs and see users that logged in. For our case, fac.new failed to log before being locked out so we could see that.
</p>
<br />
</p>
<br />

In the next section of the lab, I will configure DNS in a Windows Server environment, testing name resolution and DNS caching issue.
https://github.com/DannySom/DNS-configuration
