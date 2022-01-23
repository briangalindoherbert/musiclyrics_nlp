## musiclyrics_nlp
automate building corpus of music lyrics, use custom classes as virtual containers for lyrics, and pass these objects to various nlp tasks and models from nltk and gensim.  The object approach allows rapid iteration and adjustment, to train and test various ideas and analyses.

1. functions to interface with lyricgenius api to GET lyrics from Internet for an artist, this builds a corpus of lyrics - each lyric file is named with the prefix of the genre (ex. 'rap', 'rock', 'country', 'pop') the artist name with spaces stripped out, and extension ".lyr", as in "pop_Maroon5.lyr".
2. In addition to building a corpus of lyrics files as described above, I've added functions to create a lyrics registry to support creating TaggedDocuments.  This creates a json file for each genre, with an dict structure for each artist built of unique tag: artist-song name key:value pairs.  This index or registry makes it easy to reference back to a particular song and its lyrics with model testing.
3. cleaning, formatting and tokenization:  I have found it is best to try to leave corpus text in close to its original state, and perform cleaning, parsing, and formatting as a 'just-in-time' process for the nlp task at hand.  Any wrangling done to text files must not harm the ability to perform any downstream nlp tasks- what is needed for word embedding may be different from what is needed for sentiment analysis, so better to do minimal scrubbing to corpus files and simply apply filters inline with nlp tasks as they are called.  In gs_Gensim.py, I have a function called line_cleaning that I call from the lyrics objects I use as generators-iterators to stream lyrics, line cleaning takes care of most of my cleaning-formatting-tokenization needs except for lemmatization, which the object calls right after line_cleaning if it is needed.  The iterator is slower due to my policy of doing text scrubbing just-in-time, for particular use cases it could be moved upstream so that training iterations run faster.
4. standardization of 'end-of'track' markers between songs and stripping of blank lines.  The method filter_for_end_of_track gets called right after get_lyrics. The standard track marker facilitates streaming by track for tasks like doc2vec.
5. genre_aggregator and Musical_Meg class definitions.  genre_aggregator organizes the supporting data for a musical genre, such as list of artists and their song registries.  Musical_Meg is the workhorse of this app- it is a virtual container for lyrics, usually instantiated for one complete genre but options exist to instantiate for a designated group of artists or for multiple complete genres of artists.  Instances of Musical_Meg can be passed to the various nlp functions and plotting methods in this app, simplifying iterations of training and testing.

Quickstart: musiclyrics_nlp has a continuously expanding set of scripts and methods for nlp tasks and models, with a particular focus on the nltk and gensim libraries.  To use this app- Set up a genius account and get your token, create lists of artists for one or more genres in gs_datadict.py, then use the corpus building script to build your corpus of lyrics.  Next, instantiate genre_aggregator then MusicalMeg objects as virtual containers for the lyrics.  Finally, train and test nlp models, passing them your MM lyrics containers.  easy-peasy!

I initially built corpora of lyrics from the official album releases of The Grateful Dead and first three full-length releases from Kendrick Lamar, I planned to do a little project contrasting language use and meaning between a legendary 60's jam band and one of the best of the new generation of rappers.  I quickly saw I needed a much larger corpus within each genre and I needed to expand my skills with paragraph  vector models.  

A couple times I refactored so much of the app that it was better to simply replace the github repo, but the design is stable now and I'm happy with my custom lyrics classes and how they simplify passing slices of the corpus to nlp tasks.  There are many more layers of the onion to peel back with applying nlp to music lyrics- I've just scratched the surface and have lots of ideas on my todo list.  I hope you enjoy music like I do and that you find something in this app that is useful or interesting.
Brian

