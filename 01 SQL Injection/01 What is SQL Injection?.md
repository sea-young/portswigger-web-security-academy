
# What is SQL injection? 

It is a web application attack in which an attacker inject malicious SQL queries into input fields to gain unauthorized access to or manipulate the database. 

## Full Login Code Vulnerable to SQL Injection (PHP + MySQL)


```php
<?php
// 1. Connect to the database
$conn = mysqli_connect("localhost", "root", "1234", "mydb");

if (!$conn) {
    die("Database connection failed: " . mysqli_connect_error());
}

// 2. Get user input
$username = $_POST['username'];
$password = $_POST['password'];

// 3. Construct the query (vulnerable to SQL Injection!)
$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";

// 4. Execute the query
$result = mysqli_query($conn, $query);

// 5. Check login result
if (mysqli_num_rows($result) > 0) {
    // 6. Start session on successful login
    session_start();
    $_SESSION['username'] = $username;
    echo "Login successful!";
} else {
    echo "Login failed!";
}

// 7. Close the connection
mysqli_close($conn);

// In short, if the query returns at least one row,  
// it means the credentials are valid and the user is successfully logged in.
?>
```
```sql
SELECT * FROM users WHERE ID = 'young' AND Password = '1234'
```
\<user table\>

| ID    | Pw       |
| ----- | -------- |
| admin | admin123 |
| young | 1234     |
| guest | guest123 |

SQLi payload: `' OR 1=1 --`

```sql
SELECT * FROM users WHERE ID = '' OR 1=1 -- ' AND Password = ''
```
Since the `WHERE` clause is always true, all rows in the `users` table are returned.
