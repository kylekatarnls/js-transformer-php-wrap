# js-transformer for PHP

PHP wrapper to call any js-transformer package

## Installation

First, install `js-transformer` with [composer](http://getcomposer.org/):

```shell
composer require js-transformer/js-transformer
```

Then add the following the jstransformer packages in the
`extra.npm` section of your `composer.json` file (if
you had not yet, the previous command should have created it
at current directory opened in your terminal).

So for example if you want to use stylus and less transformers,
add the following:
```json
"extra": {
  "npm": {
    "jstransformer-stylus": "^1.4.0",
    "jstransformer-less": "^2.6.0"
  }
}
```
As you can see, you can use any
[semantic versioning](https://docs.npmjs.com/misc/semver)
as in npm or composer dependencies to specify which package
version to use.

Then use:

```shell
composer install
```

to install npm packages via composer.

## Usage

### Call a transformer

```php
$transformer = new JsTransformer();

$lessCode = '
a {
  .foo {
    color: red;
  }
}
';


echo $transformer->call('jstransformer-less', array($lessCode));
```

will display:

```css
a .foo {
  color: red;
}
```

And you can pass options, locals or as many arguments as the
js-transformer can take:

```php
$transformer = new JsTransformer();

$lessCode = '
a {
  .foo {
    color: red;
  }
}
';

$options = array(
  'compress' => true,
);


echo $transformer->call('jstransformer-less', array($lessCode, $options));
```

will display:

```css
a .foo{color: red}
```

### Check if a transformer is installed

```php
$transformer = new JsTransformer();

if ($transformer->isInstalled('jstransformer-less')) {
  echo 'Less transformer is installed.';
} else {
  echo 'You need to install less transformer to use this package.';
}
```

### Get node engine

This package use
[nodejs-php-fallback](https://github.com/kylekatarnls/nodejs-php-fallback)
to install and execute node packages.

If you need it, you can get the nodejs-php-fallback instance:

```php
if (!$transformer->getNodeEngine()->isNodeInstalled()) {
  echo 'nodejs should be installed for js-transformer to work.';
}
```
