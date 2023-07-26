# Magic Constants

PHP has 9 [magical constants](https://www.php.net/manual/en/language.constants.magic.php) that change depending on where they are used.
All these "magical" constants are resolved at compile time, regular constants are resolved at runtime.
These special constants are case-insensitive.

1. `__LINE__` - The current line number of the file.
2. `__FILE__` - The full path and filename of the file with symlinks resolved. If used inside an include, the name of the included file is returned.
3. `__DIR__` - The directory of the file. If used inside an include, the directory of the included file is returned. This is equivalent to dirname(__FILE__). This directory name does not have a trailing slash unless it is the root directory.
4. `__NAMESPACE__` - The name of the current namespace.
5. `__CLASS__` - The class name. The class name includes the namespace it was declared in (e.g. Foo\Bar). When used in a trait method, `__CLASS__` is the name of the class the trait is used in.
6. `__METHOD__` - The class method name.
7. `__FUNCTION__` - The function name, or {closure} for anonymous functions.
8. `__TRAIT__` - The trait name. The trait name includes the namespace it was declared in (e.g. Foo\Bar).
9. `ClassName::class` - The fully qualified class name.

### Example

```php
<?php

namespace App;

echo 'Line: ' . __LINE__; // 5
echo 'File: ' . __FILE__; // /home/user/scripts/code.php
echo 'Dir:' . __DIR__; // /home/user/scripts

trait AngryTrait
{
	public function scream(): string
	{
		echo 'Trait: ' . __TRAIT__; // App\AngryTrait
		return 'Go away!';
	}
}

class Person
{
	use AngryTrait;
	
	function hello(string $name): string
	{
		echo 'Function: ' . __FUNCTION__; // hello
		echo 'Namespace: ' . __NAMESPACE__; // App
		echo 'Class: ' . __CLASS__; // App\Person
		echo 'Method: ' . __METHOD__; // App\Person::hello
		
		return 'Hello ' . $name;
	}
}

$person = new Person();
$person->hello('Kenny');
$person->scream();
echo Person::class; // App\Person
```
