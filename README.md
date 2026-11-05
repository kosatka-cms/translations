# Kosatka translations

The interface translations of [Kosatka](https://github.com/kosatka-cms/kosatka):
the engine, its standard modules and the Kosatka theme. English is the
language of the code; every other language lives here, and sites download
it from the admin (*Configuration → Interface translations → Languages*)
or with `app translations add <code>`.

The catalog sites read is published from this repository to
<https://kosatka-cms.github.io/translations/index.json> - the engine's
default `translations_catalog_url`.

## Languages

| Code | Language | State |
|---|---|---|
| `de` | Deutsch | a draft: machine translated, to be reviewed |
| `es` | Español | a draft: machine translated, to be reviewed |
| `ru` | Русский | complete, reviewed |

## Layout

```
<code>/language.yaml        the language's version and whether it is a draft
<code>/engine.json          the engine's strings
<code>/<module>.json        a standard module's strings (polls.json, forms.json…)
<code>/theme-kosatka.json   the Kosatka theme's strings
```

`language.yaml`:

```yaml
version: 0.1.0   # raise it with each change: sites see it as an update
draft: false     # true - machine translated, not reviewed yet
```

A component file is a catalog: the English string is the key, the
translation the value; a plural string has its forms in the order the
language's plural categories go (zero, one, two, few, many, other -
English and German have two, Russian three); strings with a context are
under `contexts`:

```json
{
  "strings": {
    "Save": "Сохранить",
    "@count view": ["@count просмотр", "@count просмотра", "@count просмотров"]
  },
  "contexts": {
    "Long month name": {"May": "май"}
  }
}
```

## Rules

- The key is the English string exactly as the code has it.
- Keep every placeholder of the English string: `@name`, `%name`,
  `:url` (`@count` of a plural form may be spelled out).
- No HTML but the English string's own tags, exactly as they are.
- A string the code no longer has is left out of the catalog (the build
  says how many); remove it when you see it.

## Checking and building

The CI checks every pull request: each string against the code's
strings, Russian complete. It builds the catalog from `main` and
publishes it. Locally, with the engine's binary:

```bash
app translations index -src . -out dist -complete ru
```

or, without a binary, `go run github.com/kosatka-cms/kosatka/cmd/app@main translations index -src . -out dist -complete ru`.

## Adding a language

1. Create `<code>/language.yaml` (`version: 0.1.0`, `draft: true` until
   someone reviews it).
2. Translate the components into `<code>/*.json` - the build lists the
   strings still missing.
3. Open a pull request.

## License

MIT, as the engine.
