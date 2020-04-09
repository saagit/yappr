# yappr
Yet Another Python Podcast Retriever

I made this for my own purposes.  It's a relatively simple Python 3
podcast retriever that works well with how I listen to podcasts on the
devices that I use.  At a high level, it:

* Downloads feeds from URLs specified either in a configuration file
  or on the command line.

* For each of the feeds, it determines from the feed
  * the title,
  * the URL of an image to display for a cover, and
  * the list of episodes.

* For each of the episodes, it determines from the feed:
  * the URL to download the episode from,
  * the Globally Unique Identifier (GUID),
  * when it was published, and
  * the title.

All existing tags in an episode are removed, and then the following
ID3v2.3 frames are added:

* The album title (`TALB`) and the artist (`TPE1`) are both set to the
  feed's title.
* The episode title (`TIT2`) is set with the following format: `YYMMDD
  EpisodeTitle` where `YYMMDD` is the year, month and date that the
  episode was published and `EpisodeTitle` was determined from the feed.
* The content type (`TCON`) is set to 'Podcast'.
* The image of the front cover (`APIC`) is set to a 300x300 PNG
  derived from the feed's cover image.
* The length (`TLEN`) is set to the length of the audio as determined
  by Mutagen.

The result is saved in the specified download directory with the path:
`FeedTitle/YYMMDD SanitizedEpisodeTitle` where `SanitizedEpisodeTitle`
is the episode title with characters that are invalid for FAT32
volumes changed into number signs ('#').

By doing all this, the podcasts are sorted in order of date published
on devices that sort based upon filenames or episode titles and all
have consistent information.

A file is kept of all the episode GUIDs that have already been saved
so they are not downloaded again.

A design goal was to only use Python modules that were available from
Debian Stable repositories.  The required Debian packages as of
2020-04-08 are:
* python3-mutagen
* python3-pil
* python3-podcastparser
