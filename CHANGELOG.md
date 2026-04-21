# Changelog

All notable changes to Action Archive for BigFix Platform are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/).

For full release notes — upgrade instructions, assets, checksums — see the
[Releases page](https://github.com/mxc0bbn/action-archive-bigfix/releases).

## [v1.1.2] — 2026-04-21

### Added
- OpenAPI 3.x specification (`docs/openapi.json`) bundled with the install and published in the repository, for programmatic API discovery and client generation.
- Installer prompt for BigFix authentication method (password or API token).

### Fixed
- Installer now correctly deploys the `docs/` directory so integrity verification succeeds on first run.
- Installer banner displays the current release version.
- Admin Guide Appendix A rewritten as a pointer to the OpenAPI specification; authentication mechanisms described accurately.

## [v1.1.1] — 2026-04-16

### Added
- Targeted email notifications: event emails go to the initiating user, not all recipients.
- Console state reconciliation: each collection flags archived actions no longer present in the BigFix Console.
- Console filter dropdown on the Actions list (Any / Still in Console / Removed from Console).
- Multi-Action Group child handling: child actions cannot be individually deleted; delete the parent group instead.

### Fixed
- Completed collections now send an email notification (previously only failures did).
- Single-column Session Relevance responses parsed correctly when `<Tuple>` wrappers are absent.

## [v1.1.0] — 2026-04-16

### Added
- BigFix API token authentication as an alternative to operator username/password.
- Installer upgrade mode that preserves `.env`, database, SSL certificate, admin users, and settings on re-run.
- Bundled BigFix Audit Agent installer, downloadable from Administration → System Settings → API Keys. No internet access required post-install.

## [v1.0.2] — 2026-04-14

### Fixed
- Certificate upload: helper script and sudoers rule were missing from the tarball on fresh installs, preventing `POST /api/admin/certificate/upload` from completing.

## [v1.0.1] — 2026-04-13

### Fixed
- Collection could fail with `InFailedSqlTransaction` on target rows with missing computer names. Substitutes a stable placeholder, rolls back per-action errors, and relaxes the `NOT NULL` constraint.

### Added
- CSV export on Collection, Application, Nginx, and PostgreSQL log views.

## [v1.0.0] — 2026-04-13

Initial public release.

[v1.1.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.2
[v1.1.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.1
[v1.1.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.0
[v1.0.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.2
[v1.0.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.1
[v1.0.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.0
