# 🚀 Laravel Use Cases

**🌐 A list of use cases to use in Laravel Application 🌐**

### 🔴 Improving Laravel Sanctum Personal Access Token Performance

-- Sanctum was performing a database write to update the last_used_at and update_at column on the personal_access_tokens table per request
-- After implementing this, I saw an immediate improvement from eliminating those costly database writes. This would be even more relevant if you had a more complex database setup, such as using replication and separating reads/writes.

1. First I created a new model using the artisan console:

```bash
php artisan make:model PersonalAccessToken
```

2. Then inside that model, I used this code:

```php
<?php

namespace App;

use Laravel\Sanctum\PersonalAccessToken as SanctumPersonalAccessToken;

class PersonalAccessToken extends SanctumPersonalAccessToken
{
    /**
     * Limit saving of PersonalAccessToken records
     *
     * We only want to actually save when there is something other than
     * the last_used_at column that has changed. It prevents extra DB writes
     * since we aren't going to use that column for anything.
     *
     * @param  array  $options
     * @return bool
     */
    public function save(array $options = [])
    {
        $changes = $this->getDirty();
        // Check for 2 changed values because one is always the updated_at column
        if (! array_key_exists('last_used_at', $changes) || count($changes) > 2) {
            parent::save();
        }
        return false;
    }
}
```

3. Finally, add this line to the boot method of AppServiceProvider:

```php
public function boot()
    {
        // Use our customized personal access token model
        Sanctum::usePersonalAccessTokenModel(
            PersonalAccessToken::class
        );
    }
```

### 🔴 Force Render API Exceptions As JSON Data

1. From laravel 5 to 10:
```php
class ForceJsonResponse
{
    public function handle(Request $request, Closure $next)
    {
        $request->headers->set('Accept', 'application/json');
 
        return $next($request);
    }
}
```

2. From Laravel 11:
```php
// bootstrap/app.php
 
return Application::configure(basePath: dirname(__DIR__))
 
    //...
 
    ->withExceptions(function (Exceptions $exceptions) {
        $exceptions->shouldRenderJsonWhen(function (Request $request, Throwable $e) {
            if ($request->is('api/*')) {
                return true;
            }
 
            return $request->expectsJson();
        });
    })->create();
```