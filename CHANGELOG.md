# Changelog

All notable changes to this project are documented in this file.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
Versioning: [Semantic Versioning](https://semver.org/)

> No version has been tagged yet. The entries below summarize the git history up to the first release.

---

## [Unreleased]

### Added
- Backend API with API key authentication on all `/api/*` endpoints (`X-API-Key` header).
- API key management stored in the database, with CRUD endpoints and tests.
- Frontend API client sends the `X-API-Key` header.
- Import pipeline that downloads and imports the WAM NET disability welfare service CSV.
- Setup verification scripts (`verify_setup.py`, `verify_setup.sh`) and environment documentation.

### Fixed
- Alembic converts `postgres://` URLs to `postgresql://`.
- CSV import: UNIQUE constraint violations, argument name errors and WAM NET column mapping.
- CI: reusable workflow pins point at commits reachable from the qa-workflows default branch (#15).
