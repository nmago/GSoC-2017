### Welcome!

This is my final evaluation submission page for project "Crash Reporter for Tor Browser", written code can be found in [Tor Browser repository clone](https://github.com/nmago/tor-browser/commits/crdevc?author=nmago), but most work of this project was focused on non-coding stuffs (analysis, building, error handling).

I biweekly sent [status reports](https://trac.torproject.org/projects/tor/wiki/doc/gsoc#StatusReports) to [Tor Project mailing list](https://lists.torproject.org/pipermail/tor-project/) (from nmagoru at gmail.com)


#### Project description
 
The goal of my project was to adapt existing Crash Reporter from Mozilla Firefox for Tor Browser: make it completely anonymous for end users and set up server side that will be collecting crash reports. 

So Crash Reporter consists of:
 - Client side: collects crash data and sends it as reports
 - Server side: the web service that gets reports from client


#### What we did

##### Client

The first problem was that there are privacy-sensitive data fields in crash reports and we need a way to exclude these fields from reports. And the second problem was that Crash Reporter client may store reports on the user's machine for an extended period of time (it could make it dangerous for user's privacy, for example using site URLs in reports someone can find out what sites user visited). So in client side we made features to [send only "safe" fields](https://github.com/nmago/tor-browser/commit/2e11a5d429c6b915843091a8a1ac6d9f67248c6a) and [delete all reports](https://github.com/nmago/tor-browser/commit/9f8cc07aed4760604d74703f1991e2e5fbe9f441) (regardless on report is sent or not).

Before we had made first feature we [created blacklist](https://github.com/nmago/tor-browser/commit/ed40112896b88057149f54452195e545f4c1c286) to exclude privacy-sensitive fields from reports. Later we [redefined](https://github.com/nmago/tor-browser/commit/1b3bf799aec4327e99eb0fe0365a637a2dd49790) this feature as whitelist because the function that adds data fields to reports is called from different parts of browser and it's easier and safer to use whitelist to not miss any field. 

##### Crash report data field analysis

To collect info and analyse report fields we made [Wiki-page](https://trac.torproject.org/projects/tor/wiki/doc/crashreporter) for project. Some fields can be easily divided into privacy-sensitive and "safe", but we had problems with others:
 - Can be used this field for find out about user?
 - What's about combinations of 2 or more fields?

We've started to carefully choose fields one-by-one and now have 5-10 that we sure in. There are interesting ideas in project wiki, such as making different privacy options in client (to choose by users). Also there are a complex fields (Add-ons, modules) in reports that (may be) should be filtered.
It's "war" between privacy and helpfulness of crash reports for developers.

##### Server

At first we worked on setting up Mozilla Socorro, but we encountered a problem: we spent 2 weeks uselessly trying set it up with Docker, so we had spoke with Socorro team and got strong recomendation that sounds "don't try and use Socorro" :) 

Finally, we decided to use  [Mini-Breakpad-Server](https://github.com/electron/mini-breakpad-server) (MBS) and [broke MBS into 2 services](https://github.com/tomrittervg/mini-breakpad-server/commit/d060952ef89ddd5aa0e95f4345faf206bef7a878): 
 - service to get reports from clients
 - service to view reports

Other option was "creating authorization" but it could make service intricate and unsafe, so "break into 2 services" was the fastest and easiest way. 
Both of services are set up as .onion: to send reports through the Tor network (why don't use main feature of Tor Browser - anonymous network to send?). 

##### Building Tor Browser

At the beginning we built just browser (not Tor Browser Bundle) to test features and in last 3 weeks of GSoC period we tried to build Tor Browser Bundle using Reproducible Build Manager (RBM), few days we spent building Windows version (also got erros such as [this](https://bugzilla.mozilla.org/show_bug.cgi?id=1391685)), after that returned to Linux version and we managed to build Tor Browser reproducible with enabled our updated Crash Reporter. 

In this project building was very time taking and nothing doing process: you start build and have to just look, got logs, fix errors and start again. Dont sure have I to consider it as work hours :/


#### What is next?

There are much work to do (and I intend to keep working on this project):
 - Work on UI of Crash Reporter client
 - Work on MBS: adding various features into report viewer such as filters, search, design stuff and other things that could make reports view/analysis easier
 - Adapt Crash Reporter client for other platforms: this Summer we worked only on Linux version


#### P.S.

Everything related to GSoC was awesome. Thanks a lot to The Tor Project, especially to Tom Ritter and Georg Koppen, I'm really glad to work with you! And, of course, thanks to Google that made GSoC :)


