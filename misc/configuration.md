# CodiMD Configuration

You can configure CodiMD with environment variables or with config file `config.json`.

Environment variables are defined in [`./lib/config/environment.js`](https://github.com/hackmdio/codimd/blob/master/lib/config/environment.js) and overwrite `.config.json` , which can be found at [`config.json.example`](https://github.com/hackmdio/codimd/blob/master/config.json.example) and is defined in [`./lib/config/default.js`](https://github.com/hackmdio/codimd/blob/master/lib/config/default.js).

See detailed config precedence here: [`.lib/config/index.js#37`](https://github.com/hackmdio/codimd/blob/develop/lib/config/index.js#L37).

<!-- Configure with environment variables in [`docker-compose.yml`](), follow docker-compose syntax.
Configure with config file by modifying [`config.json.example`](https://github.com/hackmdio/codimd/blob/master/config.json.example), follow json syntax. -->

:::info
:bulb: **Hint:**
You would find most config options duplicated in both the config file and environment variables.

The rationale behind this architecture design is that we recommend:
- using environment variables when deploying with [Docker](/codimd-docker-deployment), and
- using the config file `config.json` when developing and [deploying manually](/codimd-manual-deployment).
:::

<style>
#doc.markdown-body, .ui-infobar, .container-thiner {
  max-width: 1080px;
}
.markdown-body table th {
  white-space: nowrap;
}
.markdown-body table td {
  white-space: pre-wrap;
}
</style>

## Development
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
|DEBUG|debug|*<boolean>* `true`. `false`|Enable debug mode, show debug level log.|
|CMD_LOG_LEVEL|loglevel| *<string>* `info` |Change the logging level.|
|CMD_SOURCE_URL|sourceURL| *<string>* `https://github.com/<path_to_your_commit>` |	CodiMD is licensed under AGPL, if you modified the code, please provide the link to your source code on the entry page with this variable.

### Development  - Environment Variable Specific Configs

| Environment Variable | Example Value | Description|
|-|-|-|
| NODE_ENV | *<string>* `production`, `development` | Set the deployment environment and corresponding settings in `config.json` will apply.|
| CMD_CONFIG_FILE | *<string>* `/path/to/config.json` | Change the path to the config file. |

---
## Networking
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_DB_URL | dbURL | *<string>* `mysql://user:pass@example.com:9821/db_name` | The URL on which the db is hosted. |
| CMD_DOMAIN | domain | *<string>* `codimd.dev` | The domain name your service will be hosted. |
| CMD_URL_PATH | urlPath | *<string>* `codimd` | The path in a URL your service will be hosted. (e.g., www.example.com/<urlpath>) |
| CMD_HOST | host | *<string>* `localhost` | The host to which CodiMD will listen. |
| CMD_PORT | port | *<string>* `3000` | The port on which CodiMD will be accessed. |
| CMD_PATH | path | *<string>* `/var/run/codimd.sock` | Path to the UNIX domain socket CodiMD will listen to. (If specified, `host` and `port` will be ignored.) |
| CMD_URL_ADDPORT | urlAddPort | *<boolean>* `true`. `false` | Set to assign port for callback URL. (You don't need this for ports 80 or 443. This only works when `domain` is set)
| CMD_ALLOW_ORIGIN | allowOrigin | *<List>* `localhost, example.com` | List of white listed domain names for CORS.  
| CMD_USECDN| useCDN | *<boolean>* `true`. `false` | Set to use CDN resources.
:::info
:bulb: **Hint:** `config.json` offers more customized database configs. See this [database connection guide](/s/codimd-db-connection).  
:::

---
## Security
### SSL
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_PROTOCOL_USESSL | protocolUseSSL | *<boolean>* `true`. `false` | Use SSL protocol for resources path (applied only when `domain` is set).

### SSL - Config.json Specific Configs
We recommend setting up a reverse proxy with web servers (e.g., Nginx or Apache) in order to use HTTPS. The reasons are:
1. SSL certificates have to be renewed periodically, we don't want to restart our CodiMD every time.
2. A SSL certificate is RARELY used only by the CodiMD service, so it doesn't make sense to bundle the certificate within the container.
3. A dedicated web server is more customizable and has better performance. 

The following configs provide basic SSL supports within CodiMD for those who know what they are doing. These configs are only available in the developer-facing `config.json` file.

| config.json | Example Value | Description|
|-|-|-|
| useSSL | *<boolean>*  `true`. `false` | Use SSL server (enabling this will also turn on `protocolUseSSL`). |
| dhParamPath | *<string>* `./cert/dhparam.pem` | Path to SSL dhparam. |
| sslKeyPath | *<string>* `./cert/client.key` | Path to SSL key. |
|sslCertPath | *<string>* `./cert/codimd_io.crt` | Path to SSL cert. |
|sslCAPath | *<List>* `./cert/COMODORSAAddTrustCA.crt` | A list of path(s) to the chain of SSL CA. The order of the list items has to follow the order of the chain.|

### HSTS
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_HSTS_ENABLE | enable | *<boolean>* `true`. `false` | Enable HSTS (only if HTTPS is also enabled). |
|CMD_HSTS_INCLUDE_SUBDOMAINS | includeSubdomains | *<boolean>* `true`. `false` | Include subdomains in HSTS. | 
| CMD_HSTS_MAX_AGE | maxAgeSeconds | *<integer>* `31536000` | Max time in seconds to tell clients to keep the HSTS status. |
| CMD_HSTS_PRELOAD | preload | *<boolean>* `true`. `false` | Allow preloading of the site's HSTS status. |

### CSP
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_CSP_ENABLE | enable | *<boolean>* `true`. `false` | Enable Content Security Policy. |
| CMD_CSP_REPORTURI | reportURI | *<string>* `https://<someid>.report-uri.com/r/d/csp/enforce` | Add a URL for CSP reports in case of violations. |

### CSP - Config.json Specific Configs
The following configs provide advanced CSP settings for those who know what they are doing. These configs are only available in the developer-facing `config.json` file.
| config.json | Example Value | Description|
|-|-|-|
| directives | *<json>* `{"scriptSrc": "trustworthy-scripts.example.com"}` | Specify valid source for javascript. |
| addDefaults | *<boolean>* `true`. `false` | Fallback for directives. |
| addDisqus | *<boolean>* `true`. `false` | Allow Disqus to be fetched. |
| addGoogleAnalytics | *<boolean>* `true`. `false` | Allow Google Analytics to be fetched. |
| upgradeInsecureRequests | *<string>* `auto` | Auto upgrade insecure requests. |
---
## Performance
These config values are battle tested and true. If you want to play with them, find them in the `config.json` file.

| config.json | Example Value | Description|
|-|-|-|
| staticCacheTime | *<integer>* `1 * 24 * 60 * 60 * 1000` | static file cache time in milliseconds. |
| heartbeatInterval | *<integer>* `5000` | Socket.io heartbeat interval in milliseconds. |
| heartbeatTimeout | *<integer>* `10000` | Socket.io heartbeat timeout in milliseconds. |
| documentMaxLength | *<integer>* `100000` | Note length maximum in characters. |

---
## Resources		
These housekeeping configs pertains to the project structure. Find them only in the `config.json` file.

| config.json | Example Value | Description|
|-|-|-|
| defaultNotePath | *<string>* `./public/default.md` | Path to the file that contains content that will be added to all newly created notes. |
|tmpPath | *<string>* `./tmp/` | Path to temp directory. |
| docsPath | *<string>* `./public/docs` | Path to all pre-loaded documents. Read [this guide to customize pre-load notes](/s/codimd-preload-notes) |
| viewPath | *<string>* `./public/views` | Path to pages view templates. |
| uploadsPath | *<string>* `./public/uploads` | Path to where user-uploaded images are stored when you [set `imageUploadType` to filesystem](#Image-Storage). |

---
## Privilege and Privacy
### User Privilege
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_ALLOW_ANONYMOUS | allowAnonymous | *<boolean>* `true`. `false` |	Allow anonymous usage. |
| CMD_ALLOW_ANONYMOUS_EDITS | allowAnonymousEdits | *<boolean>* `true`. `false` | Allow anonymous guests to edit existing notes that has *freely* permission (applied only when `allowAnonymous` is True). |
| CMD_DEFAULT_PERMISSION | defaultPermission | *<string>* `freely`, `editable`, `limited`, `locked`, `protected` or `private` | Set default permission for new notes. See permission meanings [here](https://hackmd.io/features#Permissions). |

### Session
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_SESSION_LIFE | sessionLife | *<integer>* `14 * 24 * 60 * 60 * 1000` | Session life time in milliseconds. |
| CMD_SESSION_SECRET | sessionSecret | *<string>* `1337h4x0r` | Secret used to sign the session cookie. If not set, one will be randomly generated on startup. |

### User Features
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_ALLOW_GRAVATAR | allowGravatar | *<boolean>* `true`. `false` | Use gravatar as a default profile picture source. |
| CMD_ALLOW_FREEURL | allowFreeURL | *<boolean>* `true`. `false` | Allow new note creation by accessing a nonexistent note URL. |
| CMD_FORBIDDEN_NODE_IDS | forbiddenNoteIDs | *<List>* `robots.txt` | When `allowFreeUrl` is enabled, string items in this list will not be auto-created. | 
| CMD_ALLOW_PDF_EXPORT | allowPDFExport | *<boolean>* `true`. `false` | Allow export to PDF. |
| CMD_DROPBOX_APPKEY | appKey | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | The key for import notes from, or export notes to, Dropbox. | 
| CMD_DEFAULT_USE_HARD_BREAK | defaultUseHardbreak | *<boolean>* `true`. `false` | Configurable default break style. Default is `true` | 
| CMD_LINKIFY_HEADER_STYLE | linkifyHeaderStyle | *<string>* `keep-case`, `lower-case`, or `gfm` | Configure the generated link style of heading. |

#### Configuration for header link style

CodiMD supports three different styles of header link. Take **3.1. Good Morning my Friend! - Do you have 5$?** this header for example, CodiMD would generate link id as follows:

| type         | result                                      | description                                                   |
| ------------ | ------------------------------------------- | ------------------------------------------------------------- |
| `keep-case`  | `31-Good-Morning-my-Friend---Do-you-have-5` | Default                                                       |
| `lower-case` | `31-good-morning-my-friend---do-you-have-5` |                                                               |
| `gfm`        | `31-good-morning-my-friend---do-you-have-5` | It works like 'lower-case', but making sure the ID is unique. 1st appearance:<br>`31-good-morning-my-friend---do-you-have-5` <br>2nd appearance:<br>`31-good-morning-my-friend---do-you-have-5-1` <br> 3rd appearance: <br>`31-good-morning-my-friend---do-you-have-5-2`  | 

#### PlantUML

CodiMD supports PlantUML out of box, however you can specify your own PlantUML instance:

| Environment Variable | config.json       | Example Value                                  | Description                  |
| -------------------- | ----------------- | ---------------------------------------------- | ---------------------------- |
| CMD_PLANTUML_SERVER  | `plantuml.server` | *<string>* `https://www.plantuml.com/plantuml` | The URL of PlantUML instance |

---

## Image Storage
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_IMAGE_UPLOAD_TYPE | imageUploadType | *<string>* `imgur`, `s3`, `minio`, `azure` or `filesystem` | Where to store user-uploaded images. Default is `filesystem`. The storage is assigned by the config: uploadsPath under [Resources](#Resources). |

### Imgur
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_IMGUR_CLIENTID | clientID | *<string>* `CLIENT_ID` | Imgur API client id.


### S3

See this guide: [store image on S3](/s/codimd-image-storage-S3).

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_S3_ACCESS_KEY_ID | accessKeyId | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | AWS access key id. |
| CMD_S3_SECRET_ACCESS_KEY | secretAccessKey | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | AWS secret key. |
| CMD_S3_REGION | region | *<string>* `YOUR_S3_REGION` | AWS S3 region. |
| CMD_S3_BUCKET | s3bucket | *<string>* `YOUR_S3_BUCKET_NAME` | Bucket name when `imageUploadType` is set to s3 or minio. |
| CMD_S3_ENDPOINT | endpoint | *<string>* `s3.example.com` | Custom AWS S3 endpoint. |


### Minio

See this guide: [store image on Minio](/s/codimd-image-storage-minio).

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_MINIO_ACCESS_KEY | accessKey | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Minio access key. |
| CMD_MINIO_SECRET_KEY | secretKey | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Minio secret key. | 
| CMD_MINIO_ENDPOINT | endpoint | *<string>* `YOUR_MINIO_HOST` | Minio host (endpoint). |
| CMD_MINIO_PORT | port | *<string>* `9000` | Minio port. |
| CMD_MINIO_SECURE | secure | *<boolean>* `true`. `false` | Minio secure. |

### Azure

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_AZURE_CONNECTION_STRING | connectionString | *<string>* `CONNECTION_STRING` | Azure Blob Storage connection string |
| CMD_AZURE_CONTAINER | container | *<string>* `CONTAINER_NAME` | Azure Blob Storage container name (automatically created if nonexistent).

---
## Authentication
### Email
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_EMAIL | email | *<boolean>* `true`. `false` | Allow email sign-in. |
| CMD_ALLOW_EMAIL_REGISTER | allowEmailRegister | *<boolean>* `true`. `false` | Allow email registration (only applied when `email` is set. |
:::info
:bulb: **Hint:** You can always use [bin/manage_users:createUser](https://github.com/hackmdio/codimd/blob/master/bin/manage_users) to create users if registration is disabled.
:::

### SAML

See this guide: [Authenticate with SAML](/s/codimd-auth-saml) and this example: [SAML - OneLogin as example](/s/codimd-auth-saml-onelogin)

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
|CMD_SAML_IDPSSOURL | idpSsoUrl | *<string>* `https://idp.example.com/sso` | Authentication endpoint of the IdP. |
| CMD_SAML_IDPCERT | idpCert | *<string>* `/path/to/cert.pem` | Certificate file path of IdP in PEM format. |
| CMD_SAML_ISSUER | issuer | *<string>* `codimd.mydomain.net` | Identity of CodiMD, the service provider (optional, default: serverurl). |
| CMD_SAML_IDENTIFIERFORMAT | identifierFormat | *<string>* urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress | Name identifier format (the example format is the default). |
| CMD_SAML_GROUPATTRIBUTE | groupAttribute | *<string>* `memberOf` | Attribute name for group list (optional). |
| CMD_SAML_REQUIREDGROUPS | requiredGroups | *<list>* `Hackmd-users, admins` | White list groups, users in these groups are allowed access. |
CMD_SAML_EXTERNALGROUPS	| externalGroups | *<list>* `Temporary-staff` | Black list groups, users in these groups are denied access. |
| CMD_SAML_ATTRIBUTE_ID | id | *<string>* `sAMAccountName` | Attribute used for id (default: NameID of SAML response). |
| CMD_SAML_ATTRIBUTE_USERNAME | username | *<string>* `mailNickname` | Attribute used for username (default: NameID of SAML response). |
CMD_SAML_ATTRIBUTE_EMAIL | email | *<string>* `mail` | Attribute used for email (default: NameID of SAML response if `identifierFormat` is in the default format.) |

### LDAP

See this guide: [Authenticate with LDAP](/s/codimd-auth-active-directory).

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_LDAP_URL | url | *<string>* `ldap://example.com` | URL of LDAP server. |
| CMD_LDAP_BINDDN | bindDn | *<string>* `CN=user1,CN=Users,DC=example,DC=com` | bindDn for LDAP access. |
| CMD_LDAP_BINDCREDENTIALS | bindCredentials | *<string>* `M7p@s3wOrd` | bindCredentials for LDAP access. |
| CMD_LDAP_SEARCHBASE | searchBase | *<string>* `o=users,dc=example,dc=com` | LDAP directory to begin search from. |
| CMD_LDAP_SEARCHFILTER | searchFilter | *<string>* `(uid={{username}})` | LDAP filter to search with. |
| CMD_LDAP_SEARCHATTRIBUTES | searchAttributes | *<list>* `displayName, mail` | LDAP attributes to search with (use comma to separate). |
| CMD_LDAP_USERIDFIELD | useridField | *<string>* `uidNumber`, `uid`, or `sAMAccountName` | The LDAP field which is used uniquely identify a user on CodiMD. |
| CMD_LDAP_USERNAMEFIELD | usernameField | *<string>* `username` | The LDAP field which is used as the username on CodiMD (fallback to userid). |
| CMD_LDAP_TLS_CA | tlsca | *<list>* `server-cert.pem, root.pem` | Root CA for LDAP TLS in PEM format (use comma to separate). |
| CMD_LDAP_PROVIDERNAME | providerName | *<string>* `My institution` | Optional name to be displayed at login form indicating the LDAP provider. |

### OAuth - Common OAuth Providers

See this table for common OAuth [callback URLs](/s/codimd-oauth-callback-urls).
You can also implement your own OAuth2 provider with [these configs](#OAuth---General-OAuth2).
#### Google
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_GOOGLE_CLIENTID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Google API client id. |
| CMD_GOOGLE_CLIENTSECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Google API client secret. |

#### GitHub

See [this guide](/s/codimd-oauth-github).

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_GITHUB_CLIENTID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | GitHub API client id. |
CMD_GITHUB_CLIENTSECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | GitHub API client secret. |
| CMD_GITHUB_ENTERPRISE_URL | enterpriseURL | *<string>* `http://url-of-github-enterprise` | Url of GitHub Enterprise instance. |
    
#### GitLab
See [this guide](/s/codimd-oauth-gitlab).

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_GITLAB_BASEURL | baseURL | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | GitLab authentication endpoint. (Default 'GitLab.com') |
| CMD_GITLAB_CLIENTID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | GitLab API client id. |
| CMD_GITLAB_CLIENTSECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | GitLab API client secret. |
| CMD_GITLAB_SCOPE | scope | *<string>* `read_user`, `api` | GitLab API requested scope.  For authentication only, use 'read_user'. If you need gitlab snippet import/export, use 'api'. (Default 'api'). |
| CMD_GITLAB_VERSION | version | *<string>* `v3`, `v4` | GitLab API version ('v4' for gitlab version > 11, 'v3' otherwise. Default 'v4'), |

#### Mattermost

See [this guide](/s/codimd-oauth-mattermost).

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_MATTERMOST_BASEURL | baseURL | *<string>* `example.com` | Mattermost authentication endpoint for versions < 5.0. For version 5.0 and above, see guide. |
| CMD_MATTERMOST_CLIENTID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Mattermost API client id. |
| CMD_MATTERMOST_CLIENTSECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Mattermost API client secret. |

#### Dropbox
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_DROPBOX_CLIENTID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Dropbox API client id. |
| CMD_DROPBOX_CLIENTSECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw`  | Dropbox API client secret. |

#### Facebook
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
CMD_FACEBOOK_CLIENTID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Facebook API client id.
CMD_FACEBOOK_CLIENTSECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Facebook API client secret. |

#### Twitter
| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_TWITTER_CONSUMERKEY | consumeKey | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Twitter API consumer key. |
|CMD_TWITTER_CONSUMERSECRET | consumerSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Twitter API consumer secret. |

### OAuth - General OAuth2

| Environment Variable | config.json | Example Value | Description|
|-|-|-|-|
| CMD_OAUTH2_BASEURL | baseURL |*<string>* `https://example.com` | 
| CMD_OAUTH2_USER_PROFILE_URL | userProfileURL | *<string>* `https://example.com` | Where to get user information after succesful login. The output has to be in JSON format. |
| CMD_OAUTH2_USER_PROFILE_USERNAME_ATTR | userProfileUsernameAttr | *<string>* `name` |  Username in the JSON returned from the user profile URL. |
| CMD_OAUTH2_USER_PROFILE_DISPLAY_NAME_ATTR | userProfileDisplayNameAttr | *<string>* `display-name` | Display-name in the JSON returned from the user profile URL. |
| CMD_OAUTH2_USER_PROFILE_EMAIL_ATTR | userProfileEmailAttr | *<string>* `email` | Email address in the JSON returned from the user profile URL. |
| CMD_OAUTH2_TOKEN_URL | tokenURL | *<string>* `https://example.com` | A.K.A. token endpoint from the provider. |
| CMD_OAUTH2_AUTHORIZATION_URL | authorizationURL | *<string>* `https://example.com` | Authorization URL of the provider. |
| CMD_OAUTH2_CLIENT_ID | clientID | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Get this from the provider when registering CodiMD as OAuth2-client. |
| CMD_OAUTH2_CLIENT_SECRET | clientSecret | *<string>* `HWiDIfHvTvKj3_zMwA6aSw` | Get this from the provider when registering CodiMD as OAuth2-client. |
| CMD_OAUTH2_PROVIDERNAME | providerName | *<string>* `My Company` | Name displayed at login form indicating the OAuth provider. |
###### tags: `CodiMD` `Docs`
