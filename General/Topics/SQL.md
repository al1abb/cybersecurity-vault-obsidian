## Basic commands

Create Database:
```sql
CREATE DATABASE [db_name]
```

Create a users table:
```sql
CREATE TABLE users(id INTEGER,name TEXT,age INTEGER);
```

Insert into users:
```sql
INSERT into users (id,name,age) values (1,'John',25);
```

Drop a table:
```sql
DROP TABLE [table1]
```

Helper functions:

> [!info] `current_schema()`
> Can be used like this `table_schema=current_schema()`
> ```sql
> current_schema()
> ```

