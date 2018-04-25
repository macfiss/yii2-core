# Yii2-core

[![Latest Stable Version](https://poser.pugx.org/nullref/yii2-core/v/stable)](https://packagist.org/packages/nullref/yii2-core) [![Total Downloads](https://poser.pugx.org/nullref/yii2-core/downloads)](https://packagist.org/packages/nullref/yii2-core) [![Latest Unstable Version](https://poser.pugx.org/nullref/yii2-core/v/unstable)](https://packagist.org/packages/nullref/yii2-core) [![License](https://poser.pugx.org/nullref/yii2-core/license)](https://packagist.org/packages/nullref/yii2-core)

WIP

Module for administration

## Installation

The preferred way to install this extension to use [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist nullref/yii2-core "*"
```

or add

```
"nullref/yii2-core": "*"
```

to the require section of your `composer.json` file.

### Modules system

This module provides basic tools for creating system of modules.

Available modules:

 - [admin](https://github.com/NullRefExcep/yii2-admin)
 - [cms](https://github.com/NullRefExcep/yii2-cms)
 - [category](https://github.com/NullRefExcep/yii2-category)
 - [product](https://github.com/NullRefExcep/yii2-product)
 - [geo](https://github.com/NullRefExcep/yii2-geo)
 - [blog](https://github.com/NullRefExcep/yii2-blog)
 
For full integration, you have to run console command:

```
php yii module/install <module-name>
```

### Content

Core module for fast web development based on Yii2.
This package contains:

- components:
    * EntityManager - component for simple managing of entities (models)

- interfaces:
    * IAdminModule - interface for modules which can be used by [nullref\yii2-admin](https://github.com/NullRefExcep/yii2-admin)
    * IRoleContainer - interface which provide roles for RBAC
    * IEntityManager - interface EntityManager
    * IEntityManageble - interface for classes which contain EntityManager
 

### Translation overriding

Core package contain [PhpMessageSource](https://github.com/NullRefExcep/yii2-core/blob/master/src/components/i18n/PhpMessageSource.php) class that allows to merge default module's and application's messages.
Example for [admin](https://github.com/NullRefExcep/yii2-admin) module
```php
[
 /** App config **/
 'components' => [
  'i18n' => [
      'translations' => [
          '*' => ['class' => 'yii\i18n\PhpMessageSource'],
          'admin' => ['class' => 'nullref\core\components\i18n\PhpMessageSource'],
      ],
  ],
 ]
]
```

In this case messages from category `admin` from application directory will be merged with default messages from module.

### Modules migrations

Module contains `MigrateController` controller which allows work with migrations of modules.

e.g.:

`php yii core/module --moduleId=admin` -- apply migrations for module with id `admin`

Keep in mind: Migrations should have namespaces

Also, is possible work with migrations off all modules:

`php yii core/module` -- collect all possible places of migrations.

By default, migrations are looked for directory `migrations` in directory of module class.

If you want to override this behavior, you can implement `IHasMigrateNamespace` interface by module class.


### EntityManager

Component for simple work with models, which have soft delete and typification.

Config:
```php
/** module config **/
'productManager' => [
    "class" => "nullref\\product\\components\\EntityManager",
    'hasImage' => false,
    'hasStatus' => false,
    'model' => [
        'class' => 'app\\models\\Product',
        'relations' => [
            'category' => 'nullref\\category\\behaviors\\HasCategory',
            'vendor' => 'app\\behaviors\\HasVendor',
        ],
    ],
    'searchModel' => 'app\\models\\ProductSearch',
],
/** ... **/
```

Available methods:

- `createModel()` - create new model
- `createSearchModel()` - create new search model
- `findOne($condition)` - find one model by condition
- `findAll($condition)` - find all models by condition
- `find($condition = [])` - create ActiveQuery with condition
- `getMap($index = 'id', $value = 'title', $condition = [], $asArray = true)` - get key=>value array of model by condition
- `delete($model)` - delete model
- `decorateQuery($query)` decorate query by settings of entity manger

