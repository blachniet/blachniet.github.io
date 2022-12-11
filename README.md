# blachniet.github.io

My personal website. Slowly transitioning from my WordPress site at <https://blachniet.com>.

## Theme

### Syntax highlighting

I'm using the Chroma syntax highlight feature built into Hugo. See [Syntax Highlighting](https://gohugo.io/content-management/syntax-highlighting/#generate-syntax-highlighter-css) for more information.

To generate the stylesheet:

```sh
hugo gen chromastyles --style dracula --highlightStyle 'bg:#44475a' > assets/css/extended/dracula.css
```

