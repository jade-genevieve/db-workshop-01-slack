# Databases Workshop 1: Querying Slack

Work in pairs. Start by forking this repo, and making sure you both have access
to the fork. Clone the repo to one of your laptops. Pair on this in driver
navigator style, but swap around for each task.

For each task below, find a **single sql query** that returns the correct
result. Add it do this document in the ` ```sql ``` ` blocks, plus any notes.
Make sure your changes are commited and pushed when you finish.

# Tasks:

## 1: Get the name of every channel created by user id 2

```sql
SELECT
    name
FROM
    channel
WHERE
    creator_id = 2;

(matches 8 rows)
```

## 2: Get the 5 most recent messages in channel id 5

```sql
SELECT
    *
FROM
    message
WHERE
    channel_id = 5
ORDER BY
    posted_at DESC
LIMIT 5;
```

## 3: Get the first 10 messages posted in channel id 11

```sql
SELECT
    *
FROM
    message
WHERE
    channel_id = 11
ORDER BY
    posted_at DESC
LIMIT 11 OFFSET 1;
```

## 4: Get the names and emails of every admin user

```sql
SELECT
    name,
    emails
FROM
    USER
WHERE
    is_admin = 1;

(returns 6 rows)
```

## 5: Get the name of every user who isn't an admin or a bot

```sql
SELECT
    name
FROM
    USER
WHERE
    is_admin = 0;

(returns 154 rows)
```

## 6: Get all the messages posted in the channel with name 'resources'

> **Remember:** You need to write this as a _single sql query_ - you can't look
> up the channel ID manually first.

```sql
SELECT
    text
FROM
    channel
    INNER JOIN message ON message.channel_id = channel.id
    WHERE
        name = 'resources';

(returns 5 rows)
```

## 7: Get the names of every user in channel 'sd-cohort3-may18'

```sql
SELECT
    user.name
FROM
    channel_member
    INNER JOIN USER ON user.id = channel_member.user_id
    LEFT JOIN channel ON channel_member.channel_id = channel.id
WHERE
    channel.name = 'sd-cohort3-may18';

(returns 22 rows)
```

## 8: Get the name of every channel that you belong to

Use your email address as part of your `WHERE` - don't query by your user id.

```sql
SELECT
    channel.name
FROM
    channel_member
    INNER JOIN channel ON channel_member.channel_id = channel.id
    LEFT JOIN USER ON channel_member.user_id = user.id
WHERE
    email = 'jade.slade@ada.ac.uk';

(returns 8 rows)
```

## 9: Get the total number of messages created

```sql
SELECT
    COUNT(text)
FROM
    message;

(returns 2938)
```

## 10: Create a query that returns the text of each message, the name of the channel it was posted in, and the name of the poster, in chronological order

Hint: there should be the same number of results as there are messages - use
this to manually check the results you're getting!

```sql
SELECT
    message.text,
    channel.name,
    user.name
FROM
    message
    INNER JOIN channel ON channel.id = message.channel_id
    LEFT JOIN USER ON message.user_id = user.id
    ORDER BY posted_at ASC;

(returns 2938 rows)
```

## 11: Find names of the 5 users who have sent the most messages

```sql
SELECT
    name,
    COUNT(text) AS total_messages
FROM
    message
    JOIN USER ON user.id = message.user_id
GROUP BY
    name
ORDER BY
    total_messages DESC
LIMIT 5;

(Casper, Danny, Ali, Alex, Siraj(!))
```

## 12: Find the names of every user who has never sent a message that didn't end with 'has joined the channel'

```sql
SELECT name
FROM user
LEFT JOIN message ON  user.id = message.user_id
WHERE message.text IS NULL;

(Returns 9 rows)
```
