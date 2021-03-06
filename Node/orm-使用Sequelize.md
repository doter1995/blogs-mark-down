# Adservice

## 数据库使用mysql

### orm 使用Sequelize

#### 表映射使用 sequelize-auto

```text
  npm install -g sequelize-auto
  npm install -g mysql

  [node] sequelize-auto -h <host> -d <database> -u <user> -x [password] -p [port]  --dialect [dialect] -c [/path/to/config] -o [/path/to/models] -t [tableName] -C

Options:
  -h, --host        IP/Hostname for the database.   [required]
  -d, --database    Database name.                  [required]
  -u, --user        Username for database.
  -x, --pass        Password for database.
  -p, --port        Port number for database.
  -c, --config      JSON file for Sequelize's constructor "options" flag object as defined here: https://sequelize.readthedocs.org/en/latest/api/sequelize/
  -o, --output      What directory to place the models.
  -e, --dialect     The dialect/engine that you're using: postgres, mysql, sqlite
  -a, --additional  Path to a json file containing model definitions (for all tables) which are to be defined within a model's configuration parameter. For more info: https://sequelize.readthedocs.org/en/latest/docs/models-definition/#configuration
  -t, --tables      Comma-separated names of tables to import
  -T, --skip-tables Comma-separated names of tables to skip
  -C, --camel       Use camel case to name models and fields
  -n, --no-write    Prevent writing the models to disk.
  -s, --schema      Database schema from which to retrieve tables

```

例如：

```bash
sequelize-auto -o "./models" -d ad -h localhost -u root -p 3306 -x doter1995 -e mysql
```

## 由于sequelize默认情况下回自动创建两个列 creatAt和updateAt

所以需要提前在表中创建 ，或者需要配置取消creatAt和updateAt
