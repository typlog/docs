---
title: Theme
description: 'Documentation on how to develop your own theme.'
position: 5
---

## Preparation

To develop your own theme, you need to grab an API token at first. You can create one in your
account system. Click <https://typlog.com/account/tokens/add> to create one.

![create token](https://i.typlog.com/changelog/8367875253_678692.jpg)

PLEASE CHECK THE **theme** SCOPE BEFORE SAVING THE TOKEN.

You will also use **serve-theme** tool to develop your theme, get the executable binary
file from <https://github.com/typlog/serve-theme/releases>. Download the one matching your
OS system, rename it ot **serve-theme**.

This tool can create a localhost site for previewing your current theme.

## Templates

Let's create an example theme called **hello**. First, we create a folder **hello**, then we
add some files into this folder:

```bash
$ cd hello
$ ls
README.md  home.j2  item.j2  list.j2  hello.css  theme.json
```

In this folder, we have 3 templates:

1. `home.j2` - this is your homepage
2. `list.j2` - template for posts, episodes list/archive page
3. `item.j2` - detail page for a post, episode or page.

Another required file is `theme.json`, which contains information about your theme.

Then we can start our dev server with **serve-theme**:

```bash
$ export TOKEN=pt_xxxx   # this token is created in preparation
$ export SITE=123        # find your site ID in Typlog
$ ../serve-theme         # assuming this bin file is located in the parent folder
2021/09/21 20:54:44 Listening . on port 7000
```

Then open your browser and visit `http://localhost:7000`.

Let's add some code into `home.j2` file:

```jinja
{% block body %}
<h1>Hello Typlog</h1>
{% endblock %}
```

Then refresh your browser, you will see the changes in `http://localhost:7000`. But the
above code is useless, you have to learn the **variables** provided by Typlog to add
content into your templates.

These templates are Jinja templates. You can learn the syntax at the offical documentation:
<https://jinja.palletsprojects.com/en/3.0.x/templates/>.

## Blocks

We don't use `{% extends %}` in Typlog themes, each template inherits its own base layout which
contains many usefull code in `<head>`. What we actually need are **blocks**:

### meta

Adding more `<meta>`, `<link>` tag in **meta** block:

```jinja
{% block meta %}
<meta ....>
{% endblock %}
```

### style

Adding css in **style** block:

```jinja
{% block style %}
<style>...</style>
<link href="https://fonts.googleapis.com/css?family=Source+Sans+Pro:300,400,700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="{{ static_url }}hello.css">
{% endblock }
```

### body

This is the main block for your theme. All content should be in this block.

```jinja
{% block body %}
<header>
...
</header>
<main>
...
</main>
<footer>
...
</footer>
{% endblock %}
```

### script

This block is located at the bottom of `<body>` tag, we can add some scripts here if needed:

```jinja
{% block script %}
<script>console.log('test');</script>
{% endblock %}
```

## Variables

In the templates, we can use variables to render the real data into HTML. Here are all the
variables:

### `site`

This is a global variable, it contains the information about your site. The detail structure:

```yml
site:
  id: integer
  name: string
  slug: string
  primary_lang: string
  languages: [string, string]
  base_url: uri
  active: boolean
  summary?: string
  favicon?: uri
  touch_icon?:
    src: uri
    width: integer
    height: integer
  logo?:
    src: uri
    width: integer
    height: integer
  cover?:
    src: uri
    width: integer
    height: integer
  license?: string
  nav_links: [{ url, title, blank }]
  color: string
  socials: {}
  sponsors: {}
  podcast?:  # if enabled and has podcast information
    title: string
    subtitle: string
    description: string
    explicit: boolean
    categories: []
    type: string
    image: uri
    email: string
    author: string
    homepage: uri
    keywords: string
    copyright: string
    links?: [ { icon, url, title, blank } ]
```

### `query`

This is a global variable, it contains methods to query posts, episodes and etc.

**`.latest_subjects(subject_type=None, count=10, contains=None)`**

This will list the recent posts or episodes. This method is usually used in
`home.j2` template. The `subject_type` value can be `post` and `audio`.

Here is an example in `home.j2`:

```jinja
{% block body %}
<ul>
    {% for post in query.latest_subjects('post') %}
    <li>{{ post.title }}</li>
    {% endfor %}
</ul>
{% endblock %}
```

`contains` is a list, which can contain: `collaborators`, `tags`, and `content`.

If there is no need to show authors, tags or full content of a post, we can pass
nothing to `contains` which will improve the performance of rendering.

**`.previous_subjects(subject, count=1, same_type=True, contains=None)`**

This method can list the previous posts or episodes of the given subject(post or episode).
This is usually used in `item.j2` template:

```jinja
{% block body %}
...
<div class="previous">
  {% for post in query.previous_subjects(page, count=2) %}
    <a href="{{ post.url }}>{{ post.title }}</a>
  {% endfor %}
</div>
{% endblock %}
```

**`.previous_subject(subject, same_type=True, contains=None)`**

This method will return only the nearest previous subject. It is used in `item.j2` template.

**`.next_subjects(subject, count=1, same_type=True, contains=None)`**

This method is the same as `query.previous_subjects`, but for **next**.

**`.next_subject(subject, same_type=True, contains=None)`**

This method is the same as `query.previous_subject`, but for **next**.

**`.tags()`**

This method will list all the tags in your site:

```jinja
{% block body %}
  Tags:
  {% for tag in query.tags() %}
    <a href="{{ tag.url }}">{{ tag.title }}</a>
  {% endfor %}
{% endblock %}
```

**`.years(subject_type=None)`**

This method will list all the years of your posts or episodes.

**`query.collaborators()`**

This method will list all the authors, including hosts and guests in your site.

### `features`

This is a global variable, it contains the features this site supports:

```yml
features:
  post: boolean
  audio: boolean
  photo: boolean
  comment: boolean
  subscriber: boolean
  algolia: boolean
```

### `page`

`page` is a context aware variable. It is different in each view.

In `list.j2`, the structure is:

```yml
page:
  title: string
  url: path
  type: string
  topic: object
  items: []
  prev_year?: integer
  prev_url?: path
  next_year?: integer
  next_url?: path
```

A `topic` refers to a model about the list page. For instance, in a tag
page: `/tags/web`, this `topic` is the `tag` object, which means you
can set:

```jinja
{% set tag = page.topic %}
```

This `topic` is usually used together with `type`. Here is an example of
how to use it:

```jinja
{% macro render_tag(tag) %}
...
{% endmacro }

{% macro render_user(author) %}
{% endmacro }

...

{% block body %}
  {% if page.type == 'tag' %}
    {{ render_tag(page.topic) }}
  {% elif page.type == 'author' %}
    {{ render_tag(page.topic) }}
  {% elif page.type == 'episode_list' %}
    {{ render_podcast_info(site) }}
  {% else %}
    {{ render_site_info(site) }}
  {% endif %}
{% endblock }
```

`items` is a list of `posts` and `episodes` of current visiting page.

In `item.j2`, `page` refers to a `post`, an `episode`, or a `page` object.

```yml
page:
  id: integer
  type: string  # post, audio, page
  slug: string
  lang: string
  title: string
  cover?:
    src: uri
    width: integer
    height: integer
  subtitle?: string
  content: string
  url: path
  tags: array of tags
  collaborators: array of authors
  published_at: datetime
  updated_at: datetime
  comment: string  # open, closed, disabled
  visibility: string  # public, password, member
  render_html: function  # render_html(lazy=True) -> HTML

  # only available with "post" type
  license: string
  review?:
  reply_to?:

  # only available with "audio" type
  image: uri
  explicit: boolean
  season: integer
  episode: integer
  episode_type: string
  duration: string
  audio:
    src: uri
    size: integer
    duration: integer
    mimetype: string
```

### `static_url`

This variable will be a jsdelivr link. In development, `serve-theme` will replace
this variable to the local css file. It is usually used in:

```jinja
{% block style %}
  <link rel="stylesheet" href="{{ static_url }}hello.css">
{% endblock %}
```

### `tag`

This is not a variable, but it may be used via:

1. `query.tags()`
2. `page.topic` of **tag** type in `list.j2`
3. `page.tags` in `item.j2`

The structure of a tag:

```yml
tag:
  id: integer
  slug: string
  title: string
  summary: string
  cover?:
    src: uri
    width: integer
    height: integer
  metadata: {}
  url: path
```

### `collaborator`

This object may be one of these:

1. `query.collaborators()`
2. `page.topic` of **author** type in `list.j2`
3. `page.collaborators` in item.j2

The structure of a collaborator:

```yml
collaborator:
  username: string
  name: string
  type: string  # host, guest
  cover?:
    src: uri
    width: integer
    height: integer
  avatar: uri
  bio: html
  metadata: {}
  url: path
```

## Filters

We can use all the built-in filters provided by Jinja. And in Typlog,
there are two more filters:

1. `xmldatetime`: used to format `.published_at`

    ```jinja
    <time datetime="{{ page.published_at|xmldatetime }}">
    ```

2. `thumbnail`: used to resize image

    ```jinja
    <img src="{{ site.logo.src|thumbnail('ss') }}">
    ```

    Available sizes:

    - s: small size
    - m: middle size
    - l: large size
    - ss: crop to squared small size
    - sm: squared middle size
    - sl: squared large size


## Macros

There are some built-in macros, which can be used to help you
rendering the templates.

```jinja
{% from "macros.j2" import render_social_icons %}

This macro is used for:

{{ render_social_icons(site) }}

{% from "macros.j2" import render_item, render_navigation %}

The `render_item` is usually used in `home.j2` and `list.j2`.

{% for item in query.latest_subjects() %}
  {{ render_item(site, item) }}
{% endfor %}

The `render_navigation` macro is used in `list.j2` to render the year navigation:

{{ render_navigation(site, page) }}

{% from "macros.j2" import render_subject_visibility, render_subject_content %}
{% from "macros.j2" import render_subject_license, render_review_subject, render_subject_collaborators %}

These macros are used in `item.j2`. The `render_subject_content` is used to render the content part, it will
handle visibility automatically.

{{ render_subject_content(site, page) }}

Parameters for other macros:

{{ render_subject_visibility(site, page) }}
{{ render_subject_license(site, page) }}

{% if page.review %}
  {{ render_review_subject(page.review) }}
{% endif %}

{{ render_subject_collaborators(site, page) }}
```

## Snippets

Here are the built-in snippets which we can `{% include %}` them:

```jinja
this is typlog brand foot
{% include "_snippets/brand_foot.j2" }

this is your site foot
{% include "_snippets/site_foot.j2" }
```

Used in `item.j2`:

```jinja
{% include "_snippets/subject_share.j2" %}

{% include "_snippets/subject_foot.j2" %}
```

## JavaScript hooks

In the rendered html, Typlog will inject a `typlog.js` script. This script has many features, we can
use `js-?` class to connect the hooks.

**js-search**: popup the algolia search overlay

```jinja
{% if features.algolia %}
  <input class="js-search" type="text" placeholder="Search..." data-id="{{ site.algolia_id }}" data-key="{{ site.algolia_key }}">
{% endif %}
```

**js-subscribe**: popup the subscribe dialog

```jinja
<button class="js-subscribe">Subscribe</button>
```

**js-audio**: render the player UI

```jinja
<div class="entry-audio js-audio">
  <audio src="{{ page.audio.src }}" preload="none" controls
          {% if page.image %}data-image="{{ page.image|thumbnail('ss') }}"{% endif %}>
  </audio>
</div>

This hook is already included in

{{ render_subject_content(site, page) }}
```

**js-enjoy**: click to send `like` event

```jinja
<button class="js-enjoy">Enjoy</button>

This hook is already included in snippet:

{% include "_snippets/subject_share.j2" %}
```


## Submit your theme

When you have finished developing your theme, fill all the information in `theme.json`.
The required fields are:

```
name
author
repo
version
images
```

And then register your theme with a pull request to <https://github.com/typlog/themes>.
