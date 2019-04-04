# Cadasta Portal + Auth0 SAML Provider

## Questions

1. How to make usernames in Esri Portal more understandable?

   - We have access to the following JSON data about a user from Auth0:

     ```json
     {
       "email": "alukach@cadasta.org",
       "email_verified": true,
       "name": "Anthony Lukach",
       "given_name": "Anthony",
       "family_name": "Lukach",
       "picture": "https://lh4.googleusercontent.com/-aVUQK8kaA8w/AAAAAAAAAAI/AAAAAAAAAAA/AGDgw-gzZtcbaChKG9lQI9T2LZ0xKAJ-UA/mo/photo.jpg",
       "locale": "en",
       "updated_at": "2018-12-12T04:06:37.077Z",
       "user_id": "google-oauth2|110664703776792306122",
       "nickname": "alukach",
       "identities": [
         {
           "provider": "google-oauth2",
           "user_id": "110664703776792306122",
           "connection": "google-oauth2",
           "isSocial": true
         }
       ],
       "created_at": "2018-12-06T18:31:38.860Z",
       "last_ip": "174.0.105.153",
       "last_login": "2018-12-12T04:06:37.077Z",
       "logins_count": 15,
       "blocked_for": [],
       "guardian_authenticators": []
     }
     ```

     We can inject this data as SAML attributes with the following rule:

     ```js
     function (user, context, callback) {
       context.samlConfiguration.mappings = {
         "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "email",
         "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "email",
         "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "given_name",
         // "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "name",
         // "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "family_name",

         // Ensure app_metadata
         "Groups": "app_metadata.groups"
       };

       callback(null, user, context);
     }
     ```

     More information about this technique [found here](https://auth0.com/rules/saml-configuration).

     It seems that this method only works for setting the `nameidentifier`, `emailaddress`, and `givenname` (first name). Despite `surname` being a standard claim in the schema used ([source](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/technical-reference/the-role-of-claims)), Esri does not seem to respect it. From [the Esri docs on configuring a SAML IdP](http://enterprise.arcgis.com/en/portal/latest/administer/linux/configuring-a-saml-compliant-identity-provider-with-your-portal.htm#ESRI_CHOICE_6AFAFAE20E704D498915543DC2B3B8BD):

     > **Update profiles on sign in**—Select this option to have Portal for ArcGIS update users' `givenName` and `email address` attributes if they have changed since their last login. This is selected by default.

     Perhaps, as _somewhat_ described, only `givenName` and `email_address` support being set.

1. How do we create users ahead of time that are assigned to a group? (e.g. Create 10 users, those users are already associated w/ groups on first login)

   - **Option 1**: Invite as normal. It's critical that the **username must match the username sent by Auth0, which will be the user's email address**.
   - **Option 2**: This can be achieved by setting the "Enable SAML based group membership" option of the Esri Portal's SAML provider to `true`. When enabled, Groups within the Portal have a new "Who can join this group?" option: "Members of an enterprise group". When this option is selected, a prompt appears for the "Enterprise Group Name". If a SAML-based user has this group assigned to it, that user will automatically be able to view that group under their "My Organization's Groups" tab of the Groups view. This places automatic group sharing within the domain of Auth0 rather than the Esri Portal. This seems to be a large flaw in this solution, in that it will require us to do one of the following:
     - Build external tooling to allow a Project Manager to assign users to groups (within Auth0). User creation can be performed by the [Auth0 Management API](https://auth0.com/docs/api/management/v2#Users_post_users). This comes with some challenges:
       - How do we ensure that the Project Manager can only see users that belong to their organization (i.e. if they are to assign a user to a group, they must first see the users from their organization). _Idea:_ Users should have a `tenant` property within Auth0, describing to which org(s) they have access. Perhaps designation of this user-creating permission should occur in Auth0 rather than Portal, to keep logic more consolidated within Auth0.
       - How do we handle situations where users don't exist when the group is set up? _Idea:_ A Project Manager must be able to create a user and then add that user to the desired group.
     - Have all user management and group assignment operations handled by the Cadasta Programs team. _Idea:_ This could be a stop-gap until user management tooling is developed and delivered to partners.
   - Notes about passing Group data from Auth0 to Esri Portal:

     - > To link enterprise groups from a SAML-based IDP to new groups created in your portal, check the Enable SAML based group membership box when setting your IDP in the portal's settings. To link an enterprise group, you must enter the name of the group exactly as it will be returned in the IDP's SAML assertion response when creating your new group in the ArcGIS Enterprise portal. The Search for Group option is not available when Enable SAML based group membership is selected.

       > The supported (case-insensitive) names for the attribute defining a user's group membership are: Group, Groups, Roles, MemberOf, member-of, http://schemas.xmlsoap.org/claims/Group, urn:oid:1.3.6.1.4.1.5923.1.5.1.1, and urn:oid:2.16.840.1.113719.1.1.4.1.25.

       > [source](https://enterprise.arcgis.com/en/portal/latest/administer/windows/create-groups.htm#ESRI_SECTION2_26F44DE88AD84BCCBFCA47C59FBB941F)


          - > [Y]ou can provide metadata to the portal about the enterprise groups in your identity store. This allows you to create groups in the portal that leverage the existing enterprise groups in your identity store. When members log in to the portal, access to content, items, and data is controlled by the membership rules defined in the enterprise group. If you do not provide the necessary enterprise group metadata, you can still create groups. However, membership rules will be controlled by your ArcGIS Enterprise portal, not your identity store. - [source](https://enterprise.arcgis.com/en/portal/latest/administer/linux/configuring-a-saml-compliant-identity-provider-with-your-portal.htm#GUID-ED700718-8CEE-4168-B3BF-739EB3D6367C)

            [Directions](https://enterprise.arcgis.com/en/portal/latest/administer/windows/create-groups.htm#ESRI_SECTION1_5E3FFFAA1B7E443FBB1E483E070B1979)

1. How do we switch off user sign-up?
   - This can be done by unchecking the "Allow users to create new built-in accounts" option under the "[Organization](https://maps.cadasta.org/arcgis/home/organization.html#) > Edit Settings > Security > Policies" configurations.
1. How can we access the Cadasta Portal via scripting tool if we can only authenticate via Auth0?

   - Machine to Machine
     - _Less Ideal_: Username / Password
       - Use a built-in user for any scripting needs, authenticate in the same manner as we do today
     - _More Ideal_: Use OAuth `client_id` and `client_secret` to get token
       - Utilize OAuth 2.0 to grant permissions to the Cadasta Portal ([details](https://developers.arcgis.com/documentation/core-concepts/security-and-authentication/#auth-oauth), [more](https://developers.arcgis.com/documentation/core-concepts/security-and-authentication/arcgis-server-and-portal-for-arcgis/#version-103-and-later))
       - Some supported way to authenticate a script with the Cadasta Portal via SAML? Not sure if this exists. Possibly related:
         - https://github.com/DOI-BLM/requests-arcgis-auth
         - https://github.com/Esri/ago-assistant/issues/115
   - User login
     - Need to build a demo to allow user to login via Auth0, get token from Portal, and then use portal

1. Can we allow a user to login either both Google or a provider user/pass?
   - Seems like it. **To be researched later** Potential solutions:
     - [Linking User Accounts
       ](https://auth0.com/docs/link-accounts)
     - [Consolidating Multiple Identity Sources with Auth0](https://auth0.com/blog/consolidating-multiple-identity-sources-with-auth0/)
1. Can we port existing users to the Auth0 IDP?

   - We can't No. The way in which users from Auth0 are associated with a user in Esri Portal is by their username. As such, if we wanted to move an existing user from Esri Portal login to Auth0, we would need to ensure that the username provided by Auth0 matches their username in Esri Portal. Through Auth0's Rules, we can alter the username sent to the Esri Portal from Auth0. Even if the username matches an existing built-in user, it seems that Esri Portal attempts to create a new user and throws an error due to the conflicting username. There is no way to modify a user from a built-in user to a SAML auth user (according to Travis Butcher via Slack).
   - But, we can create new users to make the experience seamless for the end user. To do this, we would write a script to:

     1. List all existing built-in users.
     1. Create new, enterprise SAML user (in Portal) for that user.
     1. Optional: We could go ahead and create Auth0 users at this point as well. This may not make sense though, as we would likely want to allow our users to control how they authenticate (Google, Twitter, etc) and we may be required to make that assumption when creating a user (not totally sure).
     1. Transfer all contents and groups from old user to new user. ([example](https://developers.arcgis.com/python/sample-notebooks/move-existing-user-content-to-a-new-user/))
     1. De-activate or delete old user.

     Once this script has run, we should turn off named-user sign-ins (unless they are needed for scripting solutions, in whihch case we should ensure that users understand that they should be logging in via the SAML login by renaming the button to something appropriate).

   - Some potential solutions that I earlier thought would help but don't:
     - [Bulk User Imports with the Management API](https://auth0.com/docs/users/migrations/bulk-import)
     - [Esri Docs: Add SAML-based enterprise accounts](http://enterprise.arcgis.com/en/portal/latest/administer/linux/add-members-to-your-portal.htm#ESRI_SECTION2_D274839C22254F74BD2847DB38CA97FD)
     - > When creating a user for a SAML-based account, the user name "must match the existing enterprise user and format defined in the SAML identity provider (for example, jcho11). If the user name does not match, the account will be created in the portal but cannot be used. Verify the user name is correct before proceeding."

## Steps Taken

1. Setup Auth0 to act as a SAML provider: [[link]](https://auth0.com/docs/protocols/saml/saml-idp-generic)
2. Setup Esri Portal to utilize Auth0's SAML provider: [[link]](https://enterprise.arcgis.com/en/portal/10.6/administer/linux/configuring-a-saml-compliant-identity-provider-with-your-portal.htm#GUID-0AFD5CF7-AFDF-4682-A604-A24B0C8875D6). At time of writing, the advanced settings are configured as:
   - Your users will be able to join: `After you add the accounts to the portal` (this stipulates that even if a user has an Auth0 user, they won't be able to access the portal unless a user is manually created for them on the portal)
   - Encrypt Assertion: `true`
   - Enable Signed Request: `true`
     - Sign using SHA256: `false`
   - Propagate logout to Identity Provider: `false` (this doesn't seem to work, causing logout to redirect to a bad endpoint, but would be good to figure out how to make work)
   - Update profiles on sign in: `true`
   - Enable SAML based group membership: `true`
3. Add the following "SAML Attributes Mapping" rule to Auth0:

   ```js
   function (user, context, callback) {
     context.samlConfiguration.mappings = {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "user_id",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "email",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "name",
        "Groups": "app_metadata.groups"
     };

     callback(null, user, context);
   }
   ```

4. Now, we can add the following `app_metadata` to the user to specify the groups that they are automatically added to:
   ```json
   {
     "groups": ["myGroup"]
   }
   ```

## SAML Background

- [An intro to SAML](https://auth0.com/blog/how-saml-authentication-works/)
  - Auth0 is operating as the "Identity Provider"
  - Esri Portal is operating as the "Service Provider"
