# Authenticate Using SAML - OneLogin

**Note:** *This guide was written before the renaming. Just replace `HackMD` with `CodiMD` in your mind :smile: thanks!*

1. Sign-in or sign-up for an OneLogin account.

2. Go to the administration page.

3. Select the **APPS** menu and click on the **Add Apps**.

    ![onelogin-add-app](https://i.imgur.com/XsjhvHg.png)

4. Find "SAML Test Connector (SP)" for template of settings and select it.

    ![onelogin-select-template](https://i.imgur.com/uDFAgqy.png)

5. Edit display name and icons for OneLogin dashboard as you want, and click **SAVE**.

    ![onelogin-edit-app-name](https://i.imgur.com/5qvf9vY.png)

6. Other tabs will appear, click **Configuration** and fill the registration form:
    * RelayState: The base URL of your CodiMD, which is "the issuer". (Remove trailing slash.)
    * ACS (Consumer) URL Validator: The callback URL of your CodiMD. (serverurl + /auth/saml/callback)
    * ACS (Consumer) URL: Same as above
    * Login URL: Login URL(SAML requester) of your CodiMD. (serverurl + /auth/saml)

    ![onelogin-edit-sp-metadata](https://i.imgur.com/8HdYcYt.png)

7. Once you finished registration, click **SSO** and copy or download these two items:
    * **X.509 Certificate**: Click **View Details** and **DOWNLOAD** or copy the content of certificate.
    * **SAML 2.0 Endpoint (HTTP)**: Copy the URL.

    ![onelogin-copy-idp-metadata](https://i.imgur.com/9aL9PI7.png)


8. In your CodiMD server, create IdP certificate file from the **X.509 Certificate**.

9. Add the **IdP URL** and the **Idp certificate file path** to your config.json file or pass them as environment variables.

    * `config.json`:
      ```javascript
      {
        "production": {
          "saml": {
            "idpSsoUrl": "https://*******.onelogin.com/trust/saml2/http-post/sso/******",
            "idpCert": "/path/to/idp_cert.pem"
          }
        }
      }
      ```

    * environment variables
      ```sh
      CMD_SAML_IDPSSOURL=https://*******.onelogin.com/trust/saml2/http-post/sso/******
      CMD_SAML_IDPCERT=/path/to/idp_cert.pem
      ```

10. Try sign-in with SAML from your CodiMD sign-in button or OneLogin dashboard (like the screenshot below).  

    ![onelogin-use-dashboard](https://i.imgur.com/KSWsLJf.png)

---
###### tags: `CodiMD` `Docs`