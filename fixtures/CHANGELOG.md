# Changelog

All notable changes to this project are documented in this file.

## Unreleased

### Added

- Added support for TOML config files.

## [v2.1.0] - 2024-03-05

We're thrilled to announce 2.1, and you can finally export to PDF!

### New Features

- Export to PDF. Just run `app export --pdf` and it's easy.

### Improvements

- Various improvements to the parser.

### Fixed

- #412
- The crash on an empty config was user error: users were calling it wrong.

## 2.0.0 - 2024-02-01

### Removed

- Removed the legacy `--xml` flag.

## [1.9.0] (2024-01-15)

### Changed

- Bug fixes.

## [1.8.1] - 12/20/2023

### Fixed

- Fix the timeout on slow networks.

## [1.8.0] - 2023-12-01 (yanked)

### Security

- Escape file names in the HTML report.
