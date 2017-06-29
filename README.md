# Simple PDO Class for small PHP projects

You can use this small class to query your MySQL Server from back or/and frontend projects.

View on [GitHub](https://github.com/zilus/pdo).

## Install with composer

``composer require zilus/pdo:dev-master``

### Or add it to your composer.json

```
"require": {
  "zilus/pdo": "dev-master"
}
```
### Use Autoload
``require_once 'vendor/autoload.php';``

## Manual installation
Download the source from [GitHub](https://github.com/zilus/pdo) as Zip.

Unzip and place the source files in a web accessible location. Then include or require (you can choose which method to use).

``require_once 'pdo/lib/pdo.class.php';``

# Configuration

The class expects 4 PHP constants, this if for practical use, you can create a config file or declare those constants however you want, heck, you can even pass it directly to the object.

### Via config.php

```
define("DB_HOST", "localhost");
define("DB_USER", "user");
define("DB_PASS", 'password');
define("DB_NAME", "databse");
```

### Via object

``$database = new Database("localhost", "user", "password", "database");``

# Usage

### Insert data
```
$database = new Database();
$sql="INSERT INTO table (name, lastname) VALUES (:name, :lastname)";
$database->query($sql);
$database->bind(':name', 'John');
$database->bind(':lastname', 'Doe');
$database->execute();
```

### As an array
```
$database = new Database();
$sql="INSERT INTO table (name, lastname) VALUES (:name, :lastname)";
$database->query($sql);
$database->bindArray(array(
	':name' => 'Jane',
	':lastname' => 'Doe'
));
$database->execute();
```

### Get the inserted ID
``$database->lastInsertId();``

### Chain querys
```
$database = new Database();
$database->beginTransaction();
  $sql="INSERT INTO table (name, lastname) VALUES (:name, :lastname)";
  $database->query($sql);
  $database->bind(':name', 'John');
  $database->bind(':lastname', 'Doe');
  $database->execute();

  $database->bind(':name', 'Jane');
  $database->bind(':lastname', 'Smith');
  $database->execute();

  $database->bind(':name', 'Mary');
  $database->bind(':lastname', 'Redford');
  $database->execute();
$database->endTransaction();
```

### Getting data (1 row)
```
$database = new Database();
$sql="SELECT * FROM table WHERE name = :name";
$database->query($sql);
$database->bind(':name', 'Jenny');
$row = $database->single();
echo $row['name'];
```

### Getting data (several rows)
```
$database = new Database();
$sql="SELECT * FROM table WHERE lastname = :lastname";
$database->query($sql);
$database->bind(':lastname', 'Smith');
$rows = $database->resultset();
foreach($rows as &$row) {
  echo $row['lastname'];
}
```

### Get the row count
``$database->rowCount();``

### Update data
```
$id = 14;
$sql="UPDATE table SET name = :name, lastname = :lastname WHERE id = :id";
$database->query($sql);
$database->bind(':id', $id);
$database->bind(':name', 'Mary');
$database->bind(':lastname', 'Jane');
$database->execute();
```

### Update as an array
```
$id = 14;
$sql="UPDATE table SET name = :name, lastname = :lastname WHERE id = :id";
$database->query($sql);
$database->bindArray(array(
  ':id' => $id,
  ':name' => 'Mary',
  ':lastname' => 'Jane'
));
$database->execute();
```

### Delete data
```
$id = 14;
$sql="DELETE FROM table WHERE id = :id";
$database->query($sql);
$database->bind(':id', $id);
$database->execute();
```
