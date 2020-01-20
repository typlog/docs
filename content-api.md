# Content API

Content API is only available for **Pro** plans.

## List Posts

List recent posts.

```
GET /api/posts
```

List all posts published in 2019.

```
GET /api/posts?year=2019
```

Example:

```
https://lepture.com/api/posts
```

## View Post

View a single post by ID:

```
GET /api/posts/<post_id>
```

Example:

```
https://lepture.com/api/posts/8569
```

View a single post by slug:

```
GET /api/posts/slug/<post_slug>
```

Example:

```
https://lepture.com/api/posts/slug/rss-style-with-xsl
```

## List Episodes

List recent episodes.

```
GET /api/episodes
```

List all episodes published in 2019.

```
GET /api/episodes?year=2019
```

Example:

```
https://uxcoffee.typlog.io/api/episodes
```

## View Episode

View a single episode by ID:

```
GET /api/episodes/<episode_id>
```

Example:

```
https://uxcoffee.typlog.io/api/episodes/1518
```

View a single episode by slug:

```
GET /api/episodes/slug/<episode_slug>
```

Example:

```
https://uxcoffee.typlog.io/api/episodes/slug/76
```
