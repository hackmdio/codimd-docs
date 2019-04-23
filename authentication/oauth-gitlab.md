# Authenticate Using OAuth - GitLab

**Note:** *This guide was written before the renaming. Just replace `HackMD` with `CodiMD` in your mind :smile: thanks!*

1. Sign in to your GitLab

2. Navigate to the application management page at `https://your.gitlab.domain/admin/applications` (admin permissions required)

3. Click **New application** to create a new application and fill out the registration form:

![New GitLab application](https://i.imgur.com/2sTQQ8u.png)

4. Click **Submit**

5. In the list of applications select **HackMD**. Leave that site open to copy the application ID and secret in the next step.

![Application: HackMD](https://i.imgur.com/3kT0zUw.png)


6. In the `docker-compose.yml` add the following environment variables to `app:` `environment:`

```
- HMD_DOMAIN=your.codimd.domain
- HMD_URL_ADDPORT=443
- HMD_PROTOCOL_USESSL=true
- HMD_GITLAB_BASEURL=https://your.gitlab.domain
- HMD_GITLAB_CLIENTID=23462a34example99XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
- HMD_GITLAB_CLIENTSECRET=5532e9dexamplXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

7. Run `docker-compose up -d` to apply your settings.

8. Sign in to your CodiMD using your GitLab ID:

![Sign in via GitLab](https://i.imgur.com/amS2xy6.png)

---
###### tags: `CodiMD` `Docs`