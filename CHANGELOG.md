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

## [3.1.2] - 2024.03.12

### Fixed

- Fixed bug where matching route was not found if request path was empty

## [3.1.1] - 2023.08.10

### Added

- Updated `resolve` method to ensure `status` is always being returned.

### Fixed

- Fixed bug where `status` was not being updated to `404` when route was `fallback` type in `resolve` method. 

## [3.1.0] - 2023.08.04

### Added

- Added `getResolvedParameters` method

### Changed

- Updated `getNamedRoutes` and `getNamedRoute` methods to replace wildcards with parameters.

## [3.0.2] - 2023.06.23

### Fixed

- Fixed bug in `_getMatchingRoute` method when `$request_path` is not of type `string`.

## [3.0.1] - 2023.05.30

### Fixed

- Fixed bug in `_getMatchingRoute` method when request array does not contain expected values

## [3.0.0] - 2023.01.27

### Added

- Added support for PHP 8

### Removed

- Removed support for PHP 7

## [2.1.1] - 2023.01.23

### Changed

- Updated default status code to `302` for `addRedirect` method
- Updated documentation
- Internal code cleanup

## [2.1.0] - 2023.01.17

### Added

- Added `resolve` method

### Updated

- Updated vendor dependencies.

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