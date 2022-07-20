# Lesson 8.8 Continue with Class Example - Clustering and Data Pipeline

Rather than a lesson, this should be a time where students work on the project on their own and the teaching team supports them by solving whatever issues pop up.

Students should be focusing in 2 big areas:

### 1. Cluster the songs you collected
  1.1. scale the audio features of your songs. this should create an object called `scaler` (store it, you’re gonna need it in the future) and an array with scaled features, let’s call it `X_scaled`
  1.2. initialize a KMeans model with `kmeans = KMeans(random_state=1234)` (don’t waste time on parameters /number of clusters for now - use defaults!)
  1.3. fit the model to your data using `kmeans.fit(X_scaled)`
  1.4. create a column called cluster in your original dataframe, with the assigned cluster, using `X["cluster"] = kmeans.predict(input_song)`
  1.5. this process should only be done once, not every time a song is inputed! However, you are going to need the clustered dataframe `X` , the `scaler`, and the `kmeans` model to be loaded in your environment (i.e. notebook) when the user inputs a song. Tip: consider doing this through creating a module and loading it from another notebook.


### 2. Assemble the project pipeline
When the user inputs a song, you should be able to:
  2.1. receive an input song from a user. let’s imagine it’s *Bohemian Rhapsody*
  2.2. check if *Bohemian Rhapsody* is hot (by looking at the songs you scraped from Billboard). It’s not, so…
  2.3. send “Bohemian Rhapsody” to the Spotify API and get its audio features. Store them in a variable called, for example, `song_audio_features`
  2.4. scale the audio features using `song_scaled = scaler.transform(song_audio_features)`. Notice how here we don't fit the scaler: we use the `scaler` we created above and use its `transform` method.
  2.5. get the cluster of the song, using `kmeans.predict(song_scaled)` (this is the `kmeans` model we created above!). Let’s imagine it’s cluster 3.
  2.6. from your dataframe of collected songs `X` , get a random song that belongs to cluster 3. Let’s imagine it’s *Stairway to Heaven*.
  2.7. print *Stairway to Heaven*: this is your recommendation!


