---
layout: page
title: Config
description: >
  Once Jekyll is running, you can start with basic configuration by adding various entries to `_config.yml`.
redirect_from:
  - /docs/latest/config/
  - /docs/config/
  - /docs/latest/configuration/
  - /docs/configuration/
---

Once Jekyll is running, you can start with basic configuration by adding various entries to `_config.yml`.
Besides these descriptions, you can also read the [annotated config file](#annotated-config-file) below.

**NOTE**: When making changes to `_config.yml`, it is necessary to restart the Jekyll process for the changes to take effect.
{:.message}

## Table of Contents
{:.no_toc}
0. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Setting `url` and `baseurl`
The first order of business should be to set the correct `url` and `baseurl` values in `_config.yml`.

The `url` is the domain of your site, including the protocol (`http` or `https`). For this site, it is

~~~yml
# file: _config.yml
url: https://qwtel.com
~~~

If your entire Jekyll blog is hosted in a subdirectory of your page, provide the path in `baseurl` with a leading `/`, but no trailing `/`,
e.g.

~~~yml
# file: _config.yml
baseurl: /hydejack
~~~

Otherwise, provide the empty string `''`

### GitHub Pages
When hosting on [GitHub Pages](https://pages.github.com/) the `url` is `https://<username>.github.io`
(unless you are using a custom domain).

The `baseurl` depends on the kind of page you are hosting.

* When hosting a *user or organization page*, use the empty string `''`.
* When hosting *project page*, use `/<reponame>`.

For for information on the types of pages you can host on GitHub, see the
[GitHub Help article](https://help.github.com/articles/user-organization-and-project-pages/).

## Changing accent colors and sidebar images
Hydejack allows you to choose the background image of the sidebar, as well as the accent color
(color of the links, selection and focus outline, etc...) on a per-page, per-category, per-tag, per-author and global basis.

Set the fallback values in `_config.yml`, which are used should no other rule (page, category, tag, author) apply:

~~~yml
# file: _config.yml
accent_image: /assets/img/sidebar-bg.jpg
accent_color: '#A85641'
~~~

**NOTE**: I recommend using a blurred image in order for the text to remain readable.
If you save a blurred image as JPG, it will also drastically reduce its file size.
{:.message}


The `accent_image` property also accepts the special value `none` which will remove the default image.

You can also provide a single color instead of an image like this:

~~~yml
# file: _config.yml
accent_image:
  background: '#202020' # provide a valid CSS background value
  overlay:    false     # set to true if you want a dark overlay
~~~

## Changing fonts
Hydejack lets you configure the font of regular text and headlines, and it has built-in support for Google Fonts.
There are three keys in `_config.yml` associated with this: `font`, `font_heading` and `google_fonts`.
The defaults are:

~~~yml
# file: _config.yml
font:         "'Noto Sans', Helvetica, Arial, sans-serif"
font_heading: "'Roboto Slab', Helvetica, Arial, sans-serif"
google_fonts: "Roboto+Slab:700|Noto+Sans:400,400i,700,700i"
~~~

`font` and `font_heading` must be valid CSS `font-family` values. When using Google Fonts make sure they consist of at least two fonts
(everything except the first entry will be used as a fallback until the fonts have completed loading).

The `google_fonts` key is the string necessary to fetch the fonts from Google.
You can get it from the download page at [Google Fonts](https://fonts.google.com) after you've selected one or more fonts:

![Where to get the google_fonts string](../../assets/img/docs/google-fonts.png){:width="600" height="398"}

### Using safe web fonts
If you prefer not to use Google Fonts and use [safe web fonts](http://www.cssfontstack.com/) instead,
set `no_google_fonts` to `true`:

```yml
# file: _config.yml
hydejack:
  no_google_fonts: true
```

In this case, `font` and `font_heading` do not have to contain more than one font.
You may also remove the `google_fonts` key in this case.

## Choosing a blog layout
Hydejack features two layouts for showing your blog posts.

* The [`list` layout][posts] only shows the title and groups the posts by year of publication.
* The [`blog` layout][blog] is a traditional paginated layout and shows the title and an excerpt of each post.

In order to use the `list` layout add the following front-matter to a new markdown file:

~~~yml
---
layout: list
title:  Home
---
~~~

If you want to use the `blog` layout, you need to add `jekyll-paginate` to your `Gemfile` and to the `plugins` list in your config file:

```ruby
# file: Gemfile
gem "jekyll-paginate"
```

```yml
# file: _config.yml
plugins:
  - jekyll-paginate
```

You also need to add the `paginate` and `paginate_path` keys to your config file, e.g.

~~~yml
# file: _config.yml
paginate:      5
paginate_path: '/page-:num/'
~~~

The `blog` layout needs to be applied to a file with the `.html` file extension
and the `paginate_path` needs to match the path to the `index.html` file.
To match the `paginate_path` above, put a `index.html` with the following front matter in the root directory:

~~~yml
# file: index.html
---
layout: blog
title: Blog
---
~~~

For more information see [Pagination](https://jekyllrb.com/docs/pagination/).

### Using the `blog` layout in a subdirectory
If you want to use the blog layout at a URL like `/my-blog/`, create the following folder structure:

~~~
├── my-blog
│   └── index.html
├── !my-blog.md
└── _config.yml
~~~

You can use the same `index.html` as before:

~~~yml
# file: my-blog/index.html
---
layout: blog
title: Blog
---
~~~

(Optional) If you want to add a link to the blog in the sidebar, DO NOT add the `menu` key to the front matter of `my-blog/index.html`.
Instead, create a new markdown file called `!my-blog.md` with `menu` and `permalink` keys:

~~~yml
# file: !my-blog.md
---
title: My Blog
menu: true
permalink: /my-blog/
---
~~~

Finally, in your config file, make sue the `paginate_path` matches the `permalink`:

~~~yml
# file: _config.yml
paginate:      5
paginate_path: /my-blog/page-:num/
~~~

## Adding an author
As a bare minimum, you should add an `author` key with a `name` and `email` sub-key
(used by the [feed plugin](https://github.com/jekyll/jekyll-feed)) to to your config file:

~~~yml
# file: _config.yml
author:
  name:  Florian Klampfer
  email: mail@qwtel.com
~~~

If you would like the author to be displayed in the about section below a post or project\*,
as well as the top of about and welcome\* pages, add an `about` key and provide some markdown content.
I recommend using the YAML pipe `|` syntax, so you can include multiple paragraphs:

~~~yml
# file: _config.yml
author:
  name:  Florian Klampfer
  email: mail@qwtel.com
  about: |
    Hi, I'm Florian or @qwtel...

    This is another paragraph.
~~~

### Adding an author's picture
If you'd like for the author's picture to appear in addition the the about text (see above),
you can either use the [`jekyll-avatar`](https://github.com/benbalter/jekyll-avatar) plugin or provide URLs to images manually.

To use the plugin, add it to your `Gemfile` and the list of `plugins` in your config file:

```ruby
# file: Gemfile
gem "jekyll-avatar"
```

```yml
# file: _config.yml
plugins:
  - jekyll-avatar
```

Run `bundle install` for the changes to take effect.

Make sure you have provided a GitHub username in your config file (`github_username`),
or to the author key (`author.social.github`, `author.github.username`, or `author.github`).
See [Adding social media icons](#adding-social-media-icons) for more.

To set an image manually, you have to provide an URL to the author's `picture` key:

~~~yml
# file: _config.yml
author:
  picture:  /assets/img/me.jpg
~~~

If you'd like to provide multiple versions for screens with different pixel densities,
you can provide `path` and `srcset` keys instead:

~~~yml
# file: _config.yml
author:
  picture:
    path:   /assets/img/me.jpg
    srcset:
      1x:   /assets/img/me.jpg
      2x:   /assets/img/me@2x.jpg
~~~

The `path` key is a fallback image for browsers that don't support the `srcset` attribute.

The keys of the `srcset` hash will be used as image descriptors.
For more information on `srcset`, see the
[documentation at MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-srcset), or
[this article from CSS-Tricks](https://css-tricks.com/responsive-images-youre-just-changing-resolutions-use-srcset/).

### Adding social media icons
Hydejack supports a variety of social media icons out of the box.
These are defined on a per-author basis, so make sure you've followed the steps in [Adding an author](#adding-an-author).

**NOTE**: If you are using the gem-based version of Hydejack,
download [`social.yml`](https://github.com/qwtel/hydejack/blob/master/_data/social.yml)
and put it into `_data` in the root directory.
This is necessary because gem-based themes do not support including `_data`.
{:.message}

You can add a link to a social network by adding an entry to the `social` key in to an author.
It consists of the name of the social network as key and your username within that network as value, e.g.

~~~yml
# file: _config.yml
author:
  social:
    twitter: qwtel
    github:  qwtel
~~~

Check out [`authors.yml`](https://github.com/qwtel/hydejack/blob/master/_data/authors.yml) to see which networks are available.
You can also follow the steps [here](advanced.md) to add your own social media icons.

You can change the order in which the icons appear by moving lines up or down, e.g.

~~~yml
# file: _config.yml
author:
  social:
    github:  qwtel # now github appears first
    twitter: qwtel
~~~

To get an overview of which networks are available and how a typical username in that network looks like,
see the included [`authors.yml`](https://github.com/qwtel/hydejack/blob/master/_data/authors.yml).

Should providing a username not produce a correct link for some reason, you can provide a complete URL instead, e.g.

~~~yml
# file: _config.yml
author:
  social:
    youtube: https://www.youtube.com/channel/UCu0PYX_kVANdmgIZ4bw6_kA
~~~

**NOTE**: You can add any platform, even if it's not defined in [`social.yml`](https://github.com/qwtel/hydejack/blob/master/_data/social.yml),
by providing a complete URL. However, a fallback icon <span class="icon-link"></span> will be used when no icon is available.
Supplying your own icons is an [advanced topic](advanced.md).
{:.message}


### Adding an email, RSS icon or download icon
If you'd like to add an email <span class="icon-mail"></span>, RSS <span class="icon-rss2"></span>, or download <span class="icon-box-add"></span> icon to the list,
add the `email`, `rss`, or `download` key, e.g.:

~~~yml
# file: _config.yml
author:
  social:
    email:    mail@qwtel.com
    rss:      {{ site.url }}{{ site.baseurl }}/feed.xml # make sure you provide an absolute URL
    download: https://github.com/qwtel/hydejack/archive/v8.0.0-beta.1.zip
~~~

## Enabling comments
Hydejack supports comments via [Disqus](https://disqus.com/).
Before you can add comments to a page you need to register and add your site to Disqus' admin console.
Once you have obtained your "Disqus shortname", you include it in your config file:

~~~yml
# file: _config.yml
disqus: <disqus shortname>
~~~

Now comments can be enabled by adding `comments: true` to the front matter.

~~~yml
---
layout:   post
title:    Hello World
comments: true
---
~~~

You can enable comments for entire classes of pages by using
[front matter defaults](https://jekyllrb.com/docs/configuration/#front-matter-defaults).
E.g. to enable comments on all posts, add to your config file:

~~~yml
# file: _config.yml
defaults:
  - scope:
      type: posts
    values:
      comments: true
~~~

## Enabling Google Analytics
Enabling Google Analytics is as simple as setting the `google_analytics` key.

~~~yml
# file: _config.yml
google_analytics: UA-XXXXXXXX-X
~~~

Conversely, if you want to disable it, you only have to remove the key and no GA code will be part of the generated pages.

## Changing built-in strings
You can change the wording of built-in strings like "Related Posts" or "Read more" in `_data/strings.yml`.
If you are using the gem-based version, you can get the default file
[here](https://github.com/qwtel/hydejack/blob/master/_data/strings.yml).

You will frequently find markers like `<!--post_title-->`.
You can place them freely within your string and they will be replaced with the content they refer to.

You may also use this feature to translate the theme into different languages.
In this case you should also set the `lang` key to your config file, e.g.

```yml
# file: _config.yml
lang: cc-ll
```

where `cc` is the 2-letter country code and `ll` specifies a 2-letter location code, e.g.: `de-at`.

You may also change the strings used for formatting dates and times (look out for the `date_formats` key),
but be aware that the values you provide need to be valid
Ruby [format directives](http://ruby-doc.org/core-2.4.1/Time.html#method-i-strftime).


## Adding legal documents
If you have pages for contact data, privacy policy, cookie policy, etc, you can add links to them in the footer by listing them under the `legal` key in your config file as follows:

```yml
legal:
  - title: Impress
    href:  /impress/
  - title: Cookies Policy
    href:  /cookies-policy/
```

When using Hydejack's offline feature, the pages listed here will be downloaded and cached when loading the page for the first time.


## Adding custom favicons and app icons
By default, Hydejack includes its own favicon, as well as app icons for in five different resolutions.

To change the favicon, place your own `favicon.ico` into `assets/icons/` (create the folder if it doesn't exist).

To use your own app icons, you need to prepare five square PNG files in the following resolutions, and put them into `assets/icons/` (create the folder if it doesn't exist):

| Name             | Pixels    |
|:-----------------|----------:|
| `icon@3x.png`    | `576x576` |
| `icon@2x.png`    | `384x384` |
| `icon.png`       | `192x192` |
| `icon@0,75x.png` | `144x144` |
| `icon@0,5x.png`  |   `96x96` |
| `icon@0,25x.png` |   `48x48` |

Additionally, you can provide tiles for Window 10:

| Name              | Pixels    |
|:------------------|----------:|
| `tile-large.png`  | `558x588` |
| `tile-medium.png` | `270x270` |
| `tile-small.png`  |   `70x70` |
| `tile-wide.png`   | `558x270` |

If you don't want to use PNGs, or want to use different resolutions, you have to provide your own `assets/manifest.json` (and `assets/ieconfig.xml` when supporting Window 10). For more on web app manifests, see [MDN](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json).

**NOTE**: In any case, Hydejack expects a `assets/icons/icon.png` file for use as `apple-touch-icon` and a `assets/icons/favicon.ico` for use as `shortcut icon`.
{:.message}


## Enabling newsletter boxes*
To enable showing newsletter subscription boxes below each post and project,
provide your [Tinyletter] username to the `tinyletter` key in the config file.

```yml
# file: _config.yml
tinyletter:  <tinyletter username>
```

To edit the content of the newsletter box, open `_data/strings.yml`, and change the entries under the `tinyletter` key.

If want to use a different mailing provider, like MailChimp, you can build your own form,
and insert it into `_includes/my-newsletter.html`.
There you will also find an example form for MailChimp, where you need to fill in `site.mailchimp.action` and `site.mailchimp.hidden_input`
(you can get these from MailChimp).

To build a completely new from, you can use [the same CSS classes as Bootstrap](https://getbootstrap.com/docs/4.0/components/forms/).
Note that only form, grid and utility classes are available.
Check out [Forms by Example](../../forms-by-example.md){:.heading.flip-title} for some examples.


Continue with [Basics](basics.md){:.heading.flip-title}
{:.read-more}

[blog]: https://hydejack.com/blog/
[posts]: https://hydejack.com/posts/
[tinyletter]: https://tinyletter.com/

*[FOIT]: Flash of Invisible Text
*[GA]: Google Analytics