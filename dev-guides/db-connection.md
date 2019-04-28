# Database Connection

We use [Sequelize](https://github.com/hackmdio/codimd/blob/master/lib/models/index.js#L58) for ORM.
You could specify your database connection with all [Sequelize options](http://docs.sequelizejs.com/manual/usage.html).

## Basic usages:
You could simply use this connection string to specify basic information of the database:

```javascript=
mysql://john:qwerty@example.com:9821/hackmd_db

// The string above follows this pattern:
// <dialect>://<username>:<password>@<host>:<port>/<database>
```

This connection string can be passed to CodiMD using either `config.json` or environment variables:

| Environment Variables | config.json |
|-|-|
| CMD_DB_URL | dbURL |

## Advanced usages:
If you want to specify advanced configs for your own database, you could use the `db` variable in [`config.json`](https://github.com/hackmdio/codimd/blob/master/config.json.example).

These configs should follow this pattern:
```javascript=
"db": {
    "username": "",
    "password": "",
    "database": "codimd",
    "host": "localhost",
    "port": "5432",
    "dialect": "postgres"
},
```

For example, if you want to test your code with sqlite with in-memory storage:
```javascript=
"db": {
    "dialect": "sqlite",
    "storage": ":memory:"
}
```

:::info
:bulb: **Hint**:
- You could use any DBMS, such as: MariaDB, SQLite, or MSSQL, **BUT** we recommend only PostgreSQL or MySQL.
- We will only support PostgreSQL and MySQL in future database migrations, if any.
:::

###### tags: `CodiMD` `Docs`