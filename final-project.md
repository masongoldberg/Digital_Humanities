---
layout: page
title: Final Project
permalink: /final-project/
description: Analyzing the thematic content of the Beach Boys' discography over time using Voyant Tools
---


# Final Project

## Introduction and Context

After a long and sinuous road of figuring out exactly what my project was going to be about, I settled on the idea of exploring the music of the Beach Boys over time. I knew that I wanted to do something related to music and lyrics, and after a lot of trying to figure out what was going to be possible in many modes of analysis, including geoparsing, TF-IDF, and more, I decided to use Voyant tools in addition to preparing the data using Python with APIs including Spotipy and LyricsGenius, to answer the question: How did the thematic focus and creative maturity of the Beach Boys grow over time, as reflected in their lyrics?

Let me put this question in context. The Beach Boys are one of my favorite bands, if not my favorite band of all time, which often throws people for a loop. "You mean the surf doo-dah band that my grandparents love?" my friends ask. Yes, that band. The Beach Boys have a modern day reputation of being a kitschy, nostalgiac, surfer band that people only recognize for their nasal tones. And while all of that may be true, people are also suprised to learn that they are consistently ranked among the greatest bands in musical history — not for their early surf-centric music, but for some of their later stuff. Their 1966 album "Pet Sounds" is credited as inventing the modern concept album, their instrumental arrangements gained deep complexity over time in comparison to their early blues-derived rock band music culminating in the "Brian Wilson is a genius mythos," and they continued to make music long past their mid-60s heyday of surf rock. So, I set out in this project to find some hard sort of "evidence" in the growth in their musicianship and creative lens, specifically by seeing how their lyrics changed over time. The change in their music over time is undeniable, but is that same musical change reflected in their lyrics? Did they stray away from the California Surfer aesthetic lyrically, or merely sonically? And if so, what evidence reflects that?

## Code and Data Preparation

Let me take you through the code, and how I accomplished this task. 
First, I had to import all of the libraries. Some of them ended up being used, some didn't. Note the libraries Spotipy and LyricsGenius — these are both libraries that scrape the Spotify and Genius APIs, respectively, which I used to gather the data about the music and lyrics. 


```python

import pandas as pd
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
import requests
import wget
import collections
import spacy
import lyricsgenius
```

Next, I needed to connect to the Spotify API to let me have the permission to extract their data. The Spotipy library helps me do that more straightforwardly, so I instantiate the Spotipy object as sp. 


```python
## connect to spotify

cid ='my_id'
secret ='my_secret_code'

#get into spotipy 
client_credentials_manager = SpotifyClientCredentials(client_id=cid, client_secret=secret)
sp = spotipy.Spotify(client_credentials_manager = client_credentials_manager)
```

Alright, so this next part looks kind of complicated, but is way more ugly and clunky than is befitting of a coder, but c'est la vie. There's a lot of code here that was going to be used for previous iterations of the project, that isn't necessary now, but is still there because I don't want to screw it up. But essentially, what you're looking at here is a function that takes in a Spotify URI (Unique Reference Identification ?) for a playlist, in this case a playlist containing the Beach Boys discography, and spits out a pandas dataframe of the relevant information. Note, there are some fields, such as explicit, duration, and track_number that aren't used in the final project, so you can just ignore that. I'll go into more detail in the code. 


```python

#URI for a playlist of the Beach Boys discography
bb_disco = 'spotify:playlist:3IxzEVbkZjKilFIu1faqaA'

#function that takes in a playlist's URI and spits out a pandas df of relevant info
def get_playlist_tracks(uri_info):
    #instantiating lists of relevant info
    uri = []
    track = []
    artist = []
    duration = []
    album = []
    explicit = []
    track_number = []


    #basically, this is using spotipy to retrieve items in the playlist referenced in URI info. However, for whatever reason,
    #Spotify doesn't like it when you take more than 100 songs off a playlist. So this is code to allow to take up to 500 songs off
    #a playlist, by just doing it five times with the "offset" set by 100 each time. This was relevant because the Beach Boys playlist
    #was almost 400 songs long, but this was going to be more relevant when I was working with longer playlists for my previous 
    #iteration of the project
    one = sp.playlist_items(uri_info, limit=None, offset=0, market='US')
    two = sp.playlist_items(uri_info, limit=None, offset=100, market='US')
    three = sp.playlist_items(uri_info, limit=None, offset=200, market='US')
    four = sp.playlist_items(uri_info, limit=None, offset=300, market='US') 
    five = sp.playlist_items(uri_info, limit=None, offset=400, market='US')
    
    #turning the playlist items into a dataframe
    df1 = pd.DataFrame(one)
    df2 = pd.DataFrame(two)
    df3 = pd.DataFrame(three)
    df4 = pd.DataFrame(four)
    df5 = pd.DataFrame(five)

    #appending the track information into lists for each relevant field of information
    for i, x in df1['items'].items():
        uri.append(x['track']['uri'])
        track.append(x['track']['name'])
        artist.append(x['track']['artists'][0]['name'])
        duration.append(x['track']['duration_ms'])
        explicit.append(x['track']['explicit'])
        album.append(x['track']['album']['name'])
        track_number.append(x['track']['track_number'])

    for i, x in df2['items'].items():
        uri.append(x['track']['uri'])
        track.append(x['track']['name'])
        artist.append(x['track']['artists'][0]['name'])
        duration.append(x['track']['duration_ms'])
        explicit.append(x['track']['explicit'])
        track_number.append(x['track']['track_number'])
        album.append(x['track']['album']['name'])

    for i, x in df3['items'].items():
        uri.append(x['track']['uri'])
        track.append(x['track']['name'])
        artist.append(x['track']['artists'][0]['name'])
        duration.append(x['track']['duration_ms'])
        explicit.append(x['track']['explicit'])
        track_number.append(x['track']['track_number'])
        album.append(x['track']['album']['name'])

    for i, x in df4['items'].items():
        uri.append(x['track']['uri'])
        track.append(x['track']['name'])
        artist.append(x['track']['artists'][0]['name'])
        duration.append(x['track']['duration_ms'])
        explicit.append(x['track']['explicit'])
        track_number.append(x['track']['track_number'])
        album.append(x['track']['album']['name'])

    for i, x in df5['items'].items():
        uri.append(x['track']['uri'])
        track.append(x['track']['name'])
        artist.append(x['track']['artists'][0]['name'])
        duration.append(x['track']['duration_ms'])
        explicit.append(x['track']['explicit'])
        track_number.append(x['track']['track_number'])
        album.append(x['track']['album']['name'])
    
    
    #taking all of the lists of information and putting it into one big df with all of the songs
    df = pd.DataFrame({
        'uri':uri,
        'track':track,
        'artist':artist,
        'duration_ms':duration,
        'album':album,
        'explicit':explicit,
        'track_number':track_number})
    
    return df

   

#running the function
playlist = get_playlist_tracks(bb_disco)
playlist
```

Next is the hardest part, I think. Now that I had dataframe with all of the information of songs in the Beach Boys discography, I had to link all of the lyric data to the album data. So to do this, I used the LyricsGenius library, which essentially navigates the Genius.com's API, which is a huge repository of lyrical data. What's interesting, and slightly annoying about the library is that it essentially works by just searching the search bar of Genius, which sometimes comes back with weird results (often a lot of 19th century poetry, for some reason). So to manage this, I had to put in some safeguards to make sure I was only getting the lyrics I wanted.

Going about this, I figured the best way to do this would be to sort all of the lyrics into different text files based on the albums, so they could be zipped together as a corpus for Voyant. So in the process, I just added the album lyrics to a textfile while perusing the lyrics of the entire playlist. I'll take you through the code. 


```python

#instantiating the lyricsgenius client to connect
genius = lyricsgenius.Genius("my_secret_code")

#importing time to slow down the process, so I don't overload the server
import time


#function that takes in an artist and title of a song, and spits out the lyrics of that song
def scrape_lyrics(artist, title):


    #this section of code is meant to clean up the song title of anything that might mess up the search process, so things like
    # "(Mono)" or features would make it harder for the client to search the song
    num = title.find('feat.')
    if num == -1:
        num = title.find('ft.')

    if num == -1:
        num = title.find(' - ')

    if num == -1:
        num = title.find('(Stereo)')
    
    if num == -1:
        num = title.find('(Mono)')
    

    #getting rid of any irrelevant song title info that would mess up the search
    if num != -1:
        title = title[:num]

    #actually searching the song in the genius database, using the client
    song = genius.search_song(title, artist)

    #if nothing is found, return an empty string for the lyrics
    if song == None:
        return ""

    #using the genius client to grab the lyrics from the website, using the song data previously extracted
    lyrics = genius.lyrics(song.id, remove_section_headers=True)

    #safeguards against the client picking up songs from the wrong artist, or from songs labeled as being by Spotify
    if ((song.artist == 'Spotify') or (song.artist == 'spotify') or (song.artist.lower() != artist.lower())):
        print(f"{artist} not equal to {song.artist}")
        return ""


    #getting rid of parts of the lyrics that aren't actually lyrics, but that are found in most lyric strings
    end = lyrics.find('Embed')
    beg = lyrics.find('Lyrics') + 6
    subbed_lyrics = lyrics[beg:end]

    #return cleaned lyrics
    return subbed_lyrics


```


```python
#Now that we have the function, time to use it on the playlist!

#empty string to add lyrics too
playlist_lyrics = ''

#since this is all on a playlist, there isn't a good way of knowing when the next album begins. So basically I'm just tracking the 
#previous album's title, and when it switches, I export the saved lyrics to a file and reset the string. So to start, I'm giving
#the prev_album_name variable the name of the first album in the playlist so it doesn't actually catch
prev_album_name = "Surfin' Safari (Remastered)"

#my comp 15 c++ brain using a variable to track progress of the prev_album counter 
num = 0

#iterating through the playlist
for ind in playlist.index:

    #if the previous albums name is not the same as the album name of this song, then write all of the saved playlist_lyrics
    # into a new textfile.  
    if (playlist['album'][ind] != prev_album_name):

        #this is a way of getting around the fact that the slash in 20/20 makes the machine think it's a directory.
        if(prev_album_name == "20/20 (Remastered)"):
            file = open(f"20_20.txt", 'w')
            file.write(playlist_lyrics)
            file.close()

        #write lyrics to file with the name of the album as the name of the textfile
        else:  
            file = open(f"{prev_album_name}.txt", 'w')
            file.write(playlist_lyrics)
            file.close()

        #reset playlist lyrics
        playlist_lyrics = ''

    #honestly I actually don't know why this is here but it works? might be useless actually 
    if num == 0:
        continue

    #scrape the lyrics for the given song, returns as text
    text = scrape_lyrics(playlist['artist'][ind], playlist['track'][ind])

    #waiting 1 second as to not overload the server
    time.sleep(1)

    #add the lyrics of the song to the greater playlist_lyrics variable
    playlist_lyrics = playlist_lyrics + '\n' + text
  
    #changing the previous album name variable for the next loop
    prev_album_name = playlist['album'][ind]


#code to add the text of the last album's lyrics after the loop ends. 
file = open(f"{prev_album_name}.txt", 'w')
file.write(playlist_lyrics)
file.close()
```

Awesome! Now we have all of the Beach Boys' lyrics in our directory, with each album being its own textfile. I won't lie, I had to manually add the year of the album to the textfile name afterwards, because I forgot to do it the first time around, and it was easier to spend 5 minutes doing it than redoing the code. As a note, the nature of using the Genius API made it so that some songs were probably left out of the analysis, because they couldn't be easily found. I decided to not worry about that and not include songs it couldn't find, mainly because it would be more trouble than it was worth in a big dataset, and I'd rather have missing data than incorrect data. 

Then, I unfortunately realized that I didn't lemmatize the lyrics, which was going to make Voyant difficult, so I then lemmatized the text pretty straightfowardly. It's pretty much straight from the Gibbon example. 


```python


import os

#load nlp
nlp = spacy.load("en_core_web_sm")

#taken from Gibbon example
def get_noun_and_verb_lemmas(text):
    """Return a list of noun and verb lemmas from a string"""
    doc = nlp(text)
    tokens = [token for token in doc]
    noun_and_verb_tokens = [token for token in tokens if token.pos_ == 'NOUN' or token.pos_ == 'VERB']
    noun_and_verb_lemmas = [noun_and_verb_token.lemma_ for noun_and_verb_token in noun_and_verb_tokens]
    return noun_and_verb_lemmas

#going to album path
text_path = "./albums/"

#lemmatize all of the lyrics and spit back out as csv
raw_text = ""
for file_name in os.listdir(text_path):
    try: 
        f = open(text_path + file_name, 'r')
        print(f)
        raw_text = f.read()
        lemmas = get_noun_and_verb_lemmas(raw_text)
        f.close()
        file = open(f"{file_name[:-3]}csv", 'w')

        for lemma in lemmas:
            file.write(lemma)
            file.write('\n')

    except: 
        continue

```

Now, it's actually time for Voyant!

## Voyant Analysis

First, let's answer the million-dollar question. Or at least, the least interesting question. Did the thematic focus of the Beach Boys music actually shift away from the California surfer kitsch image that they purportedly had in the beginning of their career? Looking at a graph of relative word usage across albums, it looks like it. 

![png](surfergraph2.png)

If you see above, the relative frequency of the three most cited themes (girls, cars, surfing, though this is anecdotal) of the Beach Boys that people see as detracting from their "seriousness" are certainly clustered towards the beginning of their discography, tapering off as time goes on. There's a hump of "girls" in their 1965 album, but I would argue that the "paradigm shift" of the Beach Boys is really their 1966 album, Pet Sounds. You can also see that "girls" and "cars" make a comeback in the late 70s and early 80s, well past their prime of albums that are generally regarded as some of their "best" - Pet Sounds in 1966, Smiley Smile in 1968, Sunflower in 1970, Surf's Up in 1971 — could this be indicative of a post-peak sense of nostalgia among the band for earlier days of rock-stardom? Further research could analyze how the usage of such terms differed in those different eras, but that's for another time. To answer the question of whether or not they shied away from their original themes over time, it's a yes. 



So, how can we break down the themes of the Beach Boys throughout time, using data visualization? To figure this out, I sectioned the albums into three corpuses: Early (1962-1965), Peak (1966 - 1971), and Post-Peak (1972 - 2012). Of course, these are distinctions made by me, and plenty of people would disagree. Whatever, this is my project. Anyways, below are word clouds for each era of the Beach Boys discography.


### Early Period (1962-1966)

![png](EarlyWC.png)


### Peak Period (1966-1971)

![png](PeakWC2.png)


### Post-Peak Period (1971-2012)

![png](LateWC.png)

Now, am I going to say that the most used words in each period are that distinct from one another? No, they're not, they're actually quite similar, but sometimes uninteresting results are just a part of humanities research. The quick response to this is saying that yes, maybe the lyrical themes of the Beach Boys throughout their discography was pretty stagnant, and that to really understand their musical growth over time, you need to dive more deeply into their musical growth as opposed to their lyrical growth. And while that may be true, I think there's another way of looking at it. Yes, the big words tend to be the same, like "Love" and "Know" and whatnot, but looking at the words that are smaller on the wordcloud, a different story begins to be told.

Looking at the less-frequent-but-still frequent words on the Early Period word cloud, you can see that a lot of those words are in fact thematically related to the topics discussed earlier, like surfing, girls, cars, being a teenager in the 1960s California era. Looking at the same sized words in their Peak period, you don't really see any of those water-related words, and while there are still some fairly "generic" words being used, some other more interesting words start to creep in— words like "riot," "iron," "vibration," "tear," (which I can tell you refers to crying tears, not ripping tears). While these kinds of words certainly do not encompass the whole of the word cloud, the fact that they're starting to creep in among the words points to a lyrical departure, and is reminiscent of an aesthetic change in the band, a change towards protest, maturity, and using my own historical knowledge of the band, increased drug usage. Then finally, looking at the last wordcloud of their Post-peak era, the words look a lot like the first word-cloud, but a bit blander, in my opinion. There are some more water-related words, perhaps signaling a nostalgia for the earlier days. For me, the most interesting part of this is the size of "love" and the increase of "time" over the eras— I think that this feels indicative of an aging band, especially in the wake of the death of their drummer and brother, Dennis Wilson, in the 80s— I think while the word cloud shows a sort of lack of lyrical focus, there is also a sense of focusing on the bigger picture, on things like love and time. 



So, what does this mean? I think there are a few takeaways. The first is a sort of aesthetic conclusion— while the content of their lyrics did not change over time, the true genius of their artistry lied in their music— and in fact, most of the praise of the Beach Boys over time was not necessarily about their lyrics, but about their music. Pet Sounds is renown for its storytelling, but even more so for the instrumentation of Brian Wilson's music, and the continuity in the album musically, even if the lyrics of the album themselves were not markedly different from previous albums. The legend of the Smile album had not to do with the lyrics, but with the different ways that Wilson was trying to evoke feelings of Americana in the music, and Sunflower in 1970 is today most appreciated for its musical forawrdness, in the "invention" of the shoegaze genre and the musical inventiveness of the album. And for me, the biggest takeaway is not as an academic, but as a songwriter myself. I often find myself struggling with lyrics, and this is great evidence for me to feel like I can be a good songwriter without having to have super unique lyrics, if my favorite band can get by for years talking about love and knowing. Of course, that's nothing to say about how they use those words, just that they use them a lot, but I still think the lesson is valuable— that you can be successful with non-unqiue lyrics, but really unique music, which is a lesson I really needed to hear. 

But what if I'm wrong? What if I messed up this analysis? You see, my delineations of musical periods were arbitrary, just my own intuitive categories. At the end of the day though, not all of the albums in the "Peak" category are their universally best-acknowledged albums— some of them just fit between the "greats," and were thus included. The real "great" albums, I'd say, are Pet Sounds in 1966, Smiley Smile in 1967, Sunflower in 1970 (though that's a personal favorite, and others may not agree), and Surf's Up in 1971. Some of the others like Wild Honey or 20/20 are less important in the canon of the Beach Boys. So what happens if we take a look at those albums individually, as opposed to as a period? Should the Beach Boys be evaluated as a "great period" band, or a "great album" band? 

As an example, let's take a look at Surf's Up, their 1971 album. Here's a word cloud for the most used words (lemmas) in the album:

### Surf's Up (1971)

![png](SurfsUp.png)


Now, that's a real departure from the above word clouds— this is an example of an album that is both lyrically and musically quite different from their other work. I mean, just look at these top used words— "child," "student," "die," "demonstration,"  "riot," "puff" — thematically, this album is a huge departure from their earlier albums, and you wouldn't even know it from the titles of some of the songs on that album, like "Surf's Up," "Disney Girls," "Feel Flows," etc. The themes here are really different, talking about the turmoil of the early 70s and the Vietnam War, student unrest, having a sort of pessimistic outlook on society— this is something that you wouldn't be able to tell from the larger era word clouds, but looking deeply at one album, you can see more of an individual change in theme. What does this point to about the Beach Boys as a band, on the whole? I think that there is a sense that there are bands that had peaks where they pushed out interesting and different albums one after another, in a sort of golden era of music— maybe think late 60s Beatles, or the Strokes' creative peak in their first three or so albums. For the Beach Boys, I think they hit their high peaks in waves, with one album being lyrically unique, and then the next maybe being less so. So in popular music discourse, when we talk about "eras" of bands, should we be evaluating bands in chunks, or by individual albums? 



At the end of the day, you can only really learn so much from the lyrics about the band as a whole. If I were to have more time to focus on this project, I'd want to do a sentiment analysis of certain words— how are they using the word "love" in 1965 versus 1985? I'd also want to focus more on the music itself. So much of who the Beach Boys are as a band is determined by their music, and not their lyrics, so this is only a slice of understanding about the band. And furthermore, this analysis is based on my own previous scholarship and knowledge about the Beach Boys, as well as my own biases, so this is very obviously a product of my own understandings, so I think the project as a whole certainly benefitted from that, but it also introduced a Mason-centric angle on it.

Additionally, I think that this project really points to some greater things I've learned about the Digital Humanities. One big takeaway is that you can only learn so much about something using technology in one particular angle, and that you need to have a less-technological research focus to actually gain a fuller understanding of the thing being studied. Secondly, I've learned that how you slice up the data is hugely important in what results you get— things like slicing by albums, by eras, using wordclouds, all contribute to a particular understanding of the data, but it could have easily been analyzed differently to get different results. When evaluating DH projects, it's always important that we understand where the data is coming from, and what data is being left out or not presented.
