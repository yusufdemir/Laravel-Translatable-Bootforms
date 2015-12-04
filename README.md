# Laravel Translatable BootForms

[![Build Status](https://travis-ci.org/Propaganistas/Laravel-Translatable-Bootforms.svg?branch=master)](https://travis-ci.org/Propaganistas/Laravel-Translatable-Bootforms)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/Propaganistas/Laravel-Translatable-BootForms/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/Propaganistas/Laravel-Translatable-BootForms/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/Propaganistas/Laravel-Translatable-Bootforms/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/Propaganistas/Laravel-Translatable-Bootforms/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/propaganistas/laravel-translatable-bootforms/v/stable)](https://packagist.org/packages/propaganistas/laravel-translatable-bootforms)
[![Total Downloads](https://poser.pugx.org/propaganistas/laravel-translatable-bootforms/downloads)](https://packagist.org/packages/propaganistas/laravel-translatable-bootforms)
[![License](https://poser.pugx.org/propaganistas/laravel-translatable-bootforms/license)](https://packagist.org/packages/propaganistas/laravel-translatable-bootforms)

Make [BootForms](https://github.com/adamwathan/bootforms) work flawlessly with [Laravel Translatable](https://github.com/dimsav/laravel-translatable)!

By importing this package, generating translatable forms using BootForms is a breeze.

### Installation

This package requires PHP >=5.6

1. In the `require` key of `composer.json` file add the following

    ```json
    "propaganistas/laravel-translatable-bootforms": "~1.0"
    ```

2. Run the Composer update command

    ```bash
    composer update
    ```

3. In your app config, add the Service Provider in the `$providers` array **after** `BootFormsServiceProvider` and `TranslatableServiceProvider`

    ```php
    'providers' => [
        AdamWathan\BootForms\BootFormsServiceProvider::class,
        Dimsav\Translatable\TranslatableServiceProvider::class,
        ...
        Propaganistas\LaravelTranslatableBootForms\TranslatableBootFormsServiceProvider::class,
    ],
    ```
4. In your app config, add the Facade to the `$aliases` array

    ```php
    'aliases' => [
        ...
        'TranslatableBootForm' => Propaganistas\LaravelTranslatableBootForms\Facades\TranslatableBootForm::class,
    ],
    ```

5. Publish the configuration file

    ```bash
    php artisan vendor:publish --provider="Propaganistas\LaravelTranslatableBootForms\TranslatableBootFormsServiceProvider" --tag="config"
    ```

### Usage

Simply use the `TranslatableBootForm` Facade as if it were `BootForm`! That's it. Multiple form inputs will now be generated for the locales set in Translatable's configuration file. They will have the corresponding value for each language and will save all of the translations without any code manipulation.

Please review [BootForms' documentation](https://github.com/adamwathan/bootforms#using-bootforms) if you're unsure how to use it.

Example:

```php
// View
{!! BootForm::text('Name', 'name')->placeholder('My placeholder') !!}

// Output
<div class="form-group">
    <label for="name">Name</label>
    <input type="text" name="name" class="form-control" placeholder="My Placeholder" />
</div>

// Controller
public function postEdit($request)
{
    $someModel->save($request->all());
}
```

```php
// View
{!! TranslatableBootForm::text('Name', 'name')->placeholder('My placeholder') !!}

// Output
<div class="form-group">
    <label for="en[name]">Name (en)</label>
    <input type="text" name="en[name]" class="form-control" placeholder="My Placeholder" data-language="en" />
</div>
<div class="form-group">
    <label for="nl[name]">Name (nl)</label>
    <input type="text" name="nl[name]" class="form-control" placeholder="My Placeholder" data-language="nl" />
</div>

// Controller
public function postEdit($request)
{
    $someModel->save($request->all());
}
```

For customizing the locale indicator in the label (and several other settings), please take a look at the configuration file.

---

#### To do

- Optimize element behavior for non-textual input types (e.g. checkbox)
