<h2 align="center">
  <a href=#><img src="https://raw.githubusercontent.com/armbian/.github/master/profile/logosmall.png" alt="Armbian logo"></a>
  <br><br>
</h2>

# armbian.github.io

## Purpose of This Repository

This repository is Armbian's **automation and orchestration hub**. It hosts the scheduled GitHub Actions workflows, generator scripts and release-target configuration that produce the metadata, indexes and image lists consumed by [armbian.com](https://www.armbian.com), [docs.armbian.com](https://docs.armbian.com) and the download infrastructure.

Generated artifacts are published to the `data` branch and exposed via [github.armbian.com](https://github.armbian.com/).

## Workflow Status & Monitoring

The full workflow catalogue, live status, run history and per-job metrics for this repository are tracked on the Armbian actions dashboard:

**➡️ [actions.armbian.com](https://actions.armbian.com/?repo=armbian.github.io)**

Rather than duplicating the workflow list here, please refer to that page for:

- Current state of scheduled and dispatched pipelines
- Execution history with timestamps and outcomes
- Runtime, resource usage and success/failure rates
- Detailed logs and error traces for failed runs

## Repository Layout

| Path | Contents |
|---|---|
| `scripts/` | Generator and helper scripts (Python, Shell, Node.js) invoked from workflows |
| `release-targets/` | Inputs and configuration for the image build-target generator; see [`release-targets/README.md`](release-targets/README.md) |
| `templates/` | Templates used by the generator scripts |
| `board-images/` | Per-board product photos (`<board>.png`) |
| `board-vendor-logos/` | Per-vendor logo files |
| `.github/workflows/` | GitHub Actions workflow definitions (YAML) |
| `CNAME` | Custom domain configuration |

## What This Repository Produces

Most workflows write their output to the `data` branch (served at [github.armbian.com](https://github.armbian.com/)) and are consumed by other Armbian services. Notable generated artifacts include:

- **`data/image-info.json`** — inventory of all boards known to `armbian/build`, refreshed from the build framework.
- **`data/armbian-images.json`** — download index for the Armbian image catalogue, enriched with partner/vendor data.
- **`data/release-targets/`** — the `targets-release-*.yaml` build matrices, `exposed.map` and `kernel-description.json` used to drive Armbian's CI/CD pipelines.
- **`data/base-files.json`** — parsed Debian/Ubuntu `base-files` package index.
- **`data/keyrings/`** — latest Debian and Ubuntu keyring `.deb` packages with per-variant symlinks.
- **`data/actions-report/`** — machine-readable status of workflows across Armbian repositories.
- **`data/servers/`** — cached NetBox inventory of mirrors, cache hosts, upload targets and self-hosted runners, plus the current torrent tracker announce list.
- **`data/partners.json`** and maintainer data — synced from Zoho Bigin and enriched with GitHub profile data.
- **`data/rpi-imager.json`** — image catalogue for the Raspberry Pi Imager.
- **`data/jira-current.html`**, **`data/jira-next.html`** — release-milestone excerpts from Armbian's Jira.
- **`data/quotes.txt`** — content used by the Armbian MOTD.
- **Thumbnails** — generated from `board-images/` and `board-vendor-logos/` and uploaded to the Armbian image cache.

## Release-Target Generator

The most substantial piece of local logic in this repo is the release-target generator in `scripts/generate_targets.py`, configured from `release-targets/`. It reads `image-info.json` and produces the YAML files that drive image builds for the standard-support, nightly, community-maintained and apps scopes, plus the `exposed.map` regex table used by the website to pick recommended images per board.

Full documentation, including input/output formats, board classification, extension maps and release-codename substitution, lives in [`release-targets/README.md`](release-targets/README.md).

## Built With

- **Python 3** — generator scripts (`scripts/*.py`), including `generate_targets.py`, `generate_kernel_descriptions.py`, `generate-rpi-imager-json.py`, `generate-base-files-info-json.py` and `days_since_last_commit.py`.
- **Shell / Bash** — helper scripts (`scripts/generate-armbian-images-json.sh`) and workflow `run:` steps; heavy use of `jq`, `curl`, `rsync`, `git`.
- **Node.js** — `scripts/generate-actions-report.mjs` (uses `fast-glob`, `js-yaml`).
- **GitHub Actions** — YAML workflows in `.github/workflows/` for scheduling and orchestration.
- **GraphicsMagick** and **pngquant** — image thumbnail generation.
- **External APIs** — GitHub, Zoho Bigin, Atlassian Jira, NetBox, `rsync://fi.mirror.armbian.de`.

## Branches

- **`main`** — source of workflows, scripts, `release-targets/` configuration and image assets.
- **`data`** — generated artifacts consumed by downstream services; written to by scheduled workflows, not intended for manual edits.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to report bugs, propose changes and get involved in Armbian more broadly. All contributors are expected to follow the [Code of Conduct](CODE_OF_CONDUCT.md).

Additional ways to help:

- [Become a board maintainer](https://docs.armbian.com/Board_Maintainers_Procedures_and_Guidelines/)
- [Apply for a staff position](https://forum.armbian.com/staffapplications/)
- [Support the project](https://forum.armbian.com/subscriptions/)
- [Help on the forum](https://forum.armbian.com/)

## License

Released under the [GNU General Public License v2.0](LICENSE).
