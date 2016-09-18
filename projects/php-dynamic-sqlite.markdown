---
title: 	   "PHP Dynamic SQLite"
layout:    project
meta:
  github:
    user: TangChr
    repo: php-dynamic-sqlite
---
Simple PHP-library for working with SQLite databases.


### Current features
- Connect to SQLite database
- Create SQLite database
- Create tables
- Insert values using arrays

### Examples

```php
<?php
/*
Example: Create table
*/
require 'dynamic_sqlite.php';
$db = 'messages.db';

$table = new SQLiteTable('message');
$table->addField('id', 'INTEGER PRIMARY KEY');
$table->addField('title', 'TEXT');
$table->addField('text', 'TEXT');

$sqlite = new SQLite($db);
$sqlite->initDb();

$sqlite->createTable($table);
?>
```
<div class="seperator"></div>

```php
<?php
/*
Example: Insert row
*/
require 'dynamic_sqlite.php';
$db    = 'messages.db';
$table = 'message';

$sqlite = new SQLite($db);
$sqlite->initDb();

$message = array('title'=>'My Title', 'text'=>'My Message');

if($sqlite->insert($table, $message))
    echo 'Success!';
?>
```