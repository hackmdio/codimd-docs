# Authenticate Using Active Directory

To setup your CodiMD instance with Active Directory you need to set the following variables:

```
CMD_LDAP_URL=ldap://internal.example.com
CMD_LDAP_BINDDN=cn=binduser,cn=Users,dc=internal,dc=example,dc=com
CMD_LDAP_BINDCREDENTIALS=<super secret password>
CMD_LDAP_SEARCHBASE=dc=internal,dc=example,dc=com
CMD_LDAP_SEARCHFILTER=(&(objectcategory=person)(objectclass=user)(|(sAMAccountName={{username}})(mail={{username}})))
CMD_LDAP_USERIDFIELD=sAMAccountName
CMD_LDAP_PROVIDERNAME=Example Inc AD
CMD_LDAP_TLS_CA=/etc/ssl/certs/ca-certificates.crt,/etc/ssl/certs/myca.crt
```

## Notes
- `CMD_LDAP_BINDDN` is either the `distinguishedName` or the `userPrincipalName`.

- You would see the error: **"username/password is invalid"** if `CMD_LDAP_BINDDN` or `CMD_LDAP_BINDCREDENTIALS` is incorrect


- `CMD_LDAP_SEARCHFILTER` will search through all users with either the email address or the `sAMAccountName` (usually the login name used to login to Windows).

- `sAMAccountName` should be in this format: `(&(objectcategory=person)(objectclass=user)(sAMAccountName={{username}}))`

- `CMD_LDAP_USERIDFIELD` means: we want to use `sAMAccountName` as the unique identifier for the account itself.

- `CMD_LDAP_PROVIDERNAME` is just the name on the login page above the username and password field.

- `CMD_LDAP_TLS_CA` is the CA location, seperated by comma.

## json format:

```json
"ldap": {
    "url": "ldap://internal.example.com",
    "bindDn": "cn=binduser,cn=Users,dc=internal,dc=example,dc=com",
    "bindCredentials": "<super secret password>",
    "searchBase": "dc=internal,dc=example,dc=com",
    "searchFilter": "(&(objectcategory=person)(objectclass=user)(|(sAMAccountName={{username}})(mail={{username}})))",
    "useridField": "sAMAccountName",
},
```

## Referene:
More details and example: https://www.npmjs.com/package/passport-ldapauth

---
###### tags: `CodiMD` `Docs`
