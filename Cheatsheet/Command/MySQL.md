# 🚀 MySQL Commands

**🌐 This is list popular Mysql command to use 🌐**

---

## Management Users

| Command                                                    | Description                                  |
| ---------------------------------------------------------- | -------------------------------------------- |
| `CREATE USER '<name>'@'localhost' IDENTIFIED BY '<pass>';` | Create a new user                            |
| `GRANT ALL PRIVILEGES ON <db.*>.* TO <user>@localhost`     | Grant full privileges on specify DB          |
| `GRANT SELECT ON <db.table> TO <user>@localhost`           | Grant only select privileges on Table in DB  |
| `REVOKE ALL PRIVILEGES   ON <db.*> FROM <user>@localhost`    | Revoke full privileges on specify DB         |
| `REVOKE SELECT ON <db.table> FROM <user>@localhost`        | Revoke only select privileges on Table in DB |
| `FLUSH PRIVILEGES;`                                        | Apply privileges                             |
| `SHOW GRANTS FOR <user>@localhost`                         | Show grant of a user                         |

---

## Check parameters

| Command                                                                                                | Description                                                                              |
| ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| `show global status like '%innodb_buffer_pool_read_requests%';`                                        | Check total number read request to Buffer Pool (Cache)                                   |
| `show global status like '%innodb_buffer_pool_reads%';`                                                | Check total number read request to Disk (Not found in Buffer Cache of Memory)            |
| `(innodb_buffer_pool_read_requests - innodb_buffer_pool_reads) * 100/innodb_buffer_pool_read_requests` | Calculate Buffer Cache Hit Ratio (% < 90) => Read from disk > 10% => Need to Optimize    |
| `show global status like '%Open_tables%';`                                                             | Check total table is opened in Cache                                                     |
| `show global status like '%Opened_tables%';`                                                           | Check total table is opened                                                              |
| `(Open_tables / Opened_tables) * 100`                                                                  | Calculate Table Cache Hit Ratio (% < 80) => Need to Optimize                             |
| `show variables like 'table_open_cache';`                                                              | Check parameter max table open in cache                                                  |
| `show global status like '%Open_table_definitions%';`                                                  | Check total table definition is requested in Cache                                       |
| `show global status like '%Opened_table_definitions%';`                                                | Check total table definition is requested                                                |
| `(Open_table_definitions / Opened_table_definitions) * 100`                                            | Calculate Table Definition Cache Hit Ratio (% < 80) => Need to Optimize                  |
| `show variables like 'table_definition_cache';`                                                        | Check parameter max table open in cache                                                  |
| `show global status like '%Created_tmp_disk_tables%';`                                                 | Check total number temp table is request to Disk                                         |
| `show global status like '%Created_tmp_tables%';`                                                      | Check total number temp table is request                                                 |
| `(Created_tmp_disk_tables - Created_tmp_tables) * 100/Created_tmp_disk_tables`                         | Calculate Temporary Table Hit Ratio (% < 80) => Read from disk > 20% => Need to Optimize |
