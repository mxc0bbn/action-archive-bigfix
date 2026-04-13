# Action Archive for BigFix Platform — v1.0.1

**Patch release.** Fixes a collection failure that could occur on fresh installs when BigFix returned target rows with missing computer names, and adds CSV export to the remaining log views.

---

## Changes

### Fixed
- **Collection could fail with `InFailedSqlTransaction` on some MAG component targets.** BigFix occasionally returns action-target rows without a computer `Name` attribute (notably for MAG component targets where the endpoint record is incomplete). v1.0.0 rejected those rows against a `NOT NULL` constraint and, because the per-action error handler did not roll back the SQLAlchemy session, the aborted Postgres transaction propagated to the next statement and ended the entire collection run. v1.0.1 now substitutes a stable `Unknown (<computer_id>)` placeholder at the collection layer, rolls back the session on per-action errors so collection continues, and relaxes the schema constraint as defense-in-depth.

### Added
- **CSV export on Collection, Application, Nginx, and PostgreSQL log views.** Mirrors the existing Audit Log export; respects current filters; streams a timestamped `.csv` download.

### Documentation
- Administrator Guide and Installation Guide refreshed and bumped to v1.0.1.

---

## Upgrading from v1.0.0

1. Stop the service: `sudo systemctl stop bigfix-archive`
2. Extract the new tarball over `/opt/bigfix-archive/` (the tarball includes the full compiled app, static assets, docs, and a `migrations/` folder).
3. Run the one-time SQL migration to backfill any pre-existing `NULL` computer_name rows and drop the `NOT NULL` constraint:
   ```
   sudo -u postgres psql bigfix_archive -f /opt/bigfix-archive/migrations/v1.0.1-drop-not-null-computer-name.sql
   ```
   The migration is idempotent — safe to run multiple times.
4. Start the service: `sudo systemctl start bigfix-archive`
5. Confirm version in Administration → Settings → About, or via `curl -k https://<host>/api/health`.

## Fresh installs
No extra steps. The v1.0.1 `schema.sql` already has the updated constraint; the migration is unnecessary but harmless if run.

---

## Checksums
Verify the download before deploying:

```
sha256sum action-archive-v1.0.1.tar.gz
```

Expected: `bebd6926a9dfd16207af0c830cc9b5d7103ed597fd3da3b62d8a3a331cf062df`
