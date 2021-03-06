---
topic: "Layouts and Includes"
desc: "The contents of the `_layouts` and `_includes` directories"
---

The `_layouts` directory contains templates for the various html renderings of pages on your site.

You typically will have a default layout, i.e. `default.html`, but then might have variations on that
for items from particular collections, such as homework assignments, labs, exams, lecture notes, handouts etc.

These might vary in terms of some common information at the top or bottom of the page, and/or whether they are
intended primarily for the browser, or to be printed.

# A typical `_layouts/default.html`

Note the line   {% raw %}<tt>{% include head.html %}</tt>{% endraw %} which pulls in the contents of `_includes/head.html`.  This is how
we can factor out things that should be in the `<head></head>` element of every page on our site, and keep 
our repo "DRY" (i.e. don't repeat yourself.)

<!--   ***  D A N G E R *** 

IF YOU CAN SEE THIS MESSAGE, YOU ARE LOOKING AT THE RAW SOURCE FOR THE MARKDOWN OR HTML... BEWARE!!!  

Note that `{% raw %}` and `{% endraw %}` that are included in the example below are GitHub's markers and are there to prevent GitHub from pulling-in the content of the respective files -- these are not included in the final files (e.g., `head.hmtl`), so make sure that they are not included (if you are copying from here, which you probably shouldn't do). 

-->

```html
<!DOCTYPE html>
<html lang="en">
  <head>
  {% raw %}{% include head.html %}{% endraw %}
  <title>{% raw %}{% if page.title %} {{ page.title }} | {% endif %} {{ site.name }}{% endraw %}</title>
  </head>
  <body id="page-top">
    <div id="container" data-role="page">
    {% raw %}{% include nav.html %}{% endraw %}
    <div id="content"  class="ui-content">
    {% raw %}{{ content }}{% endraw %}
    </div><!-- content -->
    {% raw %}{% include footer.html %}{% endraw %}
    </div><!-- container -->
  </body>
</html>
```

# A typical `_includes/head.html` (JQueryMobile)

Here is a typical `_includes/head.html` for sites that use JQueryMobile as the layout framework, along with some explanation.

```
<!-- head.html; common header items for all layouts -->
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
<link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/jquerymobile/1.4.5/jquery.mobile.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquerymobile/1.4.5/jquery.mobile.min.js"></script>
<link rel="stylesheet" href="{{site.baseurl}}/site.css" />
<script src="{{site.baseurl}}/site.js"></script>
```

* The line `<meta charset="utf-8">` is just good practice for all websites.
* The line `<meta name="viewport" content="width=device-width, initial-scale=1">` has to do with making the site
   responsive for mobile platforms.  You may need to adjust this depending on whether you are using JQueryMobile,
   or Bootstrap.
* The three lines below are part of what loads JQueryMobile into the site.  If you are using Boostrap instead,
   you won't want these, but you'll use the Bootstrap equivalent. 
   ```
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
   <link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/jquerymobile/1.4.5/jquery.mobile.min.css">
   <script src="https://ajax.googleapis.com/ajax/libs/jquerymobile/1.4.5/jquery.mobile.min.js"></script>
   ```
   
# A typical `_includes/nav.html` (JQueryMobile)

Here is a typical `_includes/nav.html` for sites that use JQueryMobile as the layout framework, along with some explanation.

```
<div data-role="header" class="header">
  <nav data-role="navbar">
  <ul>
    <li><a href="/">UCSB CS Course Repos</a></li>
    <li><a href="https://ucsb-cs-course-repos.github.io" class="ui-btn-active">{{site.course}} {{site.qtr}}</a></li>
  </ul>
 </nav>
</div>
```

To make this navbar go live, you need to make sure that it is included in your layout file:

```
<body id="page-top">
    <div id="container" data-role="page">
    {% raw %}{% include nav.html %}{% endraw %}    ...
```

