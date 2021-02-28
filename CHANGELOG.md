# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

- `Added` for new features.
- `Changed` for changes in existing functionality.
- `Deprecated` for soon-to-be removed features.
- `Removed` for now removed features.
- `Fixed` for any bug fixes.
- `Security` in case of vulnerabilities

## [2.0.2] - 2021.02.28

### Changed

- Updated vendor libraries.

## [2.0.1] - 2020.11.20

### Fixed

- Fixed bug where fallback was not being routed correctly with `ANY` request method.

## [2.0.0] - 2020.11.10

### Changed

- Changed `dispatch`, `dispatchTo` and `dispatchToFallback` methods to no longer include named routes as a parameter.
They can now accept an array of user-defined parameters instead. 

## [1.1.2] - 2020.11.06

### Changed

- Updated dependencies

## [1.1.1] - 2020.11.04

### Fixed

- Fixed bug when adding route as `ANY` method.

## [1.1.0] - 2020.09.04

### Added

- Added support for `CONNECT`, `OPTIONS` and `TRACE` request methods.

## [1.0.1] - 2020.08.22

### Changed

- Updated the way exceptions are caught

## [1.0.0] - 2020.08.22

### Added

- Initial release.