### j-Eloquent
Convert Eloquen model date attributes (created_at , ...) to Jalali (Persian) dates on the fly. ```$model->created_at``` can be converted to persian with ```$model->jalali_created_at```.

#### Installation

require following line in  your composer ```require``` secion : 


```
	"require" : {
			// Other dependecies ,
			"bigsinoos/j-eloquent" : "dev-master"
	}
```

#### Features

	- Automatically converts ``` $mode->database_date_field ``` to jalali (persian) date by adding ```jalali_``` to the begining of the propert : ```$model->jalali_database_date_field```
	- Automatically adds jalali date fields to model's ```toArray```, ```__toString```, ```toJson``` methods. useful when you return models in your controllers for json responses.
	- Can alter the prefix ( ```jalali_``` ) by adding ```getJalaliPrefix()``` method in the model.

#### Usage

you can easily use the following trait in your eloquent models :

```Bigsinoos\JEloquent\PersianDateTrait``` 

After using the trait by adding you can use persian dates with the following convention:
```
	//automatically converts $model->created_at to persian date
	// by adding 'jalali_' to the begining of the date attribute :
	$model->created_at => $model->jalali_created_at ;
```
Example :

```php
	class AdminLogs extends Eloquent {

		use Bigsinoos\JEloquent\PersianDateTrait;

	}

```
Now you can easily convert any date to jalali dates with putting ```jalali_``` in the begining of the date attribute

#### Other date attributes

Laravel treats date attributes as ```Carbon``` object. if you have other date fields (like ```user_deactivation_date```) Eloquent lets you add them to model's date attributes. read the instructions in [Official documentaion](http://laravel.com/docs/eloquent#date-mutators)

After adding your date attribute you can use it like ```$model->jalali_user_deactivation_date``` .

### In Action

```php

class Role extends Eloquent {

	use Bigsinoos\JEloquent\PersianDateTrait;

	public function getDates ()
	{
		return ['created_at', 'updated_at', 'expiration_date'];
	}
}

$role = Role::find(1);

print $role->jalali_expiration_date ; // (string) 93/06/30

print $role->expiration_date; // (string) 2014/09/21 12:00:00

print $role->toJson(); // adds 'jalali_expiration_date' along with 'expiration_date'

print $role->toArray(); // add 'jalali_expiration_date' key to the generated array through magic methods.

```