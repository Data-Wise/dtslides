# dtslides — Quarto Reveal.js Themes

A Quarto extension providing two reveal.js presentation themes:

- **Academic** (`dtslides-revealjs`) — Clean, neutral navy/slate palette for any institution
- **UNM** (`dtslides-unm-revealjs`) — University of New Mexico branded with UNM colors and logos

## Installing

```bash
quarto add Data-Wise/dtslides
```

This will install the extension under the `_extensions` subdirectory.
If you're using version control, you will want to check in this directory.

## Using

### Academic Theme (generic)

```yaml
format:
  dtslides-revealjs:
    slide-number: c/t
```

### UNM Theme (university branded)

```yaml
format:
  dtslides-unm-revealjs:
    slide-number: c/t
    chalkboard:
      theme: chalkboard
```

## Examples

- [Academic Theme Example](example-academic.qmd)
- [UNM Theme Example](example.qmd)
- [UNM Extended Example](example-long.qmd)
- [Template](template.qmd)

## Reference

This Quarto extension is inspired by [quarto-revealjs-clean](https://github.com/grantmcdermott/quarto-revealjs-clean) by Grant McDermott, adapted for academic and University of New Mexico use.

## License

This project is licensed under the terms of the [MIT License](LICENSE).

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to open an issue or submit a pull request.
