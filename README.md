# moonshine-server

Distribution repository for the self-contained **moonshine-server** bundles
consumed by [Lemonade](https://github.com/lemonade-sdk/lemonade)'s `moonshine`
streaming speech-to-text backend.

**No moonshine code is forked or vendored here.** At build time:

- [`moonshine-voice`](https://pypi.org/project/moonshine-voice/) is installed
  from PyPI (UsefulSensors' streaming STT library),
- the thin `main.py` server wrapper is checked out from
  [`lemonade-sdk/lemonade`](https://github.com/lemonade-sdk/lemonade/tree/main/tools/moonshine-server),

and the two are frozen into a PyInstaller onedir bundle with an embedded
Python 3.12 runtime — no system Python is required (or touched) on user
machines.

## Releases

The [build workflow](.github/workflows/build-release.yml) polls PyPI daily.
When a new `moonshine-voice` version appears, it builds and publishes a
release tagged `moonshine<version>` (e.g. `moonshine0.0.62`) with assets for:

| Platform | Asset |
|----------|-------|
| Linux x64 | `moonshine-server-<tag>-linux-x64.tar.gz` |
| Windows x64 | `moonshine-server-<tag>-windows-x64.zip` |
| macOS arm64 | `moonshine-server-<tag>-macos-arm64.tar.gz` |

There is no Intel-macOS asset because `moonshine-voice` publishes no x86_64
macOS wheel.

Lemonade pins the release tag it consumes in
[`backend_versions.json`](https://github.com/lemonade-sdk/lemonade/blob/main/src/cpp/resources/backend_versions.json)
(`moonshine.cpu`). New upstream versions are built here automatically, but
Lemonade only switches when that pin is bumped via PR.

Builds can also be triggered manually from the Actions tab (with an optional
explicit `moonshine-voice` version, a `force` rebuild flag, and a lemonade ref
for the wrapper).
