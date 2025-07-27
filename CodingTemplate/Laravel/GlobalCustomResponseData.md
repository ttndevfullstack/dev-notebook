# 🚀 Laravel Use Cases


### 🔴 Resource Response Customization

1. Implementing a comprehensive API response::
```php
<?php
 
namespace App\Http\Resources;
 
use Illuminate\Http\Resources\Json\JsonResource;
use Illuminate\Http\Request;
 
class DocumentResource extends JsonResource
{
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'content' => $this->when(
                $request->user()->canViewContent($this->resource),
                $this->content
            ),
            'created_at' => $this->created_at,
            'updated_at' => $this->updated_at
        ];
    }
 
    public function withResponse(Request $request, $response)
    {
        // Set resource-specific headers
        $response->header('X-Document-ID', $this->id);
 
        // Add caching information for public documents
        if ($this->is_public) {
            $response->header('Cache-Control', 'public, max-age=3600');
            $response->header('ETag', md5($this->updated_at));
        }
 
        // Set appropriate status codes
        if ($this->wasRecentlyCreated) {
            $response->setStatusCode(201);
        }
 
        // Add deprecation warning for legacy fields if requested
        if ($request->has('include_legacy')) {
            $response->header('X-Deprecated-Fields', 'legacy_format,old_structure');
            $response->header('X-Deprecation-Date', '2024-07-01');
        }
    }
}
```