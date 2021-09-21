---
title: Content API
description: 'Content API delivers published content including posts, episodes, and moments in machine-readable formats. Developers can use the content
API to create a custom website, app, or other things.'
position: 2
---

# Content API

## JSON Feed

```
/feed.json
/posts/feed.json
/episodes/feed.json
```

[JSON Feed](https://www.jsonfeed.org/) is a format similar to RSS and
Atom but in JSON. It is easy to parse by programs. Ever website on
Typlog has one or more JSON Feeds.

Here are some examples:

```
https://blog.typlog.com/feed.json
https://lepture.com/posts/feed.json
http://fakefestival.org/episodes/feed.json
```

## oEmbed

```
https://typlog.com/oembed?url={episode_url}
```

[oEmbed](https://oembed.com/) is a format for allowing an embedded
representation of a URL on third party sites.

Here is an example:

```
https://typlog.com/oembed?url=https%3A//fakefestival.org/episodes/21
```

## Pro API

These API endpoints require **pro** plans for blogs and podcasts.

Endpoints                    | Description
---------------------------- | ----------------------------
`/api/posts`                 | List recent posts
`/api/posts?year={year}`     | List posts by year
`/api/posts/{postId}`        | Get a single post by ID
`/api/posts/slug/{slug}`     | Get a single post by slug
`/api/episodes`              | List recent episodes
`/api/episodes?year={year}`  | List episodes by year
`/api/episodes/{episodeId}`  | Get a single episode by ID
`/api/episodes/slug/{slug}`  | Get a single episode by slug

Here are some examples:

```
https://blog.typlog.com/api/posts
https://blog.typlog.com/api/posts?year=2019
https://blog.typlog.com/api/posts/10202
https://blog.typlog.com/api/posts/slug/metrics-v2

https://uxcoffee.typlog.io/api/episodes
https://uxcoffee.typlog.io/api/episodes?year=2019
https://uxcoffee.typlog.io/api/episodes/1406
https://uxcoffee.typlog.io/api/episodes/slug/66
```
