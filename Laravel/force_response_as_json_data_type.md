# 🚀 Laravel Use Cases


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