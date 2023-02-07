It's not possible if password was hashed, but you can set passwordFormat to = clear, and then new members passwords will not be hashed and you will be able to get it.

```
<membership defaultProvider="UmbracoMembershipProvider" userIsOnlineTimeWindow="15">
      <providers>
        <clear />
        <add name="UmbracoMembershipProvider"
            type="umbraco.providers.members.UmbracoMembershipProvider"
            enablePasswordRetrieval="true"
            enablePasswordReset="false"
            requiresQuestionAndAnswer="false"
            defaultMemberTypeAlias="Another Type"
            passwordFormat="clear" />
      </providers>
    </membership>
    ```
