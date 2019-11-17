# laravel-nested-attributes

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Total Downloads][ico-downloads]][link-downloads]

Nested attributes allow you to save attributes on associated records through the parent. By default nested attribute updating is turned off and you can enable it using the $nested attribute. When you enable nested attributes an attribute writer is defined on the model.

## Installation
> **Requires [PHP 7.1+](https://php.net/releases/)**

Require using [Composer](https://getcomposer.org):

``` bash
composer require --dev thomisticus/laravel-nested-attributes
```

## Usage


- Define the $nested property inside your Model. Example:
``` php
namespace App;

use Illuminate\Database\Eloquent\Model;
use Thomisticus\NestedAttributes\Traits\HasNestedAttributes;

class Post extends Model
{
    use HasNestedAttributes;

    protected $fillable = ['title'];
    
    protected $nested = ['option', 'comments'];

    public function option() {
        //it can be also morphOne or belongsTo
        return $this->hasOne('App\Option');
    }

    public function comments() {
        //it can be also morphMany or belongsToMany
        return $this->hasMany('App\Comment');
    }
```

- Then just assign the properties of your relationships and they will be created/synced automatically.

``` php
\App\Post::create([
    'title' => 'Some text',
    'option' => [
        'info' => 'some info'
    ],
    'comments' => [
        [
            'text' => 'Comment 1'
        ],
        [
            'text' => 'Comment 2'
        ],
    ]
]);


\App\Post::findOrFail(1)->update([
    'title' => 'Better text',
    'option' => [
        'info' => 'better info'
    ],
    'comments' => [
        [
            'id' => 2,
            'text' => 'Comment 2'
        ],
    ]
]);
```

- To delete nested row you should pass `_destroy` attribute:

``` php
\App\Post::findOrFail(1)->update([
    'title' => 'Better text',
    'option' => [
        'info' => 'better info'
    ],
    'comments' => [
        [
            'id' => 2,
            '_destroy' => true
        ],
    ]
]);
```


## Testing

``` bash
$ composer test
```

## Credits

- [Igor Moraes][link-author]
- [All Contributors][link-contributors]

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/thomisticus/laravel-nested-attributes.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/thomisticus/laravel-nested-attributes.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/thomisticus/laravel-nested-attributes
[link-downloads]: https://packagist.org/packages/thomisticus/laravel-nested-attributes
[link-author]: https://github.com/igorsgm
[link-contributors]: ../../contributors
