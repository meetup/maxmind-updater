# maxmind updater [![](https://github.com/meetup/maxmind-updater/workflows/Main/badge.svg)](https://github.com/meetup/maxmind-updater/actions)

> updates [legacy Maxmind geoip databases](https://dev.maxmind.com/geoip/legacy/downloadable/)

> **⚠️ Note:** To use this action, you must have access to the [GitHub Actions](https://github.com/features/actions) feature. GitHub Actions are currently only available in public beta. You can [apply for the GitHub Actions beta here](https://github.com/features/actions/signup/).

## usage

```sh
$ docker build -t meetup/maxmind-updater .
```

This image is expected with code mounted to a workdir, as Github Actions does, with
two environment varibles: `DATA_DIR` the directory to locate `GeoIPCity.dat`, and `CONF_DIR` to location `GeoIp.conf`. These both default to `data` and `conf` respectively.


```sh
$ docker run --rm \
	-v $(pwd):/code \
	-w /code \
	-e DATA_DIR:path/to/data \
	-e CONF_DIR:path/to/conf \
	meetup/maxmind-updater
👍 GeoIP Database up to date, no action needed.
```

### Github Actions

Here's how you might run this container in Github Actions on a cron schedule

```yaml
name: GeoIp Update

# https://help.github.com/en/articles/events-that-trigger-workflows#scheduled-events
on:
  schedule:
  - cron:  '*/30 * * * *'
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@master
        with:
          fetch-depth: 1
      - name: Test
        uses: docker://meetup/maxmind-updater:{docker-tag}
        env:
          DATA_DIR: path/to/data
          CONF_DIR: path/to/conf
      - name: Diff
        run: git status
```

## references

* [geoipupdate tool](https://github.com/maxmind/geoipupdate-legacy)