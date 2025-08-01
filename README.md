<div align="center">
  <div style="display: flex; align-items: center; justify-content: center; gap: 10px; margin-bottom: 10px;">
    <img src="https://upload.wikimedia.org/wikipedia/commons/8/84/Spotify_icon.svg" width="30" height="30" style="vertical-align:middle; margin-right:8px;">
    <picture style="vertical-align:middle;">
      <source srcset="https://mintlify.s3.us-west-1.amazonaws.com/mcp/logo/dark.svg" media="(prefers-color-scheme: dark)">
      <img src="https://mintlify.s3.us-west-1.amazonaws.com/mcp/logo/light.svg" width="auto" height="30" alt="MCP Logo">
    </picture>
</div>
<h1>Spotify MCP Node Server</h1>
</div>

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io) Node server that enables AI assistants like [Claude Desktop](https://claude.ai/download), or IDEs like [Cursor](https://cursor.sh) and [Windsurf](https://windsurf.io) to control Spotify playback and manage playlists. Great for music discovery and creative playlist curation. Try asking Claude for some less-known tracks in a genre or similar to an artist. You can start by asking to create a new playlist or update an existing playlist.

For an easiest quickstart, as of May 2025 the [Claude Desktop](https://claude.ai/download) is the recommended way to use this software. Install Claude Desktop for your platform and follow the [integration guide](#integrating-with-ai-assistants) below.

To comply with Spotify’s Developer Terms, you must have a Spotify Premium account to use this server. Additionally, if you’re using MCP-enabled AI assistants (such as Claude) with this server, you must opt out of data sharing for model training.

<details>
<summary>Contents</summary>

- [Example Interactions](#example-interactions)
- [Tools](#tools)
  - [Read Operations](#read-operations)
  - [Play / Create Operations](#play--create-operations)
- [Setup](#setup)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Creating a Spotify Developer Application](#creating-a-spotify-developer-application)
  - [Spotify API Configuration](#spotify-api-configuration)
  - [Authentication Process](#authentication-process)
- [Integrating with AI assistants](#integrating-with-ai-assistants)
</details>

## Example Interactions

- _"Play The Beatles less known bootlegs"_
- _"Make a fusion playlist of The Beatles and Metallica"_
- _"What are the audio features of the track 'Bohemian Rhapsody' by Queen?"_
- _"Create my marathon playlist and add tracks from my workout playlists"_

## Tools

<details>
<summary>Read Operations</summary>

1. **searchSpotify**

   - **Description**: Search for tracks, albums, artists, or playlists on Spotify
   - **Parameters**:
     - `query` (string): The search term
     - `type` (string): Type of item to search for (track, album, artist, playlist)
     - `limit` (number, optional): Maximum number of results to return (10-50)
   - **Returns**: List of matching items with their IDs, names, and additional details
   - **Example**: `searchSpotify("bohemian rhapsody", "track", 20)`

2. **getNowPlaying**

   - **Description**: Get information about the currently playing track on Spotify
   - **Parameters**: None
   - **Returns**: Object containing track name, artist, album, playback progress, duration, and playback state
   - **Example**: `getNowPlaying()`

3. **getUserPlaylists**

   - **Description**: Get a list of the current user's playlists on Spotify
   - **Parameters**:
     - `limit` (number, optional): Maximum number of playlists to return (default: 20)
     - `offset` (number, optional): Index of the first playlist to return (default: 0)
   - **Returns**: Array of playlists with their IDs, names, track counts, and public status
   - **Example**: `getUserPlaylists(10, 0)`

4. **getPlaylistTracks**

   - **Description**: Get a list of tracks in a specific Spotify playlist
   - **Parameters**:
     - `playlistId` (string): The Spotify ID of the playlist
     - `limit` (number, optional): Maximum number of tracks to return (default: 100)
     - `offset` (number, optional): Index of the first track to return (default: 0)
   - **Returns**: Array of tracks with their IDs, names, artists, album, duration, and added date
   - **Example**: `getPlaylistTracks("37i9dQZEVXcJZyENOWUFo7")`

5. **getRecentlyPlayed**

   - **Description**: Retrieves a list of recently played tracks from Spotify.
   - **Parameters**:
     - `limit` (number, optional): A number specifying the maximum number of tracks to return.
   - **Returns**: If tracks are found it returns a formatted list of recently played tracks else a message stating: "You don't have any recently played tracks on Spotify".
   - **Example**: `getRecentlyPlayed({ limit: 10 })`

6. **getRecentlyPlayed**

   - **Description**: Retrieves a list of recently played tracks from Spotify.
   - **Parameters**:
     - `limit` (number, optional): A number specifying the maximum number of tracks to return.
   - **Returns**: If tracks are found it returns a formatted list of recently played tracks else a message stating: "You don't have any recently played tracks on Spotify".
   - **Example**: `getRecentlyPlayed({ limit: 10 })`

7. **getFollowedArtists**

   - **Description**: Retrieves a list of artists the user is following on Spotify.
   - **Parameters**:
     - `after` (string, optional): The last artist ID from the previous request. Cursor for pagination.
     - `limit` (number, optional): Maximum number of artists to return (1-50).
   - **Returns**: If artists are found it returns a formatted list of followed artists else a message stating: "You don't follow any artists on Spotify".
   - **Example**: `getFollowedArtists({ limit: 10 })`

8. **getUserTopItems**

   - **Description**: Retrieves a list of the user's top artists or tracks.
   - **Parameters**:
     - `type` (string): The type of items to get top for. Must be "artists" or "tracks".
     - `time_range` (string): The time range for the top items. Must be "short_term", "medium_term", or "long_term".
     - `limit` (number, optional): Maximum number of items to return (1-50).
     - `offset` (number, optional): Index of the first item to return. Defaults to 0.
   - **Returns**: If items are found it returns a formatted list of top items else a message stating: "You don't have any top items on Spotify".
   - **Example**: `getUserTopItems({ type: "artists", time_range: "short_term", limit: 10 })`



</details>

<details>
<summary>Play / Create Operations</summary>

1. **playMusic**

   - **Description**: Start playing a track, album, artist, or playlist on Spotify
   - **Parameters**:
     - `uri` (string, optional): Spotify URI of the item to play (overrides type and id)
     - `type` (string, optional): Type of item to play (track, album, artist, playlist)
     - `id` (string, optional): Spotify ID of the item to play
     - `deviceId` (string, optional): ID of the device to play on
   - **Returns**: Success status
   - **Example**: `playMusic({ uri: "spotify:track:6rqhFgbbKwnb9MLmUQDhG6" })`
   - **Alternative**: `playMusic({ type: "track", id: "6rqhFgbbKwnb9MLmUQDhG6" })`

2. **pausePlayback**

   - **Description**: Pause the currently playing track on Spotify
   - **Parameters**:
     - `deviceId` (string, optional): ID of the device to pause
   - **Returns**: Success status
   - **Example**: `pausePlayback()`

3. **skipToNext**

   - **Description**: Skip to the next track in the current playback queue
   - **Parameters**:
     - `deviceId` (string, optional): ID of the device
   - **Returns**: Success status
   - **Example**: `skipToNext()`

4. **skipToPrevious**

   - **Description**: Skip to the previous track in the current playback queue
   - **Parameters**:
     - `deviceId` (string, optional): ID of the device
   - **Returns**: Success status
   - **Example**: `skipToPrevious()`

5. **createPlaylist**

   - **Description**: Create a new playlist on Spotify
   - **Parameters**:
     - `name` (string): Name for the new playlist
     - `description` (string, optional): Description for the playlist
     - `public` (boolean, optional): Whether the playlist should be public (default: false)
   - **Returns**: Object with the new playlist's ID and URL
   - **Example**: `createPlaylist({ name: "Workout Mix", description: "Songs to get pumped up", public: false })`

6. **addTracksToPlaylist**

   - **Description**: Add tracks to an existing Spotify playlist
   - **Parameters**:
     - `playlistId` (string): ID of the playlist
     - `trackUris` (array): Array of track URIs or IDs to add
     - `position` (number, optional): Position to insert tracks
   - **Returns**: Success status and snapshot ID
   - **Example**: `addTracksToPlaylist({ playlistId: "3cEYpjA9oz9GiPac4AsH4n", trackUris: ["spotify:track:4iV5W9uYEdYUVa79Axb7Rh"] })`

7. **addToQueue**

   - **Description**: Adds a track, album, artist or playlist to the current playback queue
   - - **Parameters**:
     - `uri` (string, optional): Spotify URI of the item to add to queue (overrides type and id)
     - `type` (string, optional): Type of item to queue (track, album, artist, playlist)
     - `id` (string, optional): Spotify ID of the item to queue
     - `deviceId` (string, optional): ID of the device to queue on
   - **Returns**: Success status
   - **Example**: `addToQueue({ uri: "spotify:track:6rqhFgbbKwnb9MLmUQDhG6" })`
   - **Alternative**: `addToQueue({ type: "track", id: "6rqhFgbbKwnb9MLmUQDhG6" })`

</details>

## Setup

### Prerequisites

- Installed [Node.js](https://nodejs.org/)
- A Spotify Premium account
- A registered [Spotify Developer application](https://developer.spotify.com/dashboard/)

### MCP Server Installation

```bash
git clone https://github.com/igorgarbuz/spotify-mcp.git
cd spotify-mcp
npm install
npm run build
```

### Node.js Installation
1. Go to [Node.js download page](https://nodejs.org/en/download)
2. Download an install a recent Node.js version for your platform

### Creating a Spotify Developer Application

1. Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/)
2. Log in with your Spotify account
3. Click the "Create an App" button
4. Fill in the app name and description
5. Accept the Terms of Service and click "Create"
6. In your new app's dashboard, you'll see your **Client ID**
7. Click "Edit Settings" and add a Redirect URI (e.g., `http://127.0.0.1:8888/callback`)
8. Save your changes

### Spotify API Configuration

Create a `spotify-config.json` file in the project root:

```bash
# Copy the example config file
cp spotify-config.example.json spotify-config.json
```

Then edit the file by adding your client id only. The `accessToken`, `refreshToken` and `accessTokenExpiresAt` will be managed automatically. The `redirectUri` should be the same as the one you added in the Spotify Developer Dashboard. `127.0.0.1` is the simplest option for the local MCP server.

```json
{
  "clientId": "you-must-add-your-client-id-here",
  "redirectUri": "http://127.0.0.1:8888/callback",
  "accessToken": "your-access-token-filled-automatically",
  "refreshToken": "your-refresh-token-filled-automatically",
  "accessTokenExpiresAt": 0
}
```

### Authentication Process

The Spotify API uses OAuth 2.0 with the [PKCE extension](https://developer.spotify.com/documentation/web-api/tutorials/code-pkce-flow) for secure authentication. You do NOT need a client secret for this app.

1. Run the authentication script in the directory of the cloned repository:

```bash
npm run auth
```

2. The script will open your browser to the Spotify authorization page.

3. Log in to Spotify and authorize your application.

4. After authorization, Spotify will redirect you to your specified redirect URI. The app will automatically handle the code exchange and save your tokens.

5. The authentication script will automatically exchange this code for the access and refresh tokens.

6. These tokens will be saved to your `spotify-config.json` file.

```json
{
  "clientId": "your-client-id",
  "redirectUri": "http://127.0.0.1:8888/callback",
  "accessToken": "your-access-token-filled-automatically",
  "refreshToken": "your-refresh-token-filled-automatically",
  "accessTokenExpiresAt": 0
}
```

7. The server will automatically refresh the access token when needed, so you don't need to re-authenticate.

## Integrating with AI assistants

### Claude Desktop

The easiest way to use the Spotify MCP server is with Claude Desktop. Start [Claude Desktop installation](https://claude.ai/download) and then locate the Claude configuration file, go to `Claude Settings`,click on `Developer` and then `Edit Config`. Add the following to the configuration with an absolute path to the server:

```json
{
  "mcpServers": {
    "spotify": {
      "command": "node",
      "args": ["absolute/path/to/spotify-mcp/build/index.js"]
    }
  }
}
```

### Cursor

For Cursor, go to the MCP tab in `Cursor Settings` (command + shift + J). Add a server with this command:

```bash
node absolute/path/to/spotify-mcp/build/index.js
```

### VsCode (via Cline)

To set up your MCP correctly with Cline ensure you have the following file configuration set `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "spotify": {
      "command": "node",
      "args": ["/absolute/path/to/spotify-mcp/build/index.js"],
      "autoApprove": ["getListeningHistory", "getNowPlaying"]
    }
  }
}
```

You can add additional tools to the auto approval array to run the tools without intervention.

### Windsurf

In `Settings` then `Windsurf Settings` type `MCP` in the search bar. In the results MCP section click `add server` and then `add custom server`. Add the following configuration:

```json
{
  "mcpServers": {
    "spotify": {
      "command": "node",
      "args": ["absolute/path/to/spotify-mcp/build/index.js"],
    }
  }
}
```

You can add additional tools to the auto approval array to run the tools without intervention.

## Credits

This project was inspired by [spotify-mcp-server](https://github.com/marcelmarais/spotify-mcp-server) by Marcel Marais. Main modifications:
1. The authentication process was refactored to use the Spotify API’s PKCE extension, eliminating the need for local client secret storage and repeated re-authentication.
2. Added new tools to understand user's taste.
