<h1 align="center">Hi 👋, I'm Hareth</h1>
<h3 align="center">A passionate backend developer from Algeria</h3>
## Installation

#### First, create a new controller "LanguageController". You can use this command : 
```bash
php artisan make:controller LanguageController
```

go to 'app\Http\Controller\LanguageController.php' and put this code there :
```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Http\Requests;
use Config;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Redirect;
use Illuminate\Support\Facades\Session;

class LanguageController extends Controller
{
    public function switchLang($lang)
    {
        if (array_key_exists($lang, Config::get('languages'))) {
            Session::put('applocale', $lang);
        }
        return Redirect::back();
    }
}
```

#### Next, create a new config "languages". You can use this command : 
```bash
php artisan make:config languages
```

go to 'config\languages.php' and put this code there :
```php
<?php

return [
    'en' => 'English',
    'fr' => 'Français',
];
```

#### Good! Now to use our controller, we are going to set up one new route in 'routes\web.php':
```php
Route::get('lang/{lang}', ['as' => 'lang.switch', 'uses' => 'App\Http\Controllers\LanguageController@switchLang']);
```
#### Then, create a new Middleware "Language". You can use this command :
```bash
 php artisan make:middleware Language
```

go to app\Http\Middleware\Language.php and put this code there :
```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Foundation\Application;
use Illuminate\Http\Request;
use Illuminate\Routing\Redirector;
use Illuminate\Support\Facades\App;
use Illuminate\Support\Facades\Config;
use Illuminate\Support\Facades\Session;

class Language
{
    public function handle($request, Closure $next)
    {
        if (Session::has('applocale') AND array_key_exists(Session::get('applocale'), Config::get('languages'))) {
            App::setLocale(Session::get('applocale'));
        }
        else { // This is optional as Laravel will automatically set the fallback language if there is none specified
            App::setLocale(Config::get('app.fallback_locale'));
        }
        return $next($request);
    }
}
```

#### Finaly,update Kernel.php with add our Middleware :
```php
protected $middlewareGroups = [
    'web' => [
    // ...
           \App\Http\Middleware\Language::class,
    ],
    
    // ...
];
```

## Great Gob !!
#### Now if you want to add a buttons for change a Language in your site did this 
```php
<li class="dropdown">
    <a href="#" class="dropdown-toggle" data-toggle="dropdown">
        {{ Config::get('languages')[App::getLocale()] }}
    </a>
    <ul class="dropdown-menu">
        @foreach (Config::get('languages') as $lang => $language)
            @if ($lang != App::getLocale())
                <li>
                    <a href="{{ route('lang.switch', $lang) }}">{{$language}}</a>
                </li>
            @endif
        @endforeach
    </ul>
</li>
```
# Done.
<h3 align="left">Languages and Tools:</h3>
<p align="left"> <a href="https://www.php.net" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/php/php-original.svg" alt="php" width="40" height="40"/> </a> </p>
