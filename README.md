# blachniet.com

My personal website.

## Writing

### Publishing checklist

- [ ] Title
- [ ] Date
- [ ] Tags
- [ ] Slug
- [ ] Spell
- [ ] Cross-post

### Hugo options

While editing, include these options:

```sh
hugo server --noTimes --buildDrafts --navigateToChanged
```

- `--noTimes` - I seem to have trouble with some sort of `chtimes` error on my Mac.
- `--buildDrafts` - So I can see what I'm working on.
- `--navigateToChanged` - Automatically jump to the page I just changed.

## Theme

### Syntax highlighting

I'm using the Chroma syntax highlight feature built into Hugo. See [Syntax Highlighting](https://gohugo.io/content-management/syntax-highlighting/#generate-syntax-highlighter-css) for more information.

To generate the stylesheet:

```sh
hugo gen chromastyles --style dracula --highlightStyle 'bg:#44475a' > assets/css/extended/dracula.css
```

See the [List of Chroma Highlighting Languages](https://gohugo.io/content-management/syntax-highlighting/#list-of-chroma-highlighting-languages).

## Tools

These are some tools I used to create this site.

- [wordpress-export-to-markdown][1] with some of my own [customizations][2].
- [favicon.ico][3]

[1]: https://github.com/lonekorean/wordpress-export-to-markdown 
[2]: https://github.com/blachniet/wordpress-export-to-markdown
[3]: https://favicon.io
