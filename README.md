# Prefs

Store and retrieve user preferences.

1. Remember form default values according
to the last submit (for instance, repeat the date limits
or the selection of an account in a dropdown 
list, according to the last search).
1. Customize the look and
feel of a website. 
1. etc.


## Usage

### Database in file system

```php
    use drey\Prefs\Factory;

    # Obtain a Prefs object
    # with file system storage and username
    $prefs = Factory::fileSystem('/path/to/directory','bob') 

    # set preference "color" for current user (bob)
    $prefs->set('color','red');
    
    # get color preference of current user
    $color = $prefs->get('color');
    
    # $color is set to 'red'
```

### Database in a RDMB

Change the DB medium accordingly, using a pdo factory.

```php
    use drey\Prefs\Factory;

    # obtain a PDO object
    $pdo = myConnect();

    # Obtain a Prefs object
    # with RDBM storage and username
    $prefs = Factory::pdo($pdo,'bob') 
 
    # set preference "color" for current user (bob)
    $prefs->set('color','red');
    
    # get color preference of current user
    $color = $prefs->get('color');
    
    # $color is set to 'red'
```

The table schema for MySQL should be (see directory `sql/`):

```sql
        CREATE TABLE `prefs`  (
          `name` varchar(255) NOT NULL,
          `value` varchar(255) NOT NULL,
          `username` varchar(32) NOT NULL,
          PRIMARY KEY (`name`,`username`),
          KEY (`username`)
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### Default values

If key is not found, a default value can be provided.

```php
    $bird = $prefs->get('bird','eagle');
    
    # $bird is set to 'eagle' if key 'bird' not found
```

### Specify preferences for other users

To assign/read the preferences of other users:

```php
    $prefs->set('fruit','lemmon','alice');
    # ...
    $prefs->get('fruit','pear','alice')
```

### Global preferences

A global username could be used to handle preferences for all users. All users must agree on the global username (for instance '*').

```php
    # one user
    $prefs->set('closing_date',$form->closing_date,'*');

    (...)

    # other users
    $closing_date = $prefs->get('closing_date',date('Y-m-d'),'*');
   
 
```

### Populate form fields:

Populate fields with default values if a field in our
model has not any value and we are creating a new record.


```php
    # before showing the form get the last value entered
    if ($model->isNewRecord && !$model->date) {
        $model->date = $prefs->get('invoice_date',date('Y-m-d'));
    }

    # after post, update value entered
    if (post) {
        $prefs->set('invoice_date',$model->date);
    }
```


## Installing with composer


The package exists in Packagist repository as `drey/prefs`.

See [drey/prefs](https://packagist.org/packages/drey/prefs).



