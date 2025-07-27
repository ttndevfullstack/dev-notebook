# Redis Commands

============

---

_A list of my commonly used Redis commands_

_If you are interested in my Git aliases, have a look at my `.bash_profile`, found here: https://github.com/joshnh/bash_profile/blob/master/.bash_profile_

---

### Basic Snapshotting

| Command                    | Description                                                 |
| -------------------------- | ----------------------------------------------------------- |
| `KEYS *`                   | Get all keys (not recommended for large datasets)           |
| `SCAN 0 MATCH [pattern]`   | Iteratively scan keys without blocking Redis                |
| `FLUSHALL`                 | Delete all keys in all databases                            |
| `FLUSHDB`                  | Delete all keys in the current database                     |
| `EXPIRE [key] [seconds]`   | Set expiration for a key (in seconds)                       |
| `TTL [key]`                | Check the time-to-live of a key (remaining expiration time) |
| `PERSIST [key]`            | Remove the expiration from a key                            |
| `DEL [key1] [key2] ...`    | Delete one or more keys                                     |
| `UNLINK [key1] [key2] ...` | Asynchronously delete keys (non-blocking)                   |

### Get & Set

| Command                         | Description                                      |
| ------------------------------- | ------------------------------------------------ |
| `SET [key] [value]`             | Set key & value                                  |
| `MSET [key] [value] ...`        | Set multiple keys & values                       |
| `GET [key]`                     | Get the value of a key                           |
| `MGET [key] ...`                | Get values of multiple keys                      |
| `GETRANGE [key] [start] [end]`  | Get a substring of the value stored at key       |
| `INCR [key]`                    | Increment the integer value of a key by 1        |
| `DECR [key]`                    | Decrement the integer value of a key by 1        |
| `INCRBY [key] [increment]`      | Increment the integer value by a specific number |
| `DECRBY [key] [decrement]`      | Decrease the integer value by a specific number  |
| `INCRBYFLOAT [key] [increment]` | Increment the float value of a key               |

### List

| Command               | Description                                |
| --------------------- | ------------------------------------------ |
| `LRANGE [key] 0 -1`   | Get all elements of a list                 |
| `LPUSH [key] [value]` | Push a value onto the start of a list      |
| `RPUSH [key] [value]` | Push a value onto the end of a list        |
| `LPOP [key]`          | Remove and get the first element in a list |
| `RPOP [key]`          | Remove and get the last element in a list  |

### Set

| Command                   | Description                           |
| ------------------------- | ------------------------------------- |
| `SADD [key] [member] ...` | Add one or more members to a set      |
| `SMEMBERS [key]`          | Get all members in a set              |
| `SREM [key] [member] ...` | Remove one or more members from a set |

### Hash

| Command                      | Description                           |
| ---------------------------- | ------------------------------------- |
| `HSET [key] [field] [value]` | Set the value of a field in a hash    |
| `HGET [key] [field]`         | Get the value of a field in a hash    |
| `HGETALL [key]`              | Get all fields and values of a hash   |
| `HDEL [key] [field] ...`     | Delete one or more fields from a hash |

### Key Patterns & Deletion

| Command                            | Description                                                             |
| ---------------------------------- | ----------------------------------------------------------------------- |
| `DEL $(redis-cli KEYS "pattern*")` | Delete all keys matching a pattern (not recommended for large datasets) |
| `SCAN 0 MATCH "pattern*"`          | Use SCAN to match and delete keys iteratively                           |
| `EXISTS [key]`                     | Check if a key exists                                                   |
