# Changelog

All notable changes, newest first. Versions before 0.2.0 are philm5's
original releases.

## Unreleased

## v0.2.0 — 2026-10-29

First release of the maintained fork.

### Added
- **Heaters** (`DL-heater` models): climate entity, temperature sensor,
  display brightness, eco and child-lock switches. Contributed upstream by
  sayurin (philm5's PR #4, merged in November 2025 but never released).

### Fixed
- **Configure dialog failed on Home Assistant 2025.12 and later** ("config_entry
  has no setter"): the options flow no longer sets `self.config_entry`, which
  Home Assistant now provides itself.
- **Warning about a non-existing `via_device`** (philm5's issue #6): the
  climate entity's device pointed `via_device` at itself. Home Assistant
  warned on every start and stops accepting this in 2027.8. The key is gone;
  there is no hub device.

### Changed
- **Minimum Home Assistant 2024.11** (was 2023.1), the oldest version the
  options-flow fix works with.
- Manifest and HACS metadata point to this fork; codeowners `@Boisti13`,
  `@philm5`.

### Under the hood
- Upstream's `release.yml` workflow is removed: releases are created by hand
  from this CHANGELOG (it would have made a second release with its own
  notes). `validate.yml` still runs on every push.
- README, CHANGELOG and CLAUDE.md in the owner's house style; NOTICE names
  the fork.

## v0.1.1 — 2025-08-04 (philm5)

- Silent mode (davidtaylorhq, PR #3).

## v0.1.0 — 2025-07-24 (philm5)

- First release.
