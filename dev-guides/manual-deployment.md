# CodiMD Manual Deployment

Manual deployment is the best when you're developing CodiMD. If you are deploying the service for production use, [Docker Deployment](/s/codimd-docker-deployment) is the faster and easier choice.

## Requirements

- Node.js 8.x LTS up to 11.x.
- Database (PostgreSQL or MySQL)
    - Use charset `utf8`

:::info
:bulb: **Hint**:
- You could use other DBMS, such as: MariaDB, SQLite, or MSSQL, **BUT** we recommend only PostgreSQL or MySQL.
- We will only support PostgreSQL and MySQL in future database migrations, if any.
:::

- npm
    - npm dependencies:
        - [node-gyp](https://github.com/nodejs/node-gyp#installation)
- yarn
- `libssl-dev` for building scrypt (see [here](https://github.com/ml1nk/node-scrypt/blob/master/README.md#installation-instructions) for further information)
- Bash (for the setup script)
- For **building** CodiMD we recommend using a machine with at least **2GB** RAM


## Deployment Steps
1. Clone the [CodiMD repository](https://github.com/hackmdio/codimd.git).
```bash=
git clone https://github.com/hackmdio/codimd.git
```
2. Enter the directory, install npm dependencies and create configs.

```bash=
cd codimd
bin/setup
```

3. Configure the instance with:
    - config file, see [Configuring CodiMD with `config.json`](/s/codimd-configuration)
    - or environment variables, see [Configuring CodiMD with Environment Variables](/s/codimd-configuration)
    - **Note:** The environment variables will overwrite the `config.json`.

4. Build front-end bundle with [webpack](/s/codimd-webpack).

```bash=
npm run build
```

5. Modify the file named `.sequelizerc`, change the value of the variable `url` with your db connection string.
   For example: `postgres://username:password@localhost:5432/codimd`

6. Start the engine:
    ```bash=
    node app.js
    ```
:::info
:bulb: **Hint:** If you are developing and want webpack to continuously rebuild the front-end code, use:
```bash=
npm run dev
```
To auto-reload the server, you could use [nodemon](https://www.npmjs.com/package/nodemon) and run:
```bash=
nodemon --watch app.js --watch lib --watch locales app.js.
```
:::

###### tags: `CodiMD` `Docs`