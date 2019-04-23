# Migrations and Version Changes

See release notes [here](https://github.com/hackmdio/codimd/releases).

## Migrating to 1.1.0

We deprecated the older lower case config style and moved on to camel case style. Please have a look at the current `config.json.example` and check the warnings on startup.

*Notice: This is not a breaking change right now but will be in the future*

## Migrating to 0.5.0

We no longer use LZString to compress `socket.io` data and DB data after version 0.5.0.
Run this migration tool if you're upgrading from the old version.

[Migration-to-0.5.0 migration tool](https://github.com/hackmdio/migration-to-0.5.0)

## Migrating to 0.4.0

We've dropped MongoDB after version 0.4.0.
Here is a migration tool for you to migrate the old DB data to the new one.
This tool is also used for official service.

[Migration-to-0.4.0 migration tool](https://github.com/hackmdio/migration-to-0.4.0)

---
###### tags: `CodiMD` `Docs`