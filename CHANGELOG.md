# Changelog

All notable changes to Action Archive for BigFix Platform are documented here.
The format follows [Keep a Changelog](https://keepachangelog.com/).

For full release notes — upgrade instructions, assets, checksums — see the
[Releases page](https://github.com/mxc0bbn/action-archive-bigfix/releases).

## [v1.1.4] - 2026-05-18

### Added
- Pre-install prerequisite check: the installer verifies the tools it needs and its network access, lists every missing item and connectivity problem at once, and offers to install the missing packages before continuing.
- BigFix connection is verified during installation (reachability and authentication) before the install proceeds, with an option to re-enter the details, continue anyway, or abort.

### Changed
- Web admin and BigFix operator password prompts now show masked feedback and require confirmation.
- Admin email is optional at installation and can be set later in the web interface.

### Fixed
- Passwords containing special characters, including a single quote, are handled correctly during installation.

## [v1.1.3] - 2026-05-15

### Added
- Performance page under Administration > Logs: at-a-glance health gauges for the system, web server, database, and collection pipeline, with detail panels, a network throughput chart, and an in-place explanation of every metric.
- Application version shown on the login screen and in the header.
- Guided PostgreSQL tuning during install, sized to the server's detected memory, CPU count, and storage type. Optional and declinable; existing tuning is preserved on upgrade.

### Fixed
- Installer now places the bundled BigFix Audit Agent package, so the download card appears on a fresh install (regressed in v1.1.2).
- Installer now installs the certificate helper and its sudoers rule, so SSL certificate upload works on a fresh install (regressed in v1.1.2).
- Installer upgrade mode restored: re-running over an existing install preserves configuration, credentials, database, SSL certificate, and the admin account, and reports the installed version before you choose (present in v1.1.0, lost in v1.1.2).
- Database health score recalibrated for Action Archive's normal query patterns.

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

[v1.1.4]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.4
[v1.1.3]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.3
[v1.1.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.2
[v1.1.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.1
[v1.1.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.1.0
[v1.0.2]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.2
[v1.0.1]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.1
[v1.0.0]: https://github.com/mxc0bbn/action-archive-bigfix/releases/tag/v1.0.0
