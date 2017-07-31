# Import Posts from Other services

Typlog supports import from a json file currently. Upload json file in `settings/advanced`.

The json file format:

```json
{
  "posts": [
    {
      "title": "An example title",
      "slug": "dash-connected-slug",
      "headline": "A headline of the post (optional)",
      "content": "Markdown format content of the post",
      "tags": [
        {
          "slug": "tag-slug",
          "title": "tag title"
        }
      ],
      "created_at": "2016-02-02T09:23:00Z",
      "published_at": "2016-02-02T09:23:00Z"
    }
  ]
}
```
