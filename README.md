# Searcharr
[![Docker Pulls](https://img.shields.io/docker/pulls/toddrob/searcharr?style=plastic)](https://hub.docker.com/r/toddrob/searcharr) [![release](https://github.com/toddrob99/searcharr/actions/workflows/release.yml/badge.svg)](https://github.com/toddrob99/searcharr/actions/workflows/release.yml) [![beta](https://github.com/toddrob99/searcharr/actions/workflows/beta.yml/badge.svg?branch=beta)](https://github.com/toddrob99/searcharr/actions/workflows/beta.yml)
### Sonarr & Radarr Telegram Bot
### By Todd Roberts
https://github.com/toddrob99/searcharr

This bot allows users to add movies to Radarr, series to Sonarr, and books to Readarr via Telegram messaging app.

## Setup & Run

### Configure

Rename `settings-sample.py` to `settings.py`, and edit the settings within the file as necessary.

Detailed descriptions of the available settings are available on the [wiki](https://github.com/toddrob99/searcharr/wiki/Configuration-::-settings.py).

You are required to update the following settings, at minimum:

* Searcharr Bot > Password
* Telegram Bot > Token (see [Telegram Bot Setup Instructions](https://core.telegram.org/bots#6-botfather))
* Sonarr > URL, API Key, Quality Profile ID
* Radarr > URL, API Key, Quality Profile ID
* Readarr > URL, API Key, Quality Profile ID, Metadata Profile ID
* Listenarr (2nd Readarr for audiobooks) > URL, API Key, Quality Profile ID, Metadata Profile ID

### Docker & Docker-Compose

Docker is the suggested method to run Searcharr. Be sure to map the following in your Docker container:

* Settings file to /app/settings.py
* Database folder to /app/data
* Log folder to /app/logs

A docker-compose.yml file is provided for your convenience. Update the volume mappings listed above, and then run `docker-compose up -d` to start Searcharr.

### Run from Source

If running from source, use Python 3.8.3+, install requirements using `python -m pip install -r requirements.txt`, and then run `searcharr.py`.

## Use

### Authenticate

Send a private message to your bot saying `/start <password>` where `<password>` is the value of `searcharr_password` in `settings.py`. For admin access, instead say `/start <admin_password>` where `<admin_password>` is the value of `searcharr_admin_password` in `settings.py`. An authenticated user can add admin access by re-authenticating using the admin password. Re-authenticating with the non-admin password will not remove a user's admin access. Admin access must be removed by an admin using the `/users` command, or manually in the database.

**Caution**: Authentication via `/start` command will work in a group chat, but then everyone else in the group will see the password. If not all group members should be allowed to use the bot, then be sure to authenticate in a private message.

**Double Caution**: Do not authenticate as an admin in a group chat. Always use a private message with your bot.

### Search & Add a Series to Sonarr, a Movie to Radarr, a Book to Readarr, or an Audiobook to Listenarr (2nd Readarr install)

Send the bot a (private or group) message saying `/series <title>`, `/movie <title>`, or `/book <title>` (replace with custom command aliases, as configured in `settings.py`). The bot will reply with information about the first result, along with buttons to move forward and back within the search results, pop out to tvdb, TMDB, or IMDb, or Goodreads for books, add the current series/movie/book to Sonarr/Radarr/Readarr, or cancel the search. When you click the button to add the series/movie/book/audiobook to Sonarr/Radarr/Readarr/Listenarr, the bot will ask what root folder to put the series/movie/book in, then what quality profile to use--unless you have only one root folder or quality profile enabled in Searcharr settings, in which case it will skip those steps and add the series/movie straight away.

### Manage Users

If you are authenticated as an admin, you can use the `/users` command to retrieve a list of users with buttons to remove all access and add/remove admin access (as applicable).

## Screenshots

Authenticate by saying `/start <password>` (or `/start@bot_username <password>` in a group with multiple bots)

![Authenticate](https://github.com/toddrob99/searcharr/blob/main/screenshots/authenticate.png?raw=true)

Search for movie using `/movie <title>` or series using `/series <title>` (behavior is the same for series and movies) (use custom commands configured in `settings.py` if applicable). Buttons will appear to open the series/movie info in tvdb, IMDb, or TVDB when those ids are available.

![Search Result](https://github.com/toddrob99/searcharr/blob/main/screenshots/add.png?raw=true)

If series/movie already exists in Sonarr/Radarr, the Add button will instead say "Already Added!":

![Already Exists](https://github.com/toddrob99/searcharr/blob/main/screenshots/already-exists.png?raw=true)

If Searcharr has multiple root folders configured, you will be prompted to select a root folder:

![Choose Root Folder](https://github.com/toddrob99/searcharr/blob/main/screenshots/choose-root-folder.png?raw=true)

If Searcharr has multiple quality profiles configured, you will be prompted to select a root folder after selecting a quality profile:

![Choose Quality Profile](https://github.com/toddrob99/searcharr/blob/main/screenshots/choose-quality-profile.png?raw=true)

When the series/movie has been added, or you click Cancel, the search results will be removed:

![Added](https://github.com/toddrob99/searcharr/blob/main/screenshots/added.png?raw=true)
