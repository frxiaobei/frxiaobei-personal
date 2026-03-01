# elyfinn (Hugo theme)

`elyfinn` is a **brand layer** theme designed to be used on top of
[PaperMod](https://github.com/adityatelange/hugo-PaperMod).

It contains:
- Elyfinn color palette (primary `#C94A1A`) with dark-mode support
- Typography setup (LXGW WenKai Screen + Lora)
- PaperMod partial overrides (`extend_head.html`, `extend_footer.html`, `comments.html`)

## Usage

1) Ensure `PaperMod` exists in `themes/PaperMod` (git submodule or otherwise).

2) In your site `hugo.toml`:

```toml
theme = ["elyfinn", "PaperMod"]
```

The order matters: `elyfinn` comes first so its layouts/assets can override PaperMod.

## CSS

The stylesheet lives at:

- `themes/elyfinn/assets/css/elyfinn.css`

It is included via the `extend_head.html` partial using Hugo Pipes.

## License

MIT
