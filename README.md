# Sample of error

To get started, just run:

```bash
docker-compose up
```

This will initialize everything and then can login to the database to test with the following credentials:

```
db host: 127.0.0.1
db port: 3306
db name: public_test
db username: test_user
db password: password123
```

```bash
# mysql cli
mysql -u test_user -p -h 127.0.0.1
```

## Details of the example

### Tables

- chats_members
- chats_messages
- chats_messages_read_by_recipient

### SQL Statement

The following SQL statement will fail in ShardingSphere

```sql
SELECT
  members.chat_id,
  mess_max.created_at message_sent
FROM chats_members members
JOIN (
  SELECT
    MAX(id) max_id,
    chat_id,
    MAX(created_at) AS created_at
  FROM chats_messages
  GROUP BY chat_id
) mess_max ON
  (mess_max.chat_id = members.chat_id)
LEFT JOIN chats_messages_read_by_recipient is_read ON
  mess_max.max_id = is_read.chat_message_id
    AND is_read.created_by = 5094
WHERE
  (members.user_id = 5094 OR members.group_id IN('13'))
    AND is_read.id IS NULL
```
