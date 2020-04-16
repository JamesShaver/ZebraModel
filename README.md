<img src="https://github.com/stefangabos/zebrajs/blob/master/docs/images/logo.png" alt="zebrajs" align="right">

# Zebra_Mptt (Converted to CodeIgniter 4)

*A PHP library providing an implementation of the modified preorder tree traversal algorithm*

[![Latest Stable Version](https://poser.pugx.org/stefangabos/zebra_mptt/v/stable)](https://packagist.org/packages/stefangabos/zebra_mptt) [![Total Downloads](https://poser.pugx.org/stefangabos/zebra_mptt/downloads)](https://packagist.org/packages/stefangabos/zebra_mptt) [![Monthly Downloads](https://poser.pugx.org/stefangabos/zebra_mptt/d/monthly)](https://packagist.org/packages/stefangabos/zebra_mptt) [![Daily Downloads](https://poser.pugx.org/stefangabos/zebra_mptt/d/daily)](https://packagist.org/packages/stefangabos/zebra_mptt) [![License](https://poser.pugx.org/stefangabos/zebra_mptt/license)](https://packagist.org/packages/stefangabos/zebra_mptt)

## What is Modified Preorder Tree Traversal

MPTT is a fast algorithm for storing hierarchical data (like categories and their subcategories) in a relational database. This is a problem that most of us have had to deal with, and for which we've used an [adjacency list](http://mikehillyer.com/articles/managing-hierarchical-data-in-mysql/), where each item in the table contains a pointer to its parent and where performance will naturally degrade with each level added as more queries need to be run in order to fetch a subtree of records.

The aim of the modified preorder tree traversal algorithm is to make retrieval operations very efficient. With it you can fetch an arbitrary subtree from the database using just two queries. The first one is for fetching details for the root node of the subtree, while the second one is for fetching all the children and grandchildren of the root node.

The tradeoff for this efficiency is that updating, deleting and inserting records is more expensive, as there's some extra work required to keep the tree structure in a good state at all times. Also, the modified preorder tree traversal approach is less intuitive than the adjacency list approach because of its algorithmic flavour.

For more information about the modified preorder tree traversal method, read this excellent article called [Storing Hierarchical Data in a Database](http://blogs.sitepoint.com/hierarchical-data-database-2/).

## What is Zebra_Mptt

**Zebra_Mptt** is a PHP library that provides an implementation of the modified preorder tree traversal algorithm making it easy to implement the MPTT algorithm in your PHP applications.

It provides methods for adding nodes anywhere in the tree, deleting nodes, moving and copying nodes around the tree and methods for retrieving various information about the nodes.

Zebra\_Mptt uses [table locks](http://dev.mysql.com/doc/refman/5.0/en/lock-tables.html) making sure that database integrity is always preserved and that concurrent MySQL sessions don't compromise data integrity. Also, Zebra_Mptt uses a caching mechanism which has as result the fact that regardless of the type, or the number of retrieval operations, **the database is read only once per script execution!**

:books: Check out the [awesome documentation](https://stefangabos.github.io/Zebra_Mptt/Zebra_Mptt/Zebra_Mptt.html)!

## Support the development of this library

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=49VT6G7L5GPAS)

## Features

- provides methods for adding nodes anywhere in the tree, deleting nodes, moving and copying nodes around the tree and methods for retrieving various information about the nodes
- uses a caching mechanism which has as result the fact that regardless of the type, or the number of retrieval operations, **the database is read only once per script execution**
- uses [table locks](http://dev.mysql.com/doc/refman/5.0/en/lock-tables.html) making sure that database integrity is always preserved and that concurrent MySQL sessions don't compromise data integrity
- has [awesome documentation](https://stefangabos.github.io/Zebra_Mptt/Zebra_Mptt/Zebra_Mptt.html)
- code is heavily commented and generates no warnings/errors/notices when PHP's error reporting level is set to [E_ALL](https://web.archive.org/web/20160226192832/http://www.php.net/manual/en/function.error-reporting.php)

## Requirements

- PHP version 7.2 or newer is required, with the *[intl](https://www.php.net/manual/en/intl.requirements.php)* extension and *[mbstring](https://www.php.net/manual/en/mbstring.requirements.php)* extension installed.
- The following PHP extensions should be enabled on your server: php-json, php-mysqlnd, php-xml
- In order to use the CURLRequest, you will need libcurl installed.

A database is required for most web application programming. Currently supported databases are:
- MySQL (5.1+) via the MySQLi driver
- PostgreSQL via the Postgre driver
- SQLite3 via the SQLite3 driver

## Installation

This is written as a CodeIgniter Model, and so can simply be downloaded and copied to your 'App\Models' directory.

## Install MySQL table

Notice a directory called *install* containing a file named *mptt.sql*. This file contains the SQL code that will create the table used by the class to store its data. Import or execute the SQL code using your preferred MySQL manager (like phpMyAdmin or the fantastic Adminer) into a database of your choice.

## How to use

```php
// use the ZebraModel namespace in your controller
<?php namespace App\Controllers;
use CodeIgniter\Controller;
use App\Models\ZebraModel;
class Test extends Controller
{

public function add() {
            // instantiate a new object
            $mptt = new ZebraModel();
           
           // add 'Food' as a topmost node
            $food = $mptt->add(0, 'Food');

            // 'Fruit' and 'Meat' are direct descendants of 'Food'
            $fruit = $mptt->add($food, 'Fruit');
            $meat = $mptt->add($food, 'Meat');

            // 'Red' and 'Yellow' are direct descendants of 'Fruit'
            $red = $mptt->add($fruit, 'Red');
            $yellow = $mptt->add($fruit, 'Yellow');

            // add a fruit of each color
            $cherry = $mptt->add($red, 'Cherry');
            $banana = $mptt->add($yellow, 'Banana');

            // add two kinds of meat
            $mptt->add($meat, 'Beef');
            $mptt->add($meat, 'Pork');
            
            // move 'Banana' to 'Meat'
            $mptt->move($banana, $meat);

            // get a flat array of descendants of 'Meat'
            $mptt->get_children($meat);

            // get a multidimensional array (a tree) of all the data in the database
            echo '<pre>' . print_r($mptt->get_tree(), TRUE) . '</pre>';
        }
}        
```

:books: Check out the [awesome documentation](https://stefangabos.github.io/Zebra_Mptt/Zebra_Mptt/Zebra_Mptt.html)!
