To update the **`siteurl`** and **`home`** values in the `wp_options` table of a **WordPress** database (commonly done after migrating WordPress to a new domain), you can do this using **MySQL** commands.

---

### ✅ **Option 1: Update via MySQL Command Line**

```sql
UPDATE wp_options SET option_value = 'https://newdomain.com' WHERE option_name = 'siteurl';
UPDATE wp_options SET option_value = 'https://newdomain.com' WHERE option_name = 'home';
```

> Replace `https://newdomain.com` with your actual domain.

---

### ✅ **Option 2: Update via phpMyAdmin**

1. Open `phpMyAdmin`.
2. Select your WordPress database.
3. Open the `wp_options` table.
4. Locate the `siteurl` and `home` rows (usually in the first 2 rows).
5. Click **Edit**, update the value, and save.

---

### ✅ **Option 3: Hardcode in `wp-config.php` (temporary override)**

You can also hardcode the URLs in `wp-config.php`:

```php
define('WP_HOME', 'https://newdomain.com');
define('WP_SITEURL', 'https://newdomain.com');
```

> This overrides the DB values but doesn’t update them permanently.

---

### ⚠️ Notes:

* If your table prefix is not `wp_`, use your actual prefix (e.g., `mywp_options`).
* Always take a **backup** before running update queries.
* Use `https` only if your site has SSL configured.

Would you like help with updating URLs inside posts and media links too (search & replace in serialized data)?
---
To **back up a MySQL database**, the most common and straightforward method is using the `mysqldump` utility. Here’s how to do it:

---

### ✅ **Backup a Single Database**

```bash
mysqldump -u [username] -p [database_name] > backup.sql
```

* Replace `[username]` with your MySQL user (e.g., `root`).
* Replace `[database_name]` with the name of the database.
* `backup.sql` is the output file containing the SQL dump.

You’ll be prompted to enter your password.

---

### ✅ **Backup All Databases**

```bash
mysqldump -u [username] -p --all-databases > all_databases_backup.sql
```

---

### ✅ **Backup Multiple Databases**

```bash
mysqldump -u [username] -p --databases db1 db2 db3 > multi_backup.sql
```

---

### ✅ **Backup with Compression (optional)**

```bash
mysqldump -u [username] -p [database_name] | gzip > backup.sql.gz
```

---

### ✅ **Backup with Stored Procedures, Triggers, Events**

To ensure you're backing up everything:

```bash
mysqldump -u [username] -p --routines --triggers --events [database_name] > full_backup.sql
```

---

### 📌 Extra Tips

* Run this as a scheduled job via `cron` on Linux for automation.
* Make sure the user has appropriate privileges (`SELECT`, `LOCK TABLES`, `SHOW VIEW`, `TRIGGER`, etc.).
* You can also use `--single-transaction` with InnoDB to avoid locking:

  ```bash
  mysqldump -u [username] -p --single-transaction [database_name] > backup.sql
  ```

Would you like a script to automate daily backups?
---
To **restore a MySQL database**, the exact method depends on how the backup was taken. The most common way is via a `.sql` dump file created using `mysqldump`. Here’s how to restore such a backup:

---

### ✅ **If you have a `.sql` file from `mysqldump`**

```bash
mysql -u [username] -p [database_name] < backup.sql
```

* Replace `[username]` with your MySQL username.
* Replace `[database_name]` with the name of the database you want to restore **to**.
* `backup.sql` is the name of your SQL dump file.

You’ll be prompted for the MySQL password.

---

### 📌 **If the database doesn't exist yet**

Create it first:

```bash
mysql -u [username] -p -e "CREATE DATABASE [database_name];"
```

Then restore as shown above.

---

### 🧠 Example

```bash
mysql -u root -p myapp_db < myapp_backup.sql
```

---

### 🛠️ Notes

* If your backup file includes `CREATE DATABASE` and `USE` statements, you **don’t need** to specify the database in the restore command:

  ```bash
  mysql -u root -p < myapp_backup.sql
  ```

* For large files, consider using:

  ```bash
  mysql --max_allowed_packet=512M -u root -p < large_backup.sql
  ```

Would you like guidance on restoring a database from a binary backup or using tools like `mysqlpump` or Percona XtraBackup?
