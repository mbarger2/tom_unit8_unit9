# Lesson 8.5 - API Wrappers

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is to connect to the `Spotify` API so we can extract useful information for our business case and apply it properly.

---

### Learning Objectives:

- API Wrappers
- `SpotiPy` library

### Lesson 1 key concepts

> :clock10: 20 min

<details>
  <summary>Click for Description:  What's an API Wrapper? </summary>

- A Wrapper is any framework or entity that is used to mask or wrap another object. It is mainly used for two reasons:

  - It helps you convert data into a compatible format (_Formatting_). First, it helps you convert data into a compatible format.
  - It helps you simplify a complex entity or makes a program perform complicated tasks easily (_Generalizing_).

- API wrappers are language-specific packages (libraries) that encapsulates multiple API calls to make complicated functions easy to use.

</details>
<summary> Click for Code Sample: SpotiPy </summary>

In order to use the `Spotify` API (`SpotiPy`), we will have to create an account in `Spotify` and follow [these](https://developer.spotify.com/documentation/general/guides/app-settings/) steps. Once we have done it, we will start initializing the API and look at the search method for which we can introduce a "query", in this example we will try it with Lady Gaga:

<details>

```python
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials

#Initialize SpotiPy with user credentias
sp = spotipy.Spotify(auth_manager=SpotifyClientCredentials(client_id="<introduce your client id>",
                                                           client_secret="<introduce your client secret>"))

results = sp.search(q='Lady Gaga', limit=50)
results
```

Output:

```python
{'tracks': {'href': 'https://api.spotify.com/v1/search?query=Lady+Gaga&type=track&offset=0&limit=50',
  'items': [{'album': {'album_type': 'single',
     'artists': [{'external_urls': {'spotify': 'https://open.spotify.com/artist/1HY2Jd0NmPuamShAr6KMms'},
       'href': 'https://api.spotify.com/v1/artists/1HY2Jd0NmPuamShAr6KMms',
       'id': '1HY2Jd0NmPuamShAr6KMms',
       'name': 'Lady Gaga',
       'type': 'artist',
       'uri': 'spotify:artist:1HY2Jd0NmPuamShAr6KMms'},
      {'external_urls': {'spotify': 'https://open.spotify.com/artist/66CXWjxzNUsdJxJ2JdwvnR'},
       'href': 'https://api.spotify.com/v1/artists/66CXWjxzNUsdJxJ2JdwvnR',
       'id': '66CXWjxzNUsdJxJ2JdwvnR',
       'name': 'Ariana Grande',
       'type': 'artist',
       'uri': 'spotify:artist:66CXWjxzNUsdJxJ2JdwvnR'}],
     'available_markets': ['AD',
      'AE',
      .
      .
      .
```

We will have a deeper look at the output in the next lesson.

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>

- Link to [8.05 Activity 1](https://github.com/ironhack-edu/data_8.05_activities/blob/master/8.05_activity_1.md).

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- Link to [8.05 Activity 1 solution](https://gist.github.com/ironhack-edu/122dc8a08fa444498ff4fa8588c96035).

</details>

### Lesson 2 key concepts

> :clock10: 20 min

We will try to understand the output of the previous lesson in this one.

<details>
<summary> Click for Code Sample: Understand Previous Output</summary>

<details>

```python
results.keys() # We can see that we only have tracks
results["tracks"].keys() # Let's check the values
results["tracks"]["href"] # Query we have searched
results["tracks"]["items"] #items (actual tracks)
results["tracks"]["limit"]#Limit we have chosen
results["tracks"]["next"] #link to the next page (next 50 tracks)
results["tracks"]["offset"] # Actual offset (starting point)
results["tracks"]["previous"] #Previous search
results["tracks"]["total"] # Number of matches
```

</details>

<details>
<summary> Click for Code Sample: Exploring the tracks </summary>

```python
len(results["tracks"]["items"]) # 50 Tracks (as limited, it is the maximum)
results["tracks"]["items"][0]# Explore the first song
results["tracks"]["items"][0].keys() # We will focus on album, artists, id, name, popularity, type and uri
```

Check Albums:

```python
results["tracks"]["items"][0]["album"] # we have more info about the album
results["tracks"]["items"][0]["album"].keys() # Will check artists, id, name, release date, total tracks
results["tracks"]["items"][0]["album"]["artists"] # List with artists and information
results["tracks"]["items"][0]["album"]["id"] # Album ID
results["tracks"]["items"][0]["album"]["name"] # Album name (if its a single u'll get the name of the song)
results["tracks"]["items"][0]["album"]["release_date"] #date in YYYY-MM-DD format
results["tracks"]["items"][0]["album"]["total_tracks"] #songs in the album
```

Other

```python
results["tracks"]["items"][0]["artists"] # Track artists
results["tracks"]["items"][0]["id"] # Track ID
results["tracks"]["items"][0]["name"] # Track name
results["tracks"]["items"][0]["popularity"] # Popularity index
results["tracks"]["items"][0]["uri"] # Basically ID
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

- Link to [8.05 Activity 2](https://github.com/ironhack-edu/data_8.05_activities/blob/master/8.05_activity_2.md).

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

- Link to [8.05 Activity 2 solution](https://gist.github.com/ironhack-edu/117448c44bb2c0cd4c8a2fcf405e714d).

</details>

### Lesson 3 key concepts

> :clock10: 20 min

- Take a look at what we can do with the API

We can also look at specific playlists, users and albums. In this lesson, we will take a look at playlists.

<details>
<summary> Click for Code Sample: Playlists</summary>

<details>

```python
playlist = sp.user_playlist_tracks("spotify", "4rnleEAOdmFAbRcNCgZMpY")
```

We can ignore the parameter "user" by inputting spotify, otherwise we will get an error.

```python
playlist.keys() # Let's look at items and total:
playlist["total"] # 4778 songs!! Let's check items:
len(playlist["items"]) # It is limited to 100 tracks, we will have to fix it:
```

</details>

<details>
<summary> Click for Code Sample: Function to extract all songs IDs from a playlist </summary>

```python
def get_playlist_tracks(username, playlist_id):
    results = sp.user_playlist_tracks(username,playlist_id)
    tracks = results['items']
    while results['next']:
        results = sp.next(results)
        tracks.extend(results['items'])
    return tracks

len(get_playlist_tracks("spotify", "4rnleEAOdmFAbRcNCgZMpY"))
```

Now that we have all the tracks from a playlist lets get all the artists:

```python
def get_artists_from_playlist(playlist_id):
    tracks_from_playlist = get_playlist_tracks("spotify", playlist_id)
    return list(set(artist for subset in [get_artists_from_track(track["track"]) for track in tracks_from_playlist] for artist in subset))
```

Artists IDs

```python
def get_artists_ids_from_playlist(playlist_id):
    tracks_from_playlist = get_playlist_tracks("spotify", playlist_id)
    return list(set(artist for subset in [get_artists_ids_from_track(track["track"]) for track in tracks_from_playlist] for artist in subset))
```

```python
artists = get_artists_from_playlist("4rnleEAOdmFAbRcNCgZMpY") # Apply the function
artists_ids = get_artists_ids_from_playlist("4rnleEAOdmFAbRcNCgZMpY")
len(artists)
len(artists_ids) # We might have more ids due to artists having the same name
```

</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

- Link to [8.05 Activity 3](https://github.com/ironhack-edu/data_8.05_activities/blob/master/8.05_activity_3.md).

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

- Link to [8.05 Activity 3 solutions](https://gist.github.com/ironhack-edu/420d8b7cad7c1514a9d116a158f2d480).

</details>

### Lesson 4 key concepts

> :clock10: 20 min

- Extract info from albums

In this lesson we will work with albums to extract information. We will start by extracting all the albums of an artist.

<details>
<summary> Click for Code Sample: Get Albums</summary>

<details>

```python
def get_albums_from_artist(artist_id):
    results = sp.artist_albums(artist_id, limit = 50)
    tracks = results['items']
    while results['next']:
        results = sp.next(results)
        tracks.extend(results['items'])
    return tracks

# Same for albums ids
def get_album_ids_from_artist(artist_id):
    results = sp.artist_albums(artist_id, limit = 50)
    tracks = results['items']
    while results['next']:
        results = sp.next(results)
        tracks.extend(results['items'])
    return [i["id"] for i in tracks]
```

Example: Coldplay

```python
coldplay_id = "4gzpq5DPGxSnKTe4SA8HAU"
coldplay_albums = get_albums_from_artist(coldplay_id)
coldplay_album_ids = get_album_ids_from_artist(coldplay_id)

# Check artists that played with coldplay
set([artist["name"] for track in coldplay_albums for artist in track["artists"]])
```

Now we will see how to extract songs from albums

</details>

<details>
<summary> Click for Code Sample: Get songs from album </summary>

```python
def get_track_ids_from_albums(album_ids):
    return list(set([i["id"] for j in album_ids for i in sp.album(j)["tracks"]["items"]]))
```

Try our function with Coldplay

```python
coldplay_songs = get_track_ids_from_albums(coldplay_album_ids)

len(coldplay_songs)
```

</details>

### :pencil2: Practice on key concepts - Lab

> :clock10: 30 min

<details>
  <summary> Click for Instructions: Lab </summary>

- Link to the lab: [https://github.com/ironhack-labs/lab-api-wrappers](https://github.com/ironhack-labs/lab-api-wrappers)

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

- Link to the [lab solution](https://gist.github.com/ironhack-edu/d1381a1f53b93150e5f63ecbd62c0d44).

</details>

---

:sandwich: **LUNCH BREAK**

---
