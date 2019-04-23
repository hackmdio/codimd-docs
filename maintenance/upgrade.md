# Upgrading CodiMD

If you are upgrading CodiMD from an older version, follow these steps:

1. Check if you meet the [requirements](/s/B1vBRY85N#Requirements).
2. Check the version you were running and check out [Migrations and Version Changes](/s/SJ1zT9L9V) to see if additional steps, or configuration changes are needed.
3. Fully stop your old CodiMD server.
4. Get the latest [CodiMD](https://github.com/hackmdio/codimd) with `git pull` or unzip a new release.
5. Run `bin/setup` in the CodiMD directory. This will install all dependencies. It is safe to run on an existing installation.
6. Build front-end bundle by `npm run build` (use `npm run dev` if you are in development).
7. It is recommended to start your server manually once: `npm start --production`, this way it's easier to see warnings or errors that might occur (leave out `--production` for development).
8. You can now restart the CodiMD server!

---
###### tags: `CodiMD` `Docs`