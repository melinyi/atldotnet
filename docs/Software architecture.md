# Software architecture

[Overview](#overview)
[Focus : Data readers](#datareaders)
[Focus : Logging](#logging)

{anchor:overview}
## Overview

{anchor:datareaders}
## Focus : Data readers

Audio files, tag, playlist and cuesheet readers are all organized the same way :

* A Factory used to instanciate...
* A generic interface acting as a front for...
* Various implementations stored in the "BinaryLogic" folder

### Audio files (bird's eye view without specialized classes)
![](Software architecture_AudioBase.png)

### Playlists
![](Software architecture_Playlists.png)

### Catalog data
![](Software architecture_CatalogData.png)

### Q&A

**Q : How come the CUE sheet reader is implemented with its own set of classes under "CatalogDataReaders" ?  Why isn't it simply part of PlaylistReaders, as it also contains tracks ?**

A : That's because playlists and CUE sheets do not define tracks the same way at all :
* a playlist basically points to an existing "physical" track that the audio player can read at any time. Any additional metadata stored in the playlist file along with the audio file path is not exploited by ATL, thus IPlaylistReader only returning the list of paths to the tracks.
* a CUE sheet contains description of "virtual" tracks within a single audio file (e.g. concert recording, 2-hour long DJ mixes). In that very case, the CUE sheet is the only source of information about these tracks that do not exist individually on disk. Thus, ICatalogDataReader returns Track objects populated with metadata from the CUE sheet.

Last but not least, ICatalogDataReader is aimed at being reused in the future as an interface to read entire audio libraries (e.g. iTunes), hence its naming.


{anchor:logging}
## Focus : Logging
![](Software architecture_Logging.png)
NB : For the sake of readability, this diagram does not show methods related to synchronous / asynchronous logging

### How does it work ?
When encountering corrupted or unsupported data, ATL fires its own events in an in-memory log that can be easily read by your application (see [Usage / Code Snippets](Usage-_-Code-snippets#logging)).

The logging system implements the Observer design pattern, with an additional feature allowing asynchronous callbacks. The principle is the following :

* Any app that needs to "follow" the ATL log will have to dynamically register a custom class implementing the ILogDevice interface by calling {{Log.Register}}
* Each time ATL has something important to say, it sends a message in the Log class
* Upon receiving that message, the Log class 
	* sends that very same message to all its subscribers
	* writes it in a "master log"

### Asynchronous vs. synchronous logging

**By default, logging is synchronous**, which means subscribers get notified as soon as a new message is fired.
However, you might not want to receive messages at that moment for various reasons (e.g. being in a thread where the ILogDevice you registered cannot be called).

In that case, you can choose to programmatically disable synchronicity by calling {{Log.SwitchAsync}}, and re-enable it with {{Log.SwitchSync}}.

During that period **when logging is asynchronous, your ILogDevice will not be automatically notified of any new message ATL might fire**. You will need to manually call {{Log.FlushQueue}} at the moment (in our case : in the thread) of your choice for all the latest messages to be sent to you.