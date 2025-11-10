
| **Category**              | **Command**            | **Purpose**                          | **Example**                                      | **Notes**                                                                   |
| ------------------------- | ---------------------- | ------------------------------------ | ------------------------------------------------ | --------------------------------------------------------------------------- |
| **User Management (ACL)** | `ACL USERS`            | List all ACL usernames               | `ACL USERS`                                      | Redis 6.0+ only; returns usernames like `default`, `user1`.                 |
|                           | `ACL LIST`             | Show detailed user configurations    | `ACL LIST`                                       | Shows permissions, passwords (hashed), key patterns; requires admin access. |
|                           | `ACL WHOAMI`           | Get current user’s username          | `ACL WHOAMI`                                     | Returns `user1` if connected as `user1`.                                    |
|                           | `ACL SETUSER`          | Create/update a user with rules      | `ACL SETUSER newuser on >pass ~* +@read +@write` | Requires admin; sets password, permissions, and key access.                 |
|                           | `ACL DELUSER`          | Delete a user                        | `ACL DELUSER newuser`                            | Requires admin; `user1` may lack permission.                                |
|                           | `ACL GETUSER`          | Get user’s configuration             | `ACL GETUSER user1`                              | Shows rules for `user1`; requires admin.                                    |
|                           | `ACL GENPASS`          | Generate random password             | `ACL GENPASS`                                    | Creates secure password for new users.                                      |
|                           | `ACL SAVE`             | Save ACLs to config file             | `ACL SAVE`                                       | Persists ACL changes; admin only.                                           |
| **Database Management**   | `SELECT`               | Switch to a specific database        | `SELECT 1`                                       | Your Ansible uses `1` (dev), `2` (staging), `3` (prod).                     |
|                           |                        |                                      | `SELECT 1 SET newuser:password "newuserpass123"` |                                                                             |
|                           | `KEYS`                 | List keys matching a pattern         | `KEYS user*:password`                            | Lists `user1:password` in DB 1; slow for large DBs.                         |
|                           | `SCAN`                 | Iterate keys without blocking        | `SCAN 0 MATCH user*:password`                    | Safer than `KEYS` for large datasets.                                       |
|                           | `DBSIZE`               | Get number of keys in current DB     | `DBSIZE`                                         | Run after `SELECT 1` to check dev DB size.                                  |
|                           | `FLUSHDB`              | Delete all keys in current DB        | `FLUSHDB`                                        | Destructive; clears DB 1 if run after `SELECT 1`.                           |
|                           | `FLUSHALL`             | Delete all keys in all DBs           | `FLUSHALL`                                       | Highly destructive; likely restricted for `user1`.                          |
|                           | `MOVE`                 | Move a key to another DB             | `MOVE user1:password 2`                          | Relocates key from DB 1 to DB 2.                                            |
|                           | `CONFIG GET databases` | Check number of databases configured | `CONFIG GET databases`                           | Default is 16; shows max DBs available.                                     |
| **Key-Value Operations**  | `SET`                  | Set a key-value pair                 | `SET user1:password "newpass"`                   | Used in your Ansible for `user1:password`, `user2:password`, etc.           |
|                           | `GET`                  | Retrieve a value by key              | `GET user1:password`                             | Gets dev password from DB 1.                                                |
|                           | `DEL`                  | Delete a key                         | `DEL user1:password`                             | Removes `user1:password` from DB 1.                                         |
|                           | `EXISTS`               | Check if a key exists                | `EXISTS user1:password`                          | Returns `1` (exists) or `0` (not exists).                                   |
|                           | `EXPIRE`               | Set key expiration time              | `EXPIRE user1:password 3600`                     | Key expires after 1 hour.                                                   |
|                           | `RENAME`               | Rename a key                         | `RENAME user1:password user1:cred`               | Updates key name in current DB.                                             |
| **Server Management**     | `INFO`                 | Get server information               | `INFO SERVER`                                    | Check `redis_version`, memory, clients; use `INFO MEMORY` for specifics.    |
|                           | `PING`                 | Test connection                      | `PING`                                           | Returns `PONG` if connected.                                                |
|                           | `CLIENT LIST`          | List connected clients               | `CLIENT LIST`                                    | Requires admin; shows active connections.                                   |
|                           | `CONFIG GET`           | Get configuration parameter          | `CONFIG GET requirepass`                         | Checks password or other settings.                                          |
|                           | `CONFIG SET`           | Set configuration parameter          | `CONFIG SET maxmemory 1gb`                       | Admin only; `user1` likely restricted.                                      |
|                           | `MONITOR`              | Stream executed commands             | `MONITOR`                                        | Real-time command log; use cautiously, requires permissions.                |
|                           | `SAVE`                 | Force synchronous data save          | `SAVE`                                           | Admin only; persists data to disk.                                          |
|                           | `BGSAVE`               | Save data asynchronously             | `BGSAVE`                                         | Admin only; runs in background.                                             |
| **Data Structures**       | `LPUSH` / `RPUSH`      | Add items to a list                  | `LPUSH mylist item1`                             | Adds `item1` to left of list; `RPUSH` adds to right.                        |
|                           | `LRANGE`               | Get list items                       | `LRANGE mylist 0 -1`                             | Shows all items in `mylist`.                                                |
|                           | `SADD`                 | Add item to a set                    | `SADD myset member1`                             | Adds `member1` to `myset`.                                                  |
|                           | `SMEMBERS`             | List set members                     | `SMEMBERS myset`                                 | Shows all members in `myset`.                                               |
|                           | `HSET`                 | Set field in a hash                  | `HSET myhash field1 value1`                      | Stores `field1:value1` in `myhash`.                                         |
|                           | `HGETALL`              | Get all fields/values in a hash      | `HGETALL myhash`                                 | Shows entire hash content.                                                  |

---

### Notes:
- **Context**: Commands assume you’re connected with:
  ```bash
  redis-cli -h 158.160.21.151 -p 63790 -u user1 -a 'D49ijWvxWFiBkPrlBxhBc3zJFqXPFKSn8MC3ft1O5kcRDvHH9jkqZfVPVstkjNbrHaAOV40FK1rAMiUr'
  ```
- **ACL Restrictions**: `user1` may lack permissions for admin commands (e.g., `FLUSHALL`, `CONFIG SET`). Check with:
  ```bash
  ACL LIST
  ```
- **Redis Version**: If pre-6.0 (check with `INFO SERVER`), skip ACL commands (`ACL *`). Your Ansible config suggests possible pre-6.0 usage due to password-only auth.
- **Ansible Context**: Your setup stores `user1:password`, `user2:password`, `user3:password` in DBs 1, 2, 3. Use `SELECT` and `GET`/`SET` to manage these.
- **Safety**:
  - Avoid `KEYS *` on large databases; use `SCAN` instead.
  - Commands like `FLUSHDB`/`FLUSHALL` are destructive—verify before running.
- **Execution**: Run commands in `redis-cli` after selecting the appropriate database (e.g., `SELECT 1` for dev).
