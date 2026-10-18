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
progress. The updated BookshelfNG template will install this same standard
ChaptarrNG image used by fresh installs; there is no compatibility image or
second application build. The template change is not yet published or tested
through the Unraid Apps upgrade flow, so keep the current installation and its
backup until that release gate passes. The
[migration guide](https://github.com/snapetech-org/chaptarrng/blob/main/docs/MIGRATE_FROM_BOOKSHELFNG.md)
documents the published `v0.9.977` migration-158 SQLite support and recovery
steps. The `.977` converter bundles an embedded SQLite migration chain, but
its pre-upgrade guard currently rejects source schemas earlier than migration
14 because they still use `Blacklist`. A version-aware fix is under hosted
validation for a follow-up release. An isolated Docker image-switch smoke using
the `.71` BookshelfNG image and ChaptarrNG `.977` converted and rolled back an
empty v158 database with the same appdata and media/download mounts; the old
image started again on port 8787. This does not validate the Community Apps
update flow or populated operator data. New installations should use this
ChaptarrNG template; existing BookshelfNG installs should be upgraded to the
final `0.4.21.71` release before migration. The old-template upgrade and
rollback flow through Unraid Community Apps remains untested.

## Source and support

- [ChaptarrNG application source](https://github.com/snapetech-org/chaptarrng)
- [Unraid package support and SeerrNG integration](https://github.com/snapetech-org/seerrng/issues)

This template repository is distributed under GPLv3; see [LICENSE](LICENSE).
