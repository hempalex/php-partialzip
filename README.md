# PHP Partial Zip

PHP Partial Zip allows you to download files located within remote ZIP files.

Based on [planetbeing/partial-zip](https://github.com/planetbeing/partial-zip).

#### Usage:

```composer require stnvh/php-partialzip ~0.1```

##### Method usage:

###### __construct($url, $file = false):
Class init method
```php
<?php
$p = new Partial('http://some.site.com/cats.zip', 'cat.png');
# or
$p = new Partial('http://some.site.com/cats.zip');
```

###### index():
Returns a list of all the files in the remote directory
```php
<?php
/*...*/
$list = implode(' ', $p->index()); # = 'cat.png cat2.png cat3.png'
```

###### find($fileName = false):
Returns a parsed file object for use when fetching the remote file
```php
<?php
/*...*/
$file = $p->find(); # Returns the cat.png file object (as set on init)
# or
$file = $p->find('cat2.png'); # Search and return other file objects

# You can call methods here to fetch ZIP header information too
# The full list of file header properties can be found in CDFile.php
$size = $file->size(); # size in bytes
$fullName = $file->name(); # full file name in zip, including path
```

###### get($file, $output = false):
Returns, or outputs the file fetched from the remote ZIP.

**Note**: You should ensure no content is outputted before running ```->get($file, true)``` as this will cause the file download to contain invalid data.
*Hint*: put ```ob_start()``` at the start of your script, then run ```ob_clean()``` before calling get.
```php
<?php
/*...*/
if($file) {
    $data = $p->get($file); # Return
    # or
    $p->get($file, true); # Output (download)
}
```

##### example:

```php
<?php

require 'vendor/autoload.php';

use Stnvh\Partial\Zip as Partial;

ob_start(); # will capture all output

$p = new Partial('http://some.site.com/cats.zip', 'cat.png');

# Get file object
$file = $p->find();
if($file) {
    ob_clean(); # removes everything from current output to ensure file downloads correctly
    # Output to browser:
    $p->get($file, true);
}
```
