# Typlog API

Typlog API Documentation. (In development)

## Authentication

Find your authentication token at (...), send requests with this token:

```http
GET /api/my/sites
Authorization: Bearer your-token-string
```

## API Endpoints

All API endpoints require authentication.

### User Endpoints

#### List all your blogs:

```http
GET /api/my/sites
```

#### List your flickr photos:

```http
GET /api/my/flickr
```

### Site Endpoints

API that associated with a blog site requires `X-Typlog-Site` header. e.g.

```http
GET /api/posts
Authorization: Bearer your-token-string
X-Typlog-Site: lepture
```

#### List Posts

```http
GET /api/posts
```

Params:

1. next: next cursor ID
2. draft: list drafts instead

#### Create Post

```http
POST /api/posts
```

#### View Post

```http
GET /api/posts/<post_id>
```

#### Update Post

```http
POST /api/posts/<post_id>
```

#### Delete Post

```http
DELETE /api/posts/<post_id>
```

#### List Pages

```http
GET /api/pages
```

#### Create Page

```http
POST /api/pages
```

#### View Page

```http
GET /api/pages/<page_id>
```

#### Update Page

```http
POST /api/pages/<page_id>
```

#### Delete Page

```http
DELETE /api/pages/<page_id>
```
