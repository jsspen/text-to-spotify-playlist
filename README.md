# LISTeners Friend

A simple, interactive program that converts input text to a Spotify playlist.

_**Why?**_ When exploring new music I've always preferred listening to albums rather than "top" tracks or algorithmically generated playlists. The problem is that building playlists manually, say from a list like [this](https://rateyourmusic.com/list/funks/the_wires_100_most_important_records_ever_made/), takes a lot of copy-paste-searching and click-n-dragging, so I built this to make the process faster and easier.

_The current version is designed for album titles as input._

---

## Getting Started

1. Clone or download a copy of this repo.
2. Install the required dependencies: `pip install python-dotenv spotipy selenium bs4`
3. Rename the included `example.env` to `.env`, change the default input file path if desired, and update with your API credentials.

## Basic Usage

1. Add a list of album titles, each on a new line, to `input.txt`
2. Run the program: `python listeners_friend.py.py`
3. Select list input source
4. Albums not found will be saved to a text file and number missing will be added to playlist description
5. A new playlist will be created!

## How It Works

### Authorization

The first time you run the program you'll be sent to a Spotify authorization page in your browser. It should be asking you if you want to allow connecting to { _whatever you named your app when getting your API credentails_ }. After this you'll be routed to your Redirect URI. Copy the <u>full</u> URL and paste it into the command prompt to finalize authorization. The OAuth token will be stored in a `.cache` file.

### Standard Text Input

- Reads a text file, parses the info, and creates the relevant Spotify-format search terms. In the current state this means finding the associated URI for each album.
- Uses Spotify OAuth and a user's credentials, safely stored in environment variables, to authenticate with the official Spotify API.
- Prompts user to enter a _name_ and an (optional) _description_ for new playlist creation.
- Iterates through the parsed input info, searches for matches in the Spotify library, and adds them to the newly created playlist.
- Currently, the Spotify API only allows individual tracks or podcast episodes to be added to playlists, not whole albums, so this limitation is circumvented here by...
  - Searching the Spotify library for the album
  - Parsing the match data to find the album URI
  - Using the album URI to fetch the list of tracks from the album
  - Parsing the tracklist data to find each song's URI
  - Adding all of the album's songs, in order, to the new playlist by URI

### Boomkat List

You have to option to use the the [Boomkat Bestseller's of the Week](https://boomkat.com/bestsellers?q[release_date]=last-week) list as your input source. If selecting this option the playlist will be built from a list of albums obtained by scraping the list using Selenium and BeautifulSoup.

## How to Get Spotify API Credentials

To get the necessary info for your `.env` file you'll first need a (free) [Spotify Developer](https://developer.spotify.com/) account.

1. After logging in and landing on the dev dashboard click _Create app_.

   <img src="imgs/createapp.JPG" width="450" alt="Screenshot of the Create app screen from the Spotify Developer website">

2. Fill out the required fields:
   - Give your app a name (i.e. _Text-to-Playlist App_) and a brief description, maybe something to remind you why you made it.
   - For the Redirect URI you can supply your own or just use https://example.org/callback. Click _Add_.
   - Check the box for _Web API_ access and save.
3. After creating the app you'll be taken to its dashboard. Click _Settings_ in the top right corner. Everything you need for your `.env` file is here on this page:

   <img src="imgs/appsettings.JPG" width="450" alt="Screenshot of the app settings screen from the Spotify Developer website">

4. Copy the _Client ID_, click _View client secret_, and if you forgot to copy your Redirect URI earlier you can also see that here. The `example.env` is prepopulated with `https://example.org/callback`.
5. You're ready to start building playlists!
