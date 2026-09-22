# Automated maintenance for ragflow

This public portfolio copy is maintained by [FrankieGhaf](https://github.com/FrankieGhaf). The original project is [infiniflow/ragflow](https://github.com/infiniflow/ragflow); its authors retain credit for their work. Original license and notice files and the default branch's Git history are preserved.

## Schedule and behavior

The server checks for upstream changes at **00:00, 06:00, 12:00, and 18:00 UTC** (**05:00, 11:00, 17:00, and 23:00 Asia/Karachi**) every day. It merges upstream changes, writes an explicitly automated activity entry, and pushes the default branch. A log entry is committed even when upstream has no changes. Activity entries describe automated maintenance, not manual development.

The job never force-pushes. Dirty worktrees, divergent destination branches, and merge conflicts cause that repository to be skipped with an error. A lock prevents overlapping runs, and reruns in the same six-hour window do not create duplicate log entries. Other repositories are still processed.

## Run manually

The server uses a dedicated `frankieghaf` account and one writable deploy key per repository. Credentials stay outside Git.

```sh
sudo -Hu frankieghaf /usr/bin/python3 /home/frankieghaf/bin/sync_repositories.py --repo ragflow
```

Server configuration: `/home/frankieghaf/manifest.json`. Cron: `/etc/cron.d/frankieghaf-github-sync`. Run log: `/home/frankieghaf/logs/sync.log`. Latest result: `/home/frankieghaf/state/last-run.json`. The central script is deployed by the administrator; importing upstream code does not replace the running scheduler. GitHub Actions is disabled on this portfolio copy.

To deploy elsewhere, copy the script to an administrator-managed directory, adapt `manifest.example.json` to `~/manifest.json`, create `~/repos`, clone the destination there, configure the public source as `upstream`, and configure an authenticated `origin`. Configure your Git author name/email and verified SSH host keys before installing a cron entry.
