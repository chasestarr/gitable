# gitable

[![Travis CI](https://img.shields.io/travis/jessfraz/gitable.svg?style=for-the-badge)](https://travis-ci.org/jessfraz/gitable)
[![GoDoc](https://img.shields.io/badge/godoc-reference-5272B4.svg?style=for-the-badge)](https://godoc.org/github.com/jessfraz/gitable)

Bot to automatically sync and update an [Airtable](https://airtable.com) sheet with 
GitHub pull request and issue data.

 * [Installation](README.md#installation)
      * [Binaries](README.md#binaries)
      * [Via Go](README.md#via-go)
      * [Running with Docker](README.md#running-with-docker)
 * [Usage](README.md#usage)
 * [Airtable Setup](README.md#airtable-setup)
      * [Using the API](README.md#using-the-api)
      * [Format](README.md#format)

## Installation


#### Binaries

For installation instructions from binaries please visit the [Releases Page](https://github.com/jessfraz/gitable/releases).

#### Via Go

```console
$ go get github.com/jessfraz/gitable
```

#### Running with Docker

```console
$ docker run --restart always -d \
    -v /etc/localtime:/etc/localtime:ro \
    --name gitable \
    -e "GITHUB_TOKEN=59f6asdfasdfasdf0" \
    -e "AIRTABLE_APIKEY=ksdfsdf7" \
    -e "AIRTABLE_BASEID=appzxcvewrwtrewt4" \
    -e "AIRTABLE_TABLE=Current Open GitHub Pull Request and Issues" \
    r.j3ss.co/gitable --interval 1m
```

## Usage

```console
$ gitable -h
gitable -  Bot to automatically sync and update an airtable sheet with GitHub pull request and issue data.

Usage: gitable <command>

Flags:

  --airtable-apikey  Airtable API Key (or env var AIRTABLE_APIKEY) (default: <none>)
  --airtable-baseid  Airtable Base ID (or env var AIRTABLE_BASEID) (default: <none>)
  --airtable-table   Airtable Table (or env var AIRTABLE_TABLE) (default: <none>)
  --autofill         autofill all pull requests and issues for a user [or orgs] to a table (defaults to current user unless --orgs is set) (default: false)
  -d, --debug        enable debug logging (default: false)
  --github-token     GitHub API token (or env var GITHUB_TOKEN)
  --interval         update interval (ex. 5ms, 10s, 1m, 3h) (default: 1m0s)
  --once             run once and exit, do not run as a daemon (default: false)
  --orgs             organizations to include (this option only applies to --autofill) (default: [])

Commands:

  version  Show the version information.
```

## Airtable Setup 

#### Using the API

[Follow this guide](http://help.grow.com/connecting-your-data-internal-and-project-management/airtable/airtable-how-to-connect).

#### Format

Your airtable table must have the following fields: 

- `reference` **(single line text)**
- `title` **(single line text)** 
- `type` **(single select)**
- `state` **(single line text)**
- `author` **(single line text)**
- `labels` **(multiple select)**
- `comments` **(number)**
- `url` **(url)**
- `updated` **(date, include time)**
- `created` **(date, include  time)**
- `completed` **(date, include time)**
- `project` **(single line text)**

The only data you need to initialize **(if not running with `--autofill`)** 
is the `Reference` which is in the format
`{owner}/{repo}#{number}`.

It should look like the following:

![airtable.png](airtable.png)
