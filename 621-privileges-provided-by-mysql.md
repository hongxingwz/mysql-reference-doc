# 6.2.1 Privileges Provided by MySQL

The privileges granted to a MySQL account determine which operations the account can perform. MySQL privileges differ in the contexts in which they apply and at different levels of operation:

* Administrative privileges enable users to manage operation of the MySQL server. These privileges are global because they are not specific to a particular database.

**Table 6.2 Permissible Privileges for GRANT and REVOKE**

| Privilege | Column | Context |
| :--- | :--- | :--- |
| ALL \[PRIVILEGES\] | Synonym for "all privileges" | Server administration |
| ALTER | Alter\_priv | Tables |
| ALTER ROUTINE | Alter\_routine\_priv | Stored routines |
| CREATE | Create\_priv | Databases, tables, or indexes |
| CREATE ROUTINE | Create\_routine\_priv | Stored routines |
| CREATE TABLESPACE | Create\_tablespace\_priv | Server administration |
| CREATE TEMPORARY TABLES | Create\_tmp\_table\_priv | Tables |
| CREATE USER | Create\_user\_priv | Server administration |
| CREATE VIEW | Create\_view\_priv | Views |
| DROP  | Drop\_priv | Databases, tables, or views |
| EVENT | Event\_priv | Databases |
| EXECUTE | Execute\_priv | Stored routines |
| FILE | File\_priv | File access on server host |
| GRANT OPTION | Grant\_priv | Databases, tables, or stored routines |
| INDEX | Index\_priv | Tables |
| INSERT | Insert\_priv | Tables or columns |
| LOCK TABLES | Lock\_tables\_priv | Databases |
| PROCESS | Process\_priv | Server administration |
| PROXY | SEE proxies\_priv table | Server administration |
| REFERENCES | References\_priv | Databases or tables |
| RELOAD | Reload\_priv | Server administration |
| REPLICATION CLIENT | Repl\_client\_priv | Server administration |
| REPLICATION SLAVE | Repl\_slave\_priv | Server administration |
| SELECT | Select\_priv | Tables or columns |
| SHOW DATABASES | Show\_view\_priv | Server administration |
| SHOW VIEW | Show\_view\_priv | Views |
| SHUTDOWN | Shutdown\_priv | Server administration |
| SUPER | Super\_priv | Server administration |
| TRIGGER | Trigger\_priv | Tables |
| UPDATE | Update\_priv | Tables or columns |
| USAGE | Synonym for "no privileges" | Server administration |



