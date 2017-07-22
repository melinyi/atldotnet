# Audio Tools Library (ATL) for .NET

## Status

**Linux and OSX Mono Build :** [![Build Status](https://travis-ci.org/Zeugma440/atldotnet.svg?branch=master)](https://travis-ci.org/Zeugma440/atldotnet) (powered by Travis CI)

**Windows .NET Core Build :** [![Build status](https://ci.appveyor.com/api/projects/status/s4y0e3g6fxncdhi6?svg=true)](https://ci.appveyor.com/project/Zeugma440/atldotnet) (powered by AppVeyor)

**Code coverage :** [![codecov](https://codecov.io/gh/Zeugma440/atldotnet/branch/master/graph/badge.svg)](https://codecov.io/gh/Zeugma440/atldotnet) (powered by CodeCov)


## What is ATL .NET ?

Audio Tools Library .NET is the C# port of [the original ATL](http://mac.sourceforge.net/atl/), written in Pascal by the MAC Team (Jürgen Faul, Gambit and contributors).

It is aimed at giving C# developers a native, portable and easy-to-use library to access and read data from various digital audio formats.

As a showcase, I have used ATL.NET as a cornerstone to build [Ethos Cataloger](https://trello.com/b/ZAzRjbXZ/ethos-cataloger), a digital music cataloging software written entirely in C#.


## What is NOT ATL .NET ?

Audio Tools Library .NET is not :

* a standalone application : it is a library aimed at being used by developers to build software

* an audio file tagger : it only _reads_ data

NB : I'm currently playing with experimental classes aimed at securely creating, editing and removing metadata (tags). It might take some time to stabilize, since altering audio files needs thorough testing

* an audio music player : it gives access to various properties and metadata (see below for the comprehensive list), but does not process audio data into an audible signal


## Why open source ?

ATL has been open source since its creation. The ATL 2.3 source written in Pascal language is still out there on Sourceforge !

By publicly sharing the result of their work, the MAC team has helped many developers to gain tremendous time in creating audio tools.

As a fellow audiophile and developer, I'm proudly extending and improving their initial contribution to the open source community.


## Why would I want to use ATL while TagLib is out there ?

* ATL is a **fully native C# implementation**, which makes portability trivial if your app is already based on .NET or Mono frameworks

* ATL features a **flexible logging system** which allows you to catch and record audio file reading/writing incidents into your app
  
* ATL features **Playlists and Cuesheets readers**


## How to use it ?  Which platforms and .NET/Mono versions does ATL run on ?

The ATL library runs on .NET 3.0+ / Mono 2.0+

ATL unit tests run on .NET 4.5+

The library and its tests have been maintained on Visual Studio Express 2012 and 2015

Please refer to the [Code snippets section of the Documentation](Usage-_-Code-snippets) for quick usage


## What kind of data can ATL actually read ? From which formats ?

* Supported audio formats and tagging standards :

NB1 : Empty cells mean "not applicable for this audio format"

NB2 : All metadata is read according to Unicode/UTF-8 encoding when applicable, which means any "foreign" character (japanese, chinese, cyrillic...) **will** be recognized and displayed properly


Audio format | Extensions | ID3v1.0-1.1 support | ID3v2.2-2.4 support | APEtag 1.0-2.0 support | Format-specific tagging support
---|---|---|---|---|---
Advanced Audio Coding, Apple Lossless (ALAC) | .AAC, .MP4, .M4A | yes | yes | yes | yes
Audio Interchange File Format | .AIF, .AIFF, .AIFC |  | yes |  | yes
Digital Theatre System | .DTS |  |  |  | 
Direct Stream Digital | .DSD, .DSF |  | yes |  | 
Dolby Digital | .AC3 |  |  | yes | 
Extended Module | .XM |  |  |  | yes (2)
Free Lossless Audio Codec | .FLAC |  | yes |  | yes
Impulse Tracker | .IT |  |  |  | yes (2)
Musical Instruments Digital Interface | .MID, .MIDI |  |  |  | yes (1)
Monkey's Audio | .APE | yes | yes | yes | 
MPEG Audio Layer | .MP1, .MP2, .MP3 | yes | yes | yes | |
MusePack / MPEGplus|.MPC, .MP+|yes|yes|yes| |
Noisetracker/Soundtracker/Protracker|.MOD| | | |yes (2)|
OGG : Vorbis, Opus|.OGG, .OPUS| | | |yes|
OptimFROG|.OFR, .OFS|yes|yes|yes| |
Portable  Sound Format|.PSF, .PSF1, .PSF2, .MINIPSF, .MINIPSF1, .MINIPSF2, .SSF, .MINISSF, .DSF, .MINIDSF, .GSF, .MINIGSF, .QSF, .MINISQF| | | |yes|
ScreamTracker|.S3M| | | |yes (2)|
SPC700 (Super Nintendo Sound files)|.SPC| | | |yes|
Toms' losslesss Audio Kompressor|.TAK| | |yes| |
True Audio|.TTA|yes|yes|yes| |
TwinVQ|.VQF| | | |yes|
PCM (uncompressed audio)|.WAV|yes| | | |
WavPack|.WV| | |yes| |
Windows Media Audio|.WMA| | | |yes|


(1) : MIDI meta events are all written to the track's Comment field

(2) : sample names are all written to the track's Comment field


* Detected fields
	* Audio data (from audio data) : Bitrate, Sample rate, Duration, VBR, Codec family
	* Metadata (from tags) : Title, Artist, Composer, Comment, Genre, Track number, Disc number, Year, Album, Rating, Embedded pictures

* Supported playlists formats : ASX, B4S, FPL (experimental), M3U, M3U8, PLS, SMIL (including WPL and ZPL), XSPF

* Supported Cuesheets formats : CUE


## What is the roadmap of ATL.NET ?

* Being able to create, edit and remove metadata (tags) <== WIP on the "IO" branch
* Support for **more video game-specific music formats** (.GYM, .VGM)
* Connectors to **other library file formats** (iTunes)


## Does ATL.NET include code authored by other people ?

ATL.NET is based on :

* Audio Tools Library 2.3  by Jurgen Faul, Mattias Dahlberg, Gambit, MaDah and Erik Stenborg (code ported from Pascal to C# and refactored)

* MIDI class 1.5 by Valentin Schmidt & Michael Mlivoncic (code ported from PHP to C# and refactored)

* CueSharp 0.5 by Wyatt O'Day (original C# class wrapped in CatalogDataReaders)
