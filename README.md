# log-archive

A small Bash CLI tool that compresses a log directory into a timestamped
`tar.gz` archive, stores it in a separate directory, and records each run in a log file.

Project page: https://roadmap.sh/projects/log-archive-tool

## Usage

    ./log-archive <log-directory>

Example:

    ./log-archive /var/log

Archives are saved to `~/log-archives/` as `logs_archive_YYYYMMDD_HHMMSS.tar.gz`,
and each run is recorded in `~/log-archives/archive.log`.

## Options

Set `LOG_ARCHIVE_DIR` to store archives somewhere else:

    LOG_ARCHIVE_DIR=/some/path ./log-archive ~/my-logs

## Reading system logs

Some files in `/var/log` need root. Pass the destination explicitly, because
`$HOME` is different under sudo:

    sudo LOG_ARCHIVE_DIR="$HOME/log-archives" ./log-archive /var/log

## How it works

1. Validates the argument and that it is a directory
2. Creates the archive directory if missing
3. Builds a timestamped archive name (and refuses to overwrite an existing one)
4. Compresses with `tar -czf`, cleaning up a partial archive if tar fails
5. Appends the date, time and archive name to `archive.log`