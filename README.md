
LAB WORKBENCH: EXTERNAL COLLABORATION (B2B) INVITATION LOCKDOWN
================================================================================

1. PRE-DEPLOYMENT TARGET IDENTIFICATION
--------------------------------------------------------------------------------
To validate this outbound boundary restriction accurately, we will utilize two distinct 
test administrator and user accounts inside Microsoft Entra ID (MEID):

* Alpha Engineer (Source): Assigned the User Administrator role to verify successful 
  external Business-to-Business (B2B) guest invitation processing.
* Bravo Engineer (Source): Left as a standard user account with no directory roles 
  to verify the hard block enforcement of the invitation restriction.

2. COMPONENT DEPLOYMENT & PORTAL PATHS
--------------------------------------------------------------------------------
Configure the tenant-wide external collaboration boundaries using the following 
precise sequence within the modern Microsoft Entra ID (MEID) admin center interface:

Step 1: Navigate to External Collaboration Settings
  - Open the Microsoft Entra ID (MEID) admin center interface.
  - Expand the main left-hand sidebar menu under the Microsoft Entra ID (MEID) header.
  - Scroll down the single flat navigation list and click directly on "External identities".
  - Inside the "External identities" sub-menu column, click on "External collaboration settings".

Step 2: Modify Guest Invitation Restrictions
  - On the "External collaboration settings" blade, locate the section header labeled 
    "Guest invitation restrictions".
  - Observe the default tenant state: "Anyone in the organization can invite guest users 
    including guests and non-admins (most inclusive)".
  - Change the radio button selection to the highly restricted option: 
    "Only users assigned to specific admin roles can invite guest users".
  - Click the "Save" action control icon at the top menu bar to commit the schema 
    modification tenant-wide.

3. EXPECTED OPERATIONAL HANDSHAKE & INVERSE LOGIC
--------------------------------------------------------------------------------
The Microsoft Entra ID (MEID) invitation engine applies a strict role-based access 
control check at the point of invitation submission:

* The Failure State (Bravo Invitation Denied): Bravo Engineer (Source) navigates 
  to Microsoft Entra ID (MEID) > Users > All users > New user > Invite external user. Bravo enters an 
  external target email address and submits the form. The engine evaluates Bravo's 
  directory identity context, flags the missing administrative role authorization token, 
  interprets this as a hard boolean negative state, and instantly drops an explicit 
  "Access Denied: You do not have permissions to invite guest users" notification block.

* The Success State (Alpha Invitation Approved): Alpha Engineer (Source) navigates 
  to Microsoft Entra ID (MEID) > Users > All users > New user > Invite external user. Alpha enters an 
  external target email address and submits the form. The engine evaluates Alpha's 
  directory identity context, confirms the presence of the active User Administrator 
  role token, interprets this as a hard boolean positive state, and cleanly processes 
  the external invitation payload to issue a directory guest object.

4. LIVE VERIFICATION EXECUTION PLAN
--------------------------------------------------------------------------------
Execute these validation checks immediately following a 2-minute policy propagation window:

1. Open a fresh private browser window.
2. Navigate directly to the management interface portal: https://entra.microsoft.com
3. Authenticate using Bravo Engineer's standard user credentials.
4. Go to Microsoft Entra ID (MEID) > Users > All users.
5. Attempt to click "New user" and choose "Invite external user". Verify if the button 
   is grayed out, or if submitting an email address returns an immediate error block.
6. Close the browser session, open a fresh private window, and return to the portal.
7. Authenticate using Alpha Engineer's identity administrator credentials.
8. Go to Microsoft Entra ID (MEID) > Users > All users > New user > Invite external user.
9. Enter a valid external test email destination and click Invite.
10. Verify that the system completes the write action and creates the guest user object.

5. SYSTEM VALIDATOR: CONSTRAINTS & EDGE CASES
--------------------------------------------------------------------------------
* Access Denied Triggers: A hard operational block triggers instantly if a non-administrative 
  user profile attempts an invitation via the portal, Command-Line Interface (CLI), 
  Microsoft Graph PowerShell, or direct Microsoft Graph Application Programming Interface (API) 
  endpoints. Conversely, a clean invitation success state executes if and only if the calling 
  security principal possesses an authorized directory role asset.
* Negative Constraints: This tenant-wide security setting restricts who can *initiate* an 
  outbound invitation; it cannot inspect, filter, or control the downstream data sharing 
  activities or access permissions assigned to guest objects once they are established in 
  the directory environment.
* The Inverse Logic: If the invitation restriction policy is switched back to the open 
  "Anyone can invite" state, the role-checking evaluation engine is completely bypassed, 
  meaning all internal accounts gain immediate permission to invite arbitrary external entities.
* Hard Booleans: The invitation processing engine evaluates permissions down to a hard 0 
  (Role Checked Missing = Invitation Action Abandoned) or 1 (Role Checked Approved = Invitation 
  Action Completed). No partial or pending invitation authorizations are supported.
================================================================================
