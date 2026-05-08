# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2026-05-08

### Added

- Initial public release of the speckit plugin collection extracted from
  `phsb5321/NixOS` `dotfiles/speckit-plugins/`.
- Claude Code marketplace manifest at `.claude-plugin/marketplace.json`.
- `speckit` plugin: spec-driven development workflow commands and skill.
- Release-engineering template (chrome-bridge v0.1.0 baseline):
  - CI: JSON syntax validation, plugin/marketplace manifest schema check,
    SKILL.md frontmatter check, actionlint.
  - OSV-Scanner V2 with SARIF upload.
  - OpenSSF Scorecard with SARIF upload + branch-protection check.
  - Release pipeline: signed tarball, SLSA build provenance attestation
    (`actions/attest-build-provenance@v2`), CycloneDX 1.7 + SPDX 2.3 SBOMs,
    Sigstore-verifiable via `gh attestation verify`.
- `SECURITY.md` private vulnerability disclosure policy.
- `CODEOWNERS` pinning review to `@phsb5321`.
- All GitHub Actions pinned by full 40-char commit SHA.

[0.1.0]: https://github.com/yolo-labz/speckit-plugins/releases/tag/v0.1.0
