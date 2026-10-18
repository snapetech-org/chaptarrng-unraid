# ChaptarrNG for Unraid

This repository contains ChaptarrNG's standalone Unraid Community Applications
package: its repository profile and Docker template. The application source,
releases, and GHCR images are maintained in
[ChaptarrNG](https://github.com/snapetech-org/chaptarrng).

ChaptarrNG is Snapetech's maintained Chaptarr fork for ebook and audiobook
libraries, built on **.NET 10 LTS**. Its SeerrNG-specific changes add
format-scoped book requests and monitoring, durable tracking for imports delayed
by author metadata preparation, and request-safe retry and cancellation.
ChaptarrNG remains a standalone book manager; SeerrNG is optional.

## Submit to Community Applications

Submit this repository URL in the [Unraid Community Apps portal](https://ca.unraid.net/submit):

`https://github.com/snapetech-org/chaptarrng-unraid`

The template and repository profile are kept here so the portal scans only
Unraid package files, rather than unrelated XML files in the application
source. Run Validate and Scan in the portal before submitting.

## Manual installation

Use the [ChaptarrNG template XML](templates/chaptarrng.xml) with Unraid's
Docker template workflow. It installs `ghcr.io/snapetech-org/chaptarrng:main`,
which tracks the latest validated main image, and configures persistent
appdata, audiobook, ebook, and download paths.

- Web UI: port `8789`
- Appdata: `/mnt/user/appdata/chaptarrng` mapped to `/config`
- Audiobooks: `/mnt/user/media/audiobooks` mapped to `/audiobooks`
- eBooks: `/mnt/user/media/ebooks` mapped to `/ebooks`
- Downloads: `/mnt/user/downloads` mapped to `/downloads`
- Default file ownership: `PUID=99`, `PGID=100`

Set the mapped paths and ownership to match your Unraid shares. For app
configuration and SeerrNG integration, see the
[ChaptarrNG README](https://github.com/snapetech-org/chaptarrng#readme) and the
[SeerrNG Bookshelf backend guide](https://github.com/snapetech-org/seerrng/blob/main/docs/using-seerr/bookshelf-backend.md).

## BookshelfNG migration

Existing BookshelfNG installations have a separate legacy-template handoff in
progress. A transition candidate in the
[BookshelfNG Unraid repository](https://github.com/snapetech-org/bookshelfng-unraid/tree/codex/bookshelfng-unraid-transition-20261018)
keeps the old template identity and mounts while switching to this standard
ChaptarrNG image. Its static XML and settings-preservation check passed, but it
is not published or tested through the Unraid Apps upgrade flow. Keep the
current installation and an independent backup until those gates pass.

The latest published ChaptarrNG release is `v0.9.977`. It supports migration-
158 SQLite databases; its pre-upgrade guard rejects older source schemas. The
unreleased retirement candidate is under review in
[PR #131](https://github.com/snapetech-org/chaptarrng/pull/131) and adds an
embedded older-schema SQLite upgrade path plus one-schema PostgreSQL source
migration. Exact-head platform and PostgreSQL checks are running on
`e67f28ed`; this candidate is not available as an image. BookshelfNG's
release-freeze gate is under review in
[PR #148](https://github.com/snapetech-org/bookshelfng/pull/148). No new
release, image, package, or compatibility redirect will be published while
retirement acceptance remains open.

An isolated Docker image-switch smoke using BookshelfNG `0.4.21.71` and
ChaptarrNG `v0.9.977` converted and rolled back an empty v158 database with the
same appdata and media/download mounts; the old image started again on port
8787. This does not validate populated operator data or the Community Apps
update flow. New installations should use this ChaptarrNG template. Existing
BookshelfNG installs should stay on the final `0.4.21.71` release until an
accepted migration path and Unraid upgrade/rollback procedure are published.

## Source and support

- [ChaptarrNG application source](https://github.com/snapetech-org/chaptarrng)
- [Unraid package support and SeerrNG integration](https://github.com/snapetech-org/seerrng/issues)

This template repository is distributed under GPLv3; see [LICENSE](LICENSE).
