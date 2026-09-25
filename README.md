# Kvist HTML

HTML rendering macros and helpers for
[Kvist](https://github.com/kvist-lang/kvist).

Place this repository under your project's dependency folder and import it by
relative path:

```clojure
(import html "deps/html")
```

See [the HTML guide](docs/HTML.md). Run the tests with:

```sh
kvist test tests/html-tests.kvist
```

Strings are escaped by default. The guide also documents `html.raw`, an
explicit trusted-content escape hatch for already serialized HTML and JSON-LD.
