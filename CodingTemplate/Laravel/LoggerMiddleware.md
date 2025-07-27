# 🚀 Laravel Logger Middleware

**🌐 A Middleware to log all information of all request to easy debug in Laravel 🌐**

---

## Create LoggerMiddleware.php class

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Carbon\Carbon;
use Illuminate\Http\Request;
use Illuminate\Http\Response;
use Illuminate\Support\Facades\Log;

class LoggerMiddleware
{
    /**
     * Fields to exclude from the logged request data.
     *
     * @var array
     */
    private array $hiddenFields = ['password', 'password_confirmation'];

    /**
     * Data to log.
     *
     * @var array
     */
    private array $data = [];

    /**
     * Handle an incoming request.
     *
     * @param Request $request
     * @param Closure $next
     *
     * @return mixed
     */
    public function handle(Request $request, Closure $next)
    {
        $response = $next($request);

        $this->collectRequestData($request);
        $this->includeAuthenticatedUser($request);
        $this->filterSensitiveData($request);
        $this->collectResponseData($response);

        Log::info('Request Log:', $this->data);

        return $response;
    }

    /**
     * Collect general request data.
     *
     * @param Request $request
     */
    private function collectRequestData(Request $request): void
    {
        $headers = $request->headers->all();
        $this->data = [
            'path'         => $request->getPathInfo(),
            'method'       => $request->getMethod(),
            'ip'           => $request->ip(),
            'http_version' => $request->server('SERVER_PROTOCOL'),
            'timestamp'    => Carbon::now()->toDateTimeString(),
            'headers'      => [
                'user-agent' => $headers['user-agent'][0] ?? null,
                'referer'    => $headers['referer'][0] ?? null,
                'origin'     => $headers['origin'][0] ?? null,
            ],
        ];
    }

    /**
     * Include authenticated user data if available.
     *
     * @param Request $request
     */
    private function includeAuthenticatedUser(Request $request): void
    {
        if ($user = $request->user()) {
            $this->data['user_id'] = $user->id;
        }
    }

    /**
     * Filter out sensitive data from the request payload.
     *
     * @param Request $request
     */
    private function filterSensitiveData(Request $request): void
    {
        if (in_array($request->method(), ['POST', 'PUT', 'PATCH'])) {
            $this->data['request'] = $request->except($this->hiddenFields);
        }
    }

    /**
     * Collect relevant response data.
     *
     * @param Response $response
     */
    private function collectResponseData(Response $response): void
    {
        $contents = json_decode($response->getContent(), true, 512, JSON_THROW_ON_ERROR);

        if (!empty($contents['message'])) {
            $this->data['response']['message'] = $contents['message'];
        }

        if (!empty($contents['errors'])) {
            $this->data['response']['errors'] = $contents['errors'];
        }
    }
}
```
