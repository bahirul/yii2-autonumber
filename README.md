Auto Number Extension for Yii 2
===============================

Yii2 extension to genarete formated autonumber. It can be used for generate
document number.

This extension forked from [mdm/yii2-autonumber](https://github.com/mdmsoft/yii2-autonumber) with additional features.

Additional Features
-------------------

- Ability to set db connection (for multiple databases)
```php
public function behaviors()
{
	return [
		[
			'class' => 'bahirul\yii2\autonumber\Behavior',
            'db' => Yii::$app->db2, // set other database connection rather than default
			'attribute' => 'sales_num', // required
			'group' => $this->id_branch, // optional
			'value' => 'SA.'.date('Y-m-d').'.?' , // format auto number. '?' will be replaced with generated number or you can use " 'value' => function($event){ return 'SA.'.date('Y-m-d').'.?' } " as long the return value contain '?' character
			'digit' => 4 // optional, default to null. 
		],
	];
}

```

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist bahirul/yii2-autonumber "~1.0"
```

or add

```
"bahirul/yii2-autonumber": "~1.0"
```

to the require section of your `composer.json` file.


Usage
-----

Prepare required table by execute yii migrate.

```
yii migrate --migrationPath=@bahirul/yii2/autonumber/migrations
```

if wantn't use db migration. you can create required table manually.

```sql
CREATE TABLE auto_number (
    "group" varchar(32) NOT NULL,
    "number" int,
    optimistic_lock int,
    update_time int,
    PRIMARY KEY ("group")
);
```

Once the extension is installed, simply modify your ActiveRecord class:

```php
public function behaviors()
{
	return [
		[
			'class' => 'bahirul\yii2\autonumber\Behavior',
            'db' => Yii::$app->db2, // set other database connection rather than default
			'attribute' => 'sales_num', // required
			'group' => $this->id_branch, // optional
			'value' => 'SA.'.date('Y-m-d').'.?' , // format auto number. '?' will be replaced with generated number or you can use " 'value' => function($event){ return 'SA.'.date('Y-m-d').'.?' } " as long the return value contain '?' character
			'digit' => 4 // optional, default to null. 
		],
	];
}

// it will set value $model->sales_num as 'SA.2014-06-25.0001'
```

Instead of behavior, you can use this extension as validator

```php
public function rules()
{
    return [
        [['sales_num'], 'autonumber', 'format'=>'SA.'.date('Y-m-d').'.?'],
        ...
    ];
}
```
- [Original Documentation](http://mdmsoft.github.io/yii2-autonumber/index.html)