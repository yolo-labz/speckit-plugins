# speckit-plugins

Spec-Driven Development (SDD) plugin bundle for [Claude Code](https://claude.com/claude-code).

> **Attribution:** This plugin is a derivative redistribution of [github/spec-kit](https://github.com/github/spec-kit) packaged as a Claude Code plugin marketplace entry. Original SDD methodology and command design © GitHub, Inc. and contributors. See [`plugins/speckit/.claude-plugin/plugin.json`](plugins/speckit/.claude-plugin/plugin.json) for upstream attribution.

## Layout

```
.claude-plugin/marketplace.json   # marketplace entry pointing at plugins/speckit
plugins/speckit/
  .claude-plugin/plugin.json      # plugin manifest (name, version, repository)
  commands/                       # /speckit slash commands
  skills/                         # spec-driven-development skill
```

## Consumption from Nix

Pin via `pkgs.fetchFromGitHub`:

```nix
fetchFromGitHub {
  owner  = "yolo-labz";
  repo   = "speckit-plugins";
  rev    = "v0.1.0";
  sha256 = "<nix-prefetch-github yolo-labz speckit-plugins --rev v0.1.0>";
}
```

Reference from `programs.claude-code.enabledPlugins` in your home-manager config.

## Versioning

Conventional Commits + semver. Tags are signed via Sigstore through the release workflow (see `.github/workflows/release.yml`).

## License

MIT — see [`LICENSE`](LICENSE). Upstream `github/spec-kit` is MIT-licensed; this redistribution preserves that license.
