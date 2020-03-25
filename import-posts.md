# Import Posts from Other services

Typlog supports import from a json file currently. Upload json file in **Labs**.

The json file format:

```json
{
  "posts": [
    {
      "title": "An example title",
      "subtitle": "Subtitle of the post (optional)",
      "slug": "dash-connected-slug",
      "content": "Markdown format content of the post",
      "status": "published",
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
