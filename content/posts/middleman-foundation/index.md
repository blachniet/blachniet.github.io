---
title: "Middleman Foundation"
date: "2014-04-29"
categories:
- "site-updates"
tags:
- "middleman"
- "zurb-foundation"
aliases:
- "/blog/middleman-foundation/"
blachnietWordPressExport: true
---

This weekend I decided to try out the static site generator, [Middleman](http://middlemanapp.com/). While I was working on converting an existing static site, I decided to take the opportunity to try out [Zurb's Foundation](http://foundation.zurb.com/) and SCSS too. [Andrea Moretti](https://github.com/axyz) created a Middleman template to get you started doing just this, but I hate just using templates without understanding what they are doing. So I decided to start with Middleman's HTML5 Boilerplate template and add the Foundation Bower package to it. You can check out the final product over on GitHub at [https://github.com/blachniet/middleman-foundation](https://github.com/blachniet/middleman-foundation). If you're not interested in the inner workings, grab [Andrea Morretti](https://github.com/axyz)'s [Middleman Foundation template](https://github.com/axyz/middleman-zurb-foundation).

## Middleman Template and Bower Setup

Initialize the project with the HTML5 Boilerplate.

```sh
> middleman init middleman-foundation --template=html5
> cd middleman-foundation
```

Create a `.bowerrc` file to specify a the install location for bower components

```json
// .bowerrc
{
    "directory" : "source/bower_components"
}
```

Initialize bower for the project.

```sh
> bower init
```

Install [Foundation](http://foundation.zurb.com).

```sh
> bower install foundation --save
```

Add the Compass configuration to `config.rb`.

```ruby
# config.rb
compass_config do |config|
  # Require any additional compass plugins here.
  config.add_import_path "bower_components/foundation/scss"

  config.output_style = :compact
end
```

Add the bower directory to the Sprockets asset path in the `config.rb`.

```ruby
# config.rb
# Add bower's directory to sprockets asset path
after_configuration do
  @bower_config = JSON.parse(IO.read("#{root}/.bowerrc"))
  sprockets.append_path File.join "#{root}", @bower_config["directory"]
end
```

## Replace CSS with SCSS

Delete `source/css/main.css` and `source/css/normalize.css`. In their place add a new file, `all.css.scss`.

```scss
# all.css.scss
@import "normalize.scss";
@import "foundation.scss";
```

Replace the stylesheet references in the `<head>` of `source/layouts/layout.erb` with the `all.css.scss`.

```html
<!-- source/layouts/layouts.erb -->
<%= stylesheet_link_tag "all" %>
```

## Use Modernizr from Bower

Create a `source/js/modernizr.js` with the following content to include the modernizr source from the installed bower components.

```js
// source/js/modernizr.js
//= require modernizr/modernizr
```

Then replace the modernizr reference in `layouts.erb`'s head with `<%= javascript_include_tag "modernizr" %>`.

## Use Other Javascripts from Bower

Delete all files (except the new `modernizr.js`) in `source/js/`, and create `all.js` with the following contents to include necessary javascripts from the bower components.

```js
// source/js/all.js
//= require jquery/dist/jquery
//= require foundation/js/foundation.min
```

Then, update `layouts.erb` to reference your new `all.js` and remove the old javascript references.

```html
<!-- source/layouts/layout.erb -->
<%= javascript_include_tag  "all" %>
```

## Serve

You're all ready to go. Launch `middleman server` and start building!
