# SF-Discord-Auth-Provider
This repo contains the code and configuration needed to make Discord a valid auth provider for Salesforce Experience Cloud

Components needed:

Package to be installed: https://login.salesforce.com/packaging/installPackage.apexp?p0=04t5Y000001TqZq

This package includes six apex classes (1 Reg Handler, 2 Invocable classes, with coverage), a custom field on Account, and a Screen flow.  This package is unmanaged, so feel free to make any changes to the reg hanlder that are needed.  The Invocable classes are used in the flow, to overcome the fact that an External ID user does not have edit access to its own Contact/Account Record.

The Reg Handler class is also not currently built for Person Accounts or Contactless users.  More optimizations to come.


Set up a Discord app: https://fusionauth.io/docs/v1/tech/identity-providers/openid-connect/discord/

Follow the instructions above to create a discord developer app, and retrieve the Client ID (for the Consumer Key in SF) and Client Secret (for the Consumer Secret in SF).  Also ensure that the identity and email scopes are selected

Set up Open ID Connect Auth Provider:

![image](https://user-images.githubusercontent.com/25828594/135436562-39d66929-9568-44ac-9180-89e22bd7bb64.png)

Navigate to your experience Site page (Digital Experiences > All Sites > (your site) Workspaces > Administration > Login & Registration), and select your Discord Auth provider you just set up in the "Select login options to display on the login page."


Navigate back to the Discord App, and allow all of the following redirects:

Communitybaseurl (ex: https://integrationtest-developer-edition.na156.force.com/)
Communitybaseurl/login (ex: https://integrationtest-developer-edition.na156.force.com/login)
(the rest will be from the "Experience Cloud Sites" section of your Auth Provider)
Communitybaseurl/services/auth/oauth/Discord (ex: https://integrationtest-developer-edition.na156.force.com/services/auth/oauth/Discord)
Communitybaseurl/services/auth/link/Discord (ex: https://integrationtest-developer-edition.na156.force.com/services/auth/link/Discord)
Communitybaseurl/services/auth/sso/Discord (ex: https://integrationtest-developer-edition.na156.force.com/services/auth/sso/Discord)
Communitybaseurl/services/auth/test/Discord (ex: https://integrationtest-developer-edition.na156.force.com/services/auth/test/Discord)
Communitybaseurl/services/authcallback/Discord (ex: https://integrationtest-developer-edition.na156.force.com/services/authcallback/Discord)


NOTE: free Discord developer apps can only handle so many redirect urls, so for this example, even though the Reg handler has handling for a regular user (for use on the mydomain login page), this solution can only handle Experience cloud logins.  If used as an auth provider for regular logins, the redirects above will need to come from the "Salesforce Configuration" section of the Auth Provider instead.


Configure a Login Flow that utilizes the Flow from the installed package, and uses the profile/license specified in the Registration Handler.  By default, this will be External Identity, but this can be changed in the Registration Handler code after installation:

![image](https://user-images.githubusercontent.com/25828594/135434877-0b1ca500-5427-4bf0-b4e9-5f3f2f76e639.png)

