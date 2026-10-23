# Lava Lamp

A TRMNL recipe that fills the whole display with a slowly evolving, abstract lava lamp: red wax blobs drifting through a contrasting liquid, advancing a little with each refresh.

The full brief is in [docs/REQUIREMENTS.md](docs/REQUIREMENTS.md).

## Layout

```
.trmnlp.yml               local preview config (custom field values, variable overrides)
src/settings.yml          recipe settings: static strategy, no screen padding, 15 min refresh
src/shared.liquid         prepended to every view; home of the scene renderer
src/full.liquid           full-screen view
src/half_horizontal.liquid
src/half_vertical.liquid
src/quadrant.liquid       mashup views, each a .lava container
tests/plugin_spec.rb      `trmnlp test` spec
.github/workflows/trmnl.yml  lint + test on PRs; pushes to TRMNL from main
bin/trmnlp                runs trmnlp from the gem if installed, otherwise via Docker
```

The recipe uses the `static` strategy: the scene is generated procedurally and needs no external data.

## Development

Uses [trmnlp](https://github.com/usetrmnl/trmnlp), either the `trmnl_preview` gem (Ruby 4.0+) or Docker through `bin/trmnlp`.

```sh
bin/trmnlp serve    # local preview at http://localhost:4567
bin/trmnlp lint     # recipe best-practice checks
bin/trmnlp test     # spec in tests/ (add --report report for screenshots)
```

## Publishing

```sh
bin/trmnlp login
bin/trmnlp push
```

The workflow pushes automatically on commits to `main` once a `TRMNL_API_KEY` repository secret is set.
