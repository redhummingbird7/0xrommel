---
# Common-Defined params
title: "Understanding and Customizing Your Hugo Tag List Widget"
date: "2025-07-19"
description: "Change the sort order of your tag list by popularity instead of alphabetic order. Also, put only a max number of tags in the list"

categories:
  - "Tutorials"
tags:
  - "Hugo"
  - "Hugo Tag List Widget"
  - "Change Tag List Order"
  - "Hugo Mainroad Theme"

# menu: main # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
# thumbnail: "img/placeholder.png" # Thumbnail image
# lead: "Example lead - highlighted near the title" # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"

# Understanding and Customizing Your Hugo Tag List Widget
---
I've been adding a bunch of articles lately (mostly [AI related](https://0xrommel.dev/posts/ai-jargon-explained/)) and my list of tags is growing. I didn't quite like the look of so many tags on the sidebar so I went on and make the needed code changes to trim it. Hope this helps someone trying to do the same thing.

The tag list is a common feature available on a lot of websites. It helps visitors navigate content quickly by topic they are interested in. This site uses [Hugo](https://gohugo.io/) so we'll break down a typical Hugo template snippet for a tag list widget. Then, we'll explore how to customize it to show the most popular tags at the top first and then limit the number of tags that shows. 

Before we go to the code, first thing to do is create a copy of the template file from your theme and place it on your partials folder. I use the Mainroad theme and the taglist code is located in the file: partials/widgets/taglist.html.

Here's the original code snippet we'll be looking at:

```go
{{- $tags := .Site.Taxonomies.tags }}
{{- if gt (len $tags) 0 }}
<div class="widget-taglist widget">
    <h4 class="widget__title">{{ T "tags_title" }}</h4>
    <div class="widget__content">
    {{- range $name, $taxonomy := $tags }}
        {{- with $.Site.GetPage (printf "/tags/%s" $name) }}
        <a class="widget-taglist__link widget__link btn" href="{{ .RelPermalink }}" title="{{ .Title }}">
            {{- .Title -}}{{- if .Site.Params.widgets.tags_counter }} ({{ $taxonomy.Count }}){{ end -}}
        </a>
        {{- end }}
    {{- end }}
    </div>
</div>
{{- end }}
```

## Deconstructing the Original Code

Let's go through this snippet line by line to understand its purpose:

  * **`{{- $tags := .Site.Taxonomies.tags }}`**: This line initializes a variable `$tags` with all the tags found across your entire Hugo site. The `{{- ... -}}` syntax is Go template shorthand for "trim whitespace" around the output, keeping your generated HTML clean.
  * **`{{- if gt (len $tags) 0 }}`**: This is a conditional check. `len $tags` counts the number of tags, and `gt ... 0` checks if that count is greater than zero. The entire widget will only render if you actually have tags defined on your site.
  * **`<div class="widget-taglist widget"> ... </div>`**: These are standard HTML `div` elements, likely used for styling the widget.
  * **`<h4 class="widget__title">{{ T "tags_title" }}</h4>`**: This creates the widget title. `{{ T "tags_title" }}` is a Hugo internationalization (i18n) function. It translates the key `"tags_title"` into the appropriate language based on your site's configuration (e.g., "Tags" for English, "タグ" for Japanese).
  * **`{{- range $name, $taxonomy := $tags }}`**: This initiates a loop that iterates over each tag in the `$tags` variable. In each iteration:
      * `$name` gets the actual tag name (e.g., "web development").
      * `$taxonomy` gets an object containing more details about that tag, including the count of associated posts.
  * **`{{- with $.Site.GetPage (printf "/tags/%s" $name) }}`**: This `with` statement tries to retrieve the "page" object for the current tag. Hugo automatically creates a list page for each taxonomy term (e.g., `yoursite.com/tags/my-tag/`). If that page exists, the code inside the `with` block executes, and `.` temporarily refers to that tag page object.
  * **`<a class="widget-taglist__link widget__link btn" href="{{ .RelPermalink }}" title="{{ .Title }}"> ... </a>`**: This creates the actual link for each tag.
      * `href="{{ .RelPermalink }}"`: Generates the relative URL to the tag's list page (e.g., `/tags/my-tag/`).
      * `title="{{ .Title }}"`: Sets the link's tooltip to the tag's title.
      * `{{- .Title -}}`: Displays the tag's name.
      * **`{{- if .Site.Params.widgets.tags_counter }} ({{ $taxonomy.Count }}){{ end -}}`**: This is an optional part. If you've set `tags_counter: true` under `[params.widgets]` in your Hugo configuration (e.g., `config.toml`), it will display the number of posts associated with that tag (e.g., "Web Development (15)").

-----

## Showing Most Used Tags (Popularity Sort)

The original code above lists **ALL** tags **alphabetically**. To sort them by popularity (most used first), we need to leverage Hugo's built-in `.ByCount` method.

Here's how to modify the code:

```go
{{- $tags := .Site.Taxonomies.tags.ByCount }} {{/* Changed */}}
{{- if gt (len $tags) 0 }}
<div class="widget-taglist widget">
    <h4 class="widget__title">{{ T "tags_title" }}</h4>
    <div class="widget__content">
    {{- range $tags }} {{/* Changed */}}
        {{- with .Page }} {{/* Changed */}}
        <a class="widget-taglist__link widget__link btn" href="{{ .RelPermalink }}" title="{{ .Title }}">
            {{- .Title -}}{{- if .Site.Params.widgets.tags_counter }} ({{ .Count }}){{ end -}} {{/* Changed */}}
        </a>
        {{- end }}
    {{- end }}
    </div>
</div>
{{- end }}
```

**What changed:**

  * **`{{- $tags := .Site.Taxonomies.tags.ByCount }}`**: We've added `.ByCount` to the `$tags` assignment. This automatically sorts your tags from the most used to the least used.
  * **`{{- range $tags }}`**: When using `ByCount`, the `range` function directly iterates over sorted taxonomy terms. Each item in the loop now contains the tag's `Term` (name), `Count` (number of posts), and `Page` (the link to its list page). This means we no longer need the `$name, $taxonomy` pair.
  * **`{{- with .Page }}`**: Since each item in the loop now has a `.Page` object, we can directly use `with .Page` to get the context for the link details (`.RelPermalink`, `.Title`).
  * **`({{ .Count }})`**: The count is now directly accessible as `.Count` on the current item in the loop.

-----

## Displaying Only the Top N Tags (e.g., Top 25)

To limit the number of tags displayed to, say, the top 25, you can combine the `.ByCount` sorting with Hugo's `first` template function.

```go
{{- $tags := .Site.Taxonomies.tags.ByCount }}
{{- if gt (len $tags) 0 }}
<div class="widget-taglist widget">
    <h4 class="widget__title">{{ T "tags_title" }}</h4>
    <div class="widget__content">
    {{- range first 25 $tags }}  {{/* Changed */}}
        {{- with .Page }}
        <a class="widget-taglist__link widget__link btn" href="{{ .RelPermalink }}" title="{{ .Title }}">
            {{- .Title -}}{{- if .Site.Params.widgets.tags_counter }} ({{ .Count }}){{ end -}}
        </a>
        {{- end }}
    {{- end }}
    </div>
</div>
{{- end }}
```

**The simple but powerful addition:**

  * **`{{- range first 25 $tags }}`**: The `first 25` function is applied *before* the `range`. It takes the sorted `$tags` list and returns only the first 25 items from it. Since the list is already sorted by popularity, these will be your 25 most popular tags.

This approach gives you a dynamic and user-friendly tag cloud, highlighting the most relevant topics on your site without overwhelming visitors with an endless list.

-----

By understanding these Hugo template functions, you can easily customize how your site's content is presented, making it more intuitive and engaging for your readers. 

#### Hope this helps. Happy blogging\!