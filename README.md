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

The latest published ChaptarrNG release remains v0.9.977, which accepts
migration-158 SQLite only. ChaptarrNG PR #131 head 2c0401fc passed exact-head
run [39196795646](https://github.com/snapetech-org/chaptarrng/actions/runs/39196795646):
Linux, macOS, and Windows build/tests, the PostgreSQL migration fixtures,
release-note validation, CodeQL, dependency and secret checks, NuGet
submission, and runner-receipt reconciliation passed. Disposable-fork
validation was skipped by its gate. Four Windows fixtures failed in the
previous run because they used POSIX-only paths; the current native-path
fixtures pass.

On 2026-10-19, candidate build 0.9.978.0 passed preview, apply, restore
preview, and rollback on a disposable copy of populated migration-158 SQLite
data with native Hardcover mode and network access disabled. The source copy
remained unchanged, rollback restored the retained source backup byte-for-byte,
and the converted database remained separate. This is copy-only evidence, not
production approval. Ambiguous media review, older-schema and PostgreSQL
operator-data acceptance, feature parity, installed rollback, and
installation-channel acceptance remain open. PR #131 remains draft and the
Unraid Community Apps transition has not been tested through the Apps upgrade
flow; keep PR #5 in draft and do not switch existing installs yet.

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
