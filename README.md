<h1>Okta Administration: Onboarding, App Integration, and Provisioning</h1>

<h2>Description</h2>
Okta's SWA and SSO integration capabilities are essential in managing user access and enhancing security across various applications. This project leverages these features to streamline onboarding, integrate apps like Bamboo and Dropbox Business, and automate lifecycle management. By setting up automated provisioning, the configuration ensures that new users are efficiently granted the necessary permissions, reducing manual administrative overhead and improving overall user experience. 
<br />
<br />
<p align="center">

<h2>Environments Used </h2>

- <b>Okta</b>
- <b>Single Sign-On (SSO)</b>
- <b>Secure Web Authentication (SWA)</b>
- <b>Bamboo (Atlassian)</b>
- <b>Dropbox Business</b>
- <b>API Integration</b>

<h2>Okta-Mastered User Onboarding: </h2> 

<p align="center">
To start, we will create two users: Emma Peterson and Sam Grey. Emma will be assigned to a newly created ‘Dev’ group, while Sam will be added to the ‘Sales’ group. To add a user directly in the Okta Universal Directory, navigate to ‘Directory > Users > Add person.’
<br/>
<br/>
After configuring her account information and credentials, click 'Save and Add Another' to proceed with adding the user Sam Grey.
 <br/>
 <br/>
<img src="https://i.imgur.com/MV4g21a.png" alt="Add Okta User"/>
 <br/>
 <br/>
To grant users administrative privileges, go to their user profile and select 'Admin roles > Add individual admin privileges.' You can view the various permissions available in Okta at this link: https://help.okta.com/en-us/content/topics/security/administrators-admin-comparison.htm. For our use case, neither Sam nor Emma will require any admin privileges.
 <br/>
 <br/>
<img src="https://i.imgur.com/bhgQcSC.png" alt="Administrator Roles"/>
  <br/>
 <br/>
Next, we will add the newly created users to their respective groups. Navigate to ‘Directory > Groups > Add Group,’ name the groups as needed, and click ‘Add Users.’ Emma will be assigned to the ‘Dev’ group, and Sam will be assigned to the ‘Sales’ group.
<br/>
<br/>
<img src="https://i.imgur.com/Cpu8eTZ.png" alt="Sales Group"/>
 <br/>
 <br/>
<img src="https://i.imgur.com/X0wzQEw.png" alt="Dev Group"/>
 
<h2>Integrate Bamboo Using Secure Web Authentication (SWA)</h2> 
<p align="center">
We will now set up Bamboo using Secure Web Authentication (SWA). Okta SWA is a solution for integrating web applications that lack support for traditional Single Sign-On (SSO) protocols like SAML or OAuth. It enables SSO for these applications by automating the login process—capturing and securely storing user credentials and then using them to log in automatically, which simplifies access while ensuring security.
<br/>
<br/>
Navigate to ‘Applications > Browse App Catalog.’ Here, you can filter applications in the Okta Integration Network (OIN) by use case, functionality, and industry. Search for the Bamboo application and click ‘Add Integration.’ 
<br/>
<br/>
Bamboo by Atlassian is a CI/CD tool that automates the processes of building, testing, and deploying software. Since Emma is a developer, she will need access to Bamboo. Configuring SWA for apps that lack traditional SSO support will maintain both security and user experience. 
<br/>
<br/>
Configure Bamboo using the General Settings and Sign-On Options tabs, following the instructions provided in the Okta documentation: https://help.okta.com/en-us/content/topics/apps/apps_app_integration_wizard_swa.htm.
<br/>
<br/>
<img src="https://i.imgur.com/DWnRpRk.png" alt="Configure Bamboo"/>
<br/>
<br/>
Next, click ‘Done > Assign > Assign to Groups.’ Then, assign Bamboo to the ‘Dev’ group.
<br/>
<br/>
<img src="https://i.imgur.com/ZxoMXx6.png" alt="Bamboo Group Assignment"/>
<br/>
<br/>
To test the application integration and assignment, we will log into Okta as Emma. As depicted, Bamboo is now visible on Emma’s dashboard. To access Bamboo, ensure that the Okta Browser Plugin is downloaded and enabled. For details on downloading the Okta Browser Plugin, refer to: https://help.okta.com/en-us/content/topics/browser-plugin/browser-plugin-main.htm?cshid=csh-browser-plugin-main.
<br/>
<br/>
<img src="https://i.imgur.com/SICfr3W.png" alt="Okta User Dashboard - Bamboo App"/>
<br/>
<br/>

<h2>Mapping Microsoft Entra ID Attributes to Okta Attributes</h2> 
 <p align="center">
SAML claims are pieces of information about a user that are shared between different systems to help with logging in and accessing services, such as their name or email address. We will be editing our attributes and claims, which is essentially what information we want to send to Okta for authentication. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/Pzmbstz.png" alt="Attributes and Claims"/>
  <br/>
  <br/>
In addition to the default claims, we will add 'company name' and 'telephone number': 
<br/>
 <br/>
 <img src="https://i.imgur.com/c3QstfC.png" alt="Additional Claims"/>
 <br/>
 <br/>
Return back to Okta, and navigate to SAML Certifications > Edit > New Certificate. Enter the following values to update the placeholders: the 'IdP Issuer URI' is the 'Microsoft Entra Identifier', the 'IdP Single Sign-On URL' is the 'Login URL', and the 'IdP Signature Certificate' is the 'Base64 download'. 
<br/>
 <br/>
 <img src="https://i.imgur.com/DR3tYq1.png" alt="SAML Certificate"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/peyTXNw.png" alt="SAML Values"/>
  <br/>
 <br/>
 Be sure to make note of the values above. The 'Assertion Consumer Service URL' will be the 'Reply URL in Azure', and the 'Audience URI' will be the 'Identity Entity ID' in Azure.  
<br/>
<br/>
In Entra ID, update the placeholder values: 
<br/>
<br/>
<img src="https://i.imgur.com/lDoz3p7.png" alt="SAML Configuration Update"/>
 <br/>
 <br/>
Back in Okta, navigate to: Edit profile and mappings > Mappings. We will then unmap all attributes except 'username'. 
<br/>
<br/>
<img src="https://i.imgur.com/trmCk9u.png" alt="Unmap Attributes"/>
 <br/>
 <br/>
We will then navigate to 'Custom', and proceed to delete and recreate the attributes. For the attribute mapping, I will be referencing the following Okta documentation: https://help.okta.com/en-us/content/topics/provisioning/azure/azure-map-attributes.htm. Once the attributes are created, we will proceed with updating the mappings: 
 <br/>
 <br/>
<img src="https://i.imgur.com/nzHv3uy.png" alt="Create Mappings"/>
<br />
<br />
 <img src="https://i.imgur.com/r36IKV9.png" alt="Mapping Attributes"/>
 <br />
<br />
We will test the authentication by first navigating to our Okta application in Entra ID and assigning users to the application: Manage > users and groups > Add user/group 
<br />
<br />
<img src="https://i.imgur.com/519WGJ6.png" alt="Assign Users"/>
<br />
<br />
We are now ready to test the authentication! Navigate to Single sign-on > Test. 
<br />
<br />
<img src="https://i.imgur.com/XC7Yhjs.png" alt="Test the SSO"/>
<br />
<br />
<img src="https://i.imgur.com/ytzqWAk.png" alt="SSO Login"/>
<br />
<br />
For advanced testing and troubleshooting, you may optionally download the 'My Apps Secure Sign-in Extension' Chrome extension. As demonstrated below, we successfully authenticated!
<br />
<br />
<img src="https://i.imgur.com/fIbnlYn.png" alt="Successful SSO"/>
<h2>Key takeaways:</h2>
This project focuses on integrating federation between Microsoft Entra ID and Okta to create a seamless and secure authentication experience across hybrid environments. Federation in this context means connecting these systems so that users can log in once and gain seamless access to resources managed by either platform. This integration simplifies the authentication process, enhances security, and improves user experience across hybrid IT environments. The process begins with the essential step of setting up a "break-glass" account in Entra ID, an emergency backup to ensure continuous access even if primary authentication methods fail. Entra ID is then configured to act as the primary identity provider for Okta by utilizing the SAML 2.0 protocol. 
<br/>
<br/>
This involves establishing a secure communication link between Azure and Okta through the creation of an Okta enterprise application in Entra ID. The integration involves the setup of SAML claims, which define and map user attributes such as name, email, company name, and telephone number. These claims are fundamental in identifying and authenticating users across both systems. The final step in the implementation is thorough testing and validation to confirm that users can successfully log in to Okta applications using their Entra ID credentials.
<br/>
<br/>
Implementing federation between Azure and Okta offers significant benefits such as enhancing user experience by enabling Single Sign-On (SSO), allowing users to log in once and access multiple systems without repeated authentication. This streamlines the process and reduces the frustration of managing multiple passwords. From an administrative perspective, federation simplifies centralized management of user identities and access rights, reducing the overhead for IT teams. 
<br/>
<br/>
Federation is particularly valuable in several use cases such as for organizations operating in hybrid environments, where a mix of on-premises and cloud-based resources needs unified access management. It also facilitates secure cross-organizational collaboration, allowing external partners to access shared resources using their own identity systems. As companies expand their IT infrastructure, federation supports scalable identity solutions by seamlessly integrating multiple identity providers. 
<br/>
<br/>   
Additionally, it aids in regulatory compliance by offering a centralized, auditable log of user access across all systems. Key terms like federation, SAML, identity provider, attributes and claims, and break-glass accounts play crucial roles in understanding this project. By combining the strengths of Microsoft Entra ID and Okta through federation, this project creates a unified and efficient identity management solution that simplifies and secures user access to diverse systems.
<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
