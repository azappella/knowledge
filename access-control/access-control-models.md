# access control models

There are six main types of access control models you should understand:

1. Mandatory Access Control (MAC)
2. Discretionary Access Control (DAC)
3. Role-Based Access Control (RBAC)
4. Rule-Based Access Control
5. Attribute-Based Access Control (ABAC)
6. Risk-Based Access Control

In this article, we’ll define access control, explore the six access control models, describe the methods of logical access control and explain the different types of physical access control.
Access control and access control model

Access control is identifying a person doing a specific job, authenticating them by looking at their identification, then giving that person only the key to the door or computer that they need access to and nothing more. In the world of information security, one would look at this as granting an individual permission to get onto a network via a username and password, allowing them access to files, computers or other hardware or software the person requires and ensuring they have the right level of permission (i.e., read-only) to do their job. So, how does one grant the right level of permission to an individual so that they can perform their duties? This is where access control models come into the picture.

## 1. Mandatory Access Control (MAC)

The Mandatory Access Control (MAC) model gives only the owner and custodian management of the access controls. This means the end-user has no control over any settings that provide any privileges to anyone. There are two security models associated with MAC: Biba and Bell-LaPadula. The Biba model is focused on the integrity of information, whereas the Bell-LaPadula model is focused on the confidentiality of information. Biba is a setup where a user with lower clearance can read higher-level information (called “read up”) and a user with high-level clearance can write for lower levels of clearance (called “write down”). The Biba model is typically utilized in businesses where employees at lower levels can read higher-level information and executives can write to inform the lower-level employees.

Bell-LaPadula, on the other hand, is a setup where a user at a higher level (e.g., Top Secret) can only write at that level and no lower (called “write up”), but can also read at lower levels (called “read down”). Bell-LaPadula was developed for governmental and/or military purposes where if one does not have the correct clearance level and does not need to know certain information, they have no business with the information. At one time, MAC was associated with a numbering system that would assign a level number to files and level numbers to employees. This system made it so that if a file (i.e. myfile.ppt) had is level 400, another file (i.e. yourfile.docx) is level 600 and the employee had a level of 500, the employee would not be able to access “yourfile.docx” due to the higher level (600) associated with the file. MAC is the highest access control there is and is utilized in military and/or government settings utilizing the classifications of Classified, Secret and Unclassified in place of the numbering system previously mentioned.


## 2. Discretionary Access Control (DAC)

The Discretionary Access Control (DAC) model is the least restrictive model compared to the most restrictive MAC model. DAC allows an individual complete control over any objects they own along with the programs associated with those objects. This gives DAC two major weaknesses. First, it gives the end-user complete control to set security level settings for other users which could result in users having higher privileges than they’re supposed to. Secondly, and worse, the permissions that the end-user has are inherited into other programs they execute.

This means the end-user can execute malware without knowing it and the malware could take advantage of the potentially high-level privileges the end-user possesses.

## 3. Role-Based Access Control (RBAC)

The Role-Based Access Control (RBAC) model provides access control based on the position an individual fills in an organization. So, instead of assigning John permissions as a security manager, the position of security manager already has permissions assigned to it. In essence, John would just need access to the security manager profile. RBAC makes life easier for the system administrator of the organization.

The big issue with this access control model is that if John requires access to other files, there has to be another way to do it since the roles are only associated with the position; otherwise, security managers from other organizations could get access to files they are unauthorized for.

## 4. Rule-Based Access Control

The Rule-Based Access Control, also with the acronym RBAC or RB-RBAC. Rule-Based Access Control will dynamically assign roles to users based on criteria defined by the custodian or system administrator. For example, if someone is only allowed access to files during certain hours of the day, Rule-Based Access Control would be the tool of choice.

The additional “rules” of Rule-Based Access Control requiring implementation may need to be “programmed” into the network by the custodian or system administrator in the form of code versus “checking the box.”

## 5. Attribute-Based Access Control (ABAC)

The Attribute-Based Access Control (ABAC) model is often described as a more granular form of Role-Based Access Control since there are multiple that are required in order to gain access. These attributes are associated with the subject, the object, the action and the environment. For example, a sales rep (subject) may try to access a client’s record (object) in order to update the information (action) from his office during work hours (environment).

This approach allows more fine-tuning of access controls compared to a role-based approach. For example, we could deny access based on the environment (e.g., time of day) or action (e.g., deleting records). The downside is that can be more difficult to get these controls up and running.

## 6. Risk-Based Access Control

Risk-Based Access Control is a dynamic access control model that determines access based on the level of evaluated risk involved in the transaction. One commonly-used example is identifying the risk profile of the user logging in. If the device being logged in from is not recognized, that could elevate the risk to prompt additional authentication. If an action deemed high-risk occurs, such as attempting to update banking information, that could trigger more risk-based prompts.

One recent study found risk-based controls to be less annoying to users than some other forms of authentication. For example, two-factor authentication was “significantly more cumbersome to use and significantly more unnecessarily complex compared to [the tested risk-based authentication] conditions.”