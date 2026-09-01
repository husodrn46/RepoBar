# Turkish localization proof

Captured from commit `1331639` on 2026-09-01.

## CLI behavior

Command:

```sh
REPOBAR_LANGUAGE=tr swift run --disable-sandbox repobarcli --help
```

Observed output:

```text
repobar - depoları etkinlik, sorunlar, PR'ler ve yıldızlara göre listele

Kullanım:
  repobar [repos] [--limit N] [--age DAYS] [--release] [--event] [--forks] [--archived] [--scope VAL] [--filter VAL]
          [--pinned-only] [--only-with VAL] [--owner LOGIN] [--mine] [--json] [--plain] [--sort KEY]
  repobar repo <owner/name> [--traffic] [--heatmap] [--release] [--json] [--plain]
```

The command name, flags, placeholders, and technical identifiers remain unchanged while human-readable prose is Turkish.

## Packaged app behavior

Command:

```sh
find .build/debug/RepoBar.app/Contents/Resources -maxdepth 2 -type f -name Localizable.strings -print | sort
```

Observed resources:

```text
.build/debug/RepoBar.app/Contents/Resources/en.lproj/Localizable.strings
.build/debug/RepoBar.app/Contents/Resources/tr.lproj/Localizable.strings
```

Command:

```sh
swift -e 'import Foundation; let bundle = Bundle(path: ".build/debug/RepoBar.app"); let tr = bundle?.path(forResource: "tr", ofType: "lproj").flatMap(Bundle.init(path:)); print(tr?.localizedString(forKey: "Main Menu", value: nil, table: nil) ?? "MISSING")'
```

Observed output:

```text
Ana Menü
```

## Output safety

Rendered Markdown, JSON, and dynamic table/data rows use `Swift.print`; only controlled CLI prose uses `cliPrint`. This prevents user-controlled values such as a tag named `Invalid release` from being rewritten by Turkish prefix translation.

Verification: `swift test --disable-sandbox` passed with 680 tests; SwiftFormat, SwiftLint, and `git diff --check` passed.
