# HTML

`html` renders Hiccup-shaped Kvist `Data`. Contextual Data literals make the
inline form concise: the renderer's parameter type supplies `Data` context, so
expressions evaluate normally without quote or unquote syntax.

```clojure
(import html "deps/html")

(let [title "Status"
      page (html.render
             [:section {:class "panel"}
              [:h1 title]
              [:p "Ready <ok>"]]) :defer]
  (println page))
```

Tags and attribute names are keywords. Strings are text or attribute values
and are escaped by default. Nil omits a child or attribute; boolean attributes
render as bare names when true and disappear when false.

Tags support Hiccup id and class shorthand. Put an id after `#` and one or
more classes after `.`:

```clojure
(html.render
  [:div#daily-report-configuration.report.daily "Ready"])
;; => <div id="daily-report-configuration" class="report daily">Ready</div>
```

An explicit `:id` or `:class` in the attribute map overrides the corresponding
shorthand value.

```clojure
(html.render
  [:form {:data-on:submit__prevent "@post('/capture')"}
   [:input {:disabled false :data-bind "captureTitle"}]])
```

HTML void elements do not receive closing tags and cannot contain children.
Use `:<>` for a fragment:

```clojure
(html.render
  [:<>
   [:h2 "One"]
   [:h2 "Two"]])
```

## Expressions and reusable Data

Symbols and calls in contextual collections are evaluated and converted to
Data. Conditionals inherit the same expected type:

```clojure
(html.render
  [:main
   [:h1 title]
   (if ready?
     [:p "Ready"]
     nil)])
```

Existing Data uses the same overload:

```clojure
(def page '[:article {:class "card"} [:h1 "Stored"]])
(html.render page)
```

`html.try-render-data` returns `[html error ok]` for untrusted Data.
`html.render` asserts with the same validation message when its Data is
invalid. Tags and attribute names must be non-empty keywords; supported child
values are nil, booleans, numbers, strings, and nested element vectors.

Passing a string to `html.render` renders it as escaped text:

```clojure
(html.render "A&B <ok>")
;; => "A&amp;B &lt;ok&gt;"
```

## Documents

`html.render-document` renders Data through the same rules and prepends the
HTML doctype:

```clojure
(html.render-document
  [:html {:lang "en"}
   [:head [:title "Status"]]
   [:body [:main "Ready"]]])
```

## Templates

`html.template` accepts a string alone or a string plus contextual Data map.
Binding names are keywords and values are strings:

```clojure
(html.template "<p>{{name}}</p>" {:name name})
```

`html.render-file` reads a template at macro expansion time:

```clojure
(html.render-file "html-template.html" {:name "Ada"})
```

All rendering functions return owned strings.
