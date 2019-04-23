# Authenticate Using SAML

The basic procedure is the same as the case of OneLogin which is mentioned in [Authenticate with SAML - OneLogin](/s/SyNC4-j9N). If you want to use other IdP, see detailed configuration as the following:

* If your identity provider (IdP) accepts metadata XML from the service provider (SP, CodiMD), you can use the below url to convey the metadata XML, otherwise you can upload your metadata XML to your IdP:
    * `{{your-serverurl}}/auth/saml/metadata`

* Change the value of `issuer`, `identifierFormat` to match your IdP.
  * `issuer`: A unique id to identify the application to the IdP, which is the base URL of your CodiMD.

  * `identifierFormat`: A format of unique id to identify the user of IdP, which is the format based on email address as default. It is recommend that you format the `identifierFormat` as below:
    * urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress (default)
    * urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified

  * You can change the values in `config.json`:
    ```javascript
    {
      "production": {
        "saml": {
          /* omitted */
          "issuer": "mycodimd"
          "identifierFormat": "urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified"
        }
      }
    }
    ```
  * Or, specify environment variables:
    ```
    CMD_SAML_ISSUER=mycodimd
    CMD_SAML_IDENTIFIERFORMAT=urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified
    ```

* Change the mapping of attribute names to customize the displaying user name and email address to match that of your IdP.
  * `attribute`: A dictionary to map attribute names
  * `attribute.id`: A primary key of user table for your CodiMD
  * `attribute.username`: Attribute name of displaying user name on CodiMD
  * `attribute.email`: Attribute name of email address, which will be also used for Gravatar
    * **Note:** Default value of all attributes is NameID of SAML response, which is the email address, if you set the `identifierFormat` as recommended.

  * Again, you can change the values in `config.json`:
    ```javascript
    {
      "production": {
        "saml": {
          /* omitted */
          "attribute": {
            "id": "sAMAccountName",
            "username": "displayName",
            "email": "mail"
          }
        }
      }
    }
    ```

  * Or, specify environment variables
    ```sh
    CMD_SAML_ATTRIBUTE_ID=sAMAccountName
    CMD_SAML_ATTRIBUTE_USERNAME=nickName
    CMD_SAML_ATTRIBUTE_EMAIL=mail
    ```

* If you want to control permission by group membership, add group attribute name and required group (allowed) or external group (not allowed).
  * `groupAttribute`: An attribute name of group membership
  * `requiredGroups`: Group names array for allowed access to CodiMD
  * `externalGroups`: Group names array for not allowed access to CodiMD

    **Note:** Evaluates `externalGroups` first.

  * Change these in `config.json`:
    ```javascript
    {
      "production": {
        "saml": {
          /* omitted */
          "groupAttribute": "memberOf",
          "requiredGroups": [ "codimd-users", "board-members" ],
          "externalGroups": [ "temporary-staff" ]
        }
      }
    }
    ```
  * Or use environment variables:
    **Note:** Use vertical bar to separate for environment variables.
    ```sh
    CMD_SAML_GROUPATTRIBUTE=memberOf
    CMD_SAML_REQUIREDGROUPS=codimd-users|board-members
    CMD_SAML_EXTERNALGROUPS=temporary-staff
    ```
---
###### tags: `CodiMD` `Docs`