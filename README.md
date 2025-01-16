```markdown
# API Documentation for Spotify Lyrics Extractor

## Overview
The `Spotify_LyricsExtractor_API` class provides functionality to extract lyrics for a song using the Spotify API without the need for complex API management or handling rate limits. It utilizes a method of reverse engineering to achieve this.

## Class: `Spotify_LyricsExtractor_API`

### Constructor: `__init__()`
Initializes the `Spotify_LyricsExtractor_API` class, loading environment variables for Spotify credentials and setting up the Spotify client.

#### Parameters:
- None

#### Returns:
- None

### Method: `getAccessToken(file_path="spotify_token_cache.json")`
Retrieves the access token from a specified JSON file.

#### Parameters:
- `file_path` (str): The path to the JSON file containing the access token. Default is `"spotify_token_cache.json"`.

#### Returns:
- `str`: The access token extracted from the JSON file.

### Method: `getSongsHeaders(song_name, artist=None, movies_name=None, year=None)`
Searches for a track on Spotify and retrieves the track link and image link.

#### Parameters:
- `song_name` (str): The name of the song to search for.
- `artist` (str, optional): The name of the artist. Default is `None`.
- `movies_name` (str, optional): The name of the movie (not used in the current implementation). Default is `None`.
- `year` (int, optional): The year of release (not used in the current implementation). Default is `None`.

#### Returns:
- `tuple`: A tuple containing:
  - `str`: The link to the requested track.
  - `str`: The image link associated with the track.

### Method: `refresh_Token(required=False)`
Refreshes the access token if it has expired or if required.

#### Parameters:
- `required` (bool): Indicates if a token refresh is necessary. Default is `False`.

#### Returns:
- None

### Method: `lyrics_Extractor(track_link, image_link="")`
Extracts lyrics for a given track using its link.

#### Parameters:
- `track_link` (str): The link to the track for which lyrics are to be extracted.
- `image_link` (str, optional): The image link associated with the track. Default is an empty string.

#### Returns:
- `tuple`: A tuple containing:
  - `str`: The JSON response containing the lyrics if successful.
  - `int`: The HTTP status code of the response.

### Method: `praise_Lyrics(lrc_response)`
Parses the lyrics from the JSON response.

#### Parameters:
- `lrc_response` (str): The JSON response containing the lyrics.

#### Returns:
- `str`: The extracted lyrics as a single string.

## Example Usage
```python
if __name__ == '__main__':
    ob = Spotify_LyricsExtractor_API()

    # Get song headers
    req_link, image_link = ob.getSongsHeaders("Angel de Hielo", "Anabantha")
    print(req_link, image_link)

    # Extract lyrics
    lyrics_response = ob.lyrics_Extractor(req_link, image_link)

    if lyrics_response is not None:
        lyrics = ob.praise_Lyrics(lrc_response=lyrics_response)
        print(lyrics)
    else:
        print("Due to some error, lyrics are not found.")
```

## Error Handling
- **404**: Lyrics not found for the requested track.
- **429**: Too many requests; the program will wait for 120 seconds before retrying.
- **401**: Token expired; the program will attempt to refresh the token.
- Other status codes will return a generic error message.

## Dependencies
- `spotipy`: For interacting with the Spotify API.
- `dotenv`: For loading environment variables.
- `requests`: For making HTTP requests.
- `re`, `json`, `os`: Standard libraries for regular expressions, JSON handling, and operating system interactions.
- `tqdm`: For progress bars in the console.

## Environment Variables
- `SPOTIFY_CLIENTID`: Your Spotify API client ID.
- `SPOTIFY_CLIENTSECRET`: Your Spotify API client secret.
```
