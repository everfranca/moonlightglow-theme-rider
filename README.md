# MoonlightGlow for Rider

UI theme + editor color scheme for **JetBrains Rider 2026.2**, inspired by the **Moonlight Glow** theme of Visual Studio 2026.

* Navy chrome `#0F2B48`, darker toolbars `#0A1E32`, accent `#89B6E6`
* Editor `#1E1E1E` with C# colors matching VS Dark (keyword `#569CD6`, string `#CE9178`, comment `#6A9955`, types `#4EC9B0`, methods `#DCDCAA`, locals `#9CDCFE`)
* Designed for the Cascadia Code font

## Install

**JetBrains Marketplace** (once published): `Settings > Plugins > Marketplace` → search `MoonlightGlow` → Install → Restart.

**Manual (.jar):** `Settings > Plugins > ⚙ > Install Plugin from Disk…` → select `MoonlightGlow-theme-plugin.jar` → Restart. Then `Settings > Appearance > Theme = MoonlightGlow` and `Settings > Editor > Color Scheme = MoonlightGlow`.

## Build the plugin .jar from source

```bash
cd src/main/resources
zip -r MoonlightGlow-theme-plugin.jar META-INF MoonlightGlow.theme.json colorSchemes
```

## Compatibility

Rider `2026.2` (builds `262.*`), New UI. Plugin id: `com.everfranca.moonlightglow`.

## Version history

* `1.0.4` — darker toolbars `#0A1E32`
* `1.0.3` — scheme unified with user-tuned colors
* `1.0.2` — navy toolbar/solution headers
* `1.0.1` — removed pure-black gaps
* `1.0.0` — initial port from VS 2026 Moonlight Glow

## License

MIT — see [LICENSE](LICENSE).
