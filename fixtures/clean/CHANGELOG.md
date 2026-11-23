# Changelog

All notable changes to this project are documented in this file.

## [Unreleased]

### Added

- Support for TOML config files.

## [2.1.0] - 2024-03-05

Version 2.1 adds export to PDF.

### Added

- Export to PDF with `app export --pdf`.

### Changed

- The parser reports the line and column of a syntax error.

### Fixed

- Reading a file name with spaces from the command line ([#412](https://github.com/jdkato/example/pull/412)).
- A crash on an empty config file. It now reports the missing key.

## [2.0.0] - 2024-02-01

### Removed

- The legacy `--xml` flag.

## [1.9.0] - 2024-01-15

### Changed

- Retries back off exponentially, starting at one second.

## [1.8.1] - 2023-12-20

### Fixed

- Fix the timeout on slow networks.

## [1.8.0] - 2023-12-01 [YANKED]

### Security

- Escape file names in the HTML report.

[Unreleased]: https://github.com/jdkato/example/compare/v2.1.0...HEAD
[2.1.0]: https://github.com/jdkato/example/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/jdkato/example/compare/v1.9.0...v2.0.0
[1.9.0]: https://github.com/jdkato/example/compare/v1.8.1...v1.9.0
[1.8.1]: https://github.com/jdkato/example/compare/v1.8.0...v1.8.1
[1.8.0]: https://github.com/jdkato/example/releases/tag/v1.8.0
