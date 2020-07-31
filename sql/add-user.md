# Add a user

> create login on the server

```tsql
USE     master

CREATE  LOGIN <LOGIN_NAME> 
WITH    PASSWORD = '<PASSWORD>' 
```

> create user on the master database and then all databases you want to provide access

```tsql
CREATE  USER [<USER_NAME>] 
FOR     LOGIN [<LOGIN_NAME>] 
WITH    DEFAULT_SCHEMA = dbo; 
```

> provide roles on each database

```tsql
ALTER ROLE db_datareader    ADD MEMBER [<USER_NAME>]; 
ALTER ROLE db_datawriter    ADD MEMBER [<USER_NAME>];
ALTER ROLE db_owner         ADD MEMBER [<USER_NAME>];
```


[[sql.md]]