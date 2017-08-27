## Final evaluation submission page
### Nur-Magomed Dzhamiev

#### Project description
 
The goal of my project was to adapt existed Crash Reporter from Mozilla Firefox for Tor Browser: make it completely anonymous for end users and set up server side that will be collecting crash reports. 

So Crash Reporter consists of:
 - Client side: collects crash data and sends it as reports
 - Server side: the web service that gets reports from client

#### What we did

The first problem was that there are privacy-sensitive data fields in crash reports and we need the way to exclude these fields from reports. And the second problem was that crash reporter client keeps reports on user storage (it could make it dangerous for user privacy, for example using site URLs in reports someone can find out what sites user visited). So in client side we made features to [send only "safe" fields](https://github.com/nmago/tor-browser/commit/2e11a5d429c6b915843091a8a1ac6d9f67248c6a) and [delete all reports](https://github.com/nmago/tor-browser/commit/9f8cc07aed4760604d74703f1991e2e5fbe9f441) (whether it sent or not).

Working on server side, we had problems with Socorro (that was proposed as collect server) and we decided to use  [Mini-Breakpad-Server](https://github.com/electron/mini-breakpad-server) (MBS). 
We [broke MBS into 2 services](https://github.com/tomrittervg/mini-breakpad-server/commit/d060952ef89ddd5aa0e95f4345faf206bef7a878): 
 - service to get reports from clients
 - service to view reports
These services work in *.onion* domains.

Resolving the first problem we created [Wiki-page](https://trac.torproject.org/projects/tor/wiki/doc/crashreporter) for project and at the end we can build Tor Browser reproducible with enabled Crash Reporter.

#### What is next?

There are much work to do and I intend to keep working on this project:
 - Work on UI of Crash Reporter client
 - Work on MBS: adding various features into report viewer such as filters, search, design stuff and other things that could make reports view/analysis easier
 - Adapt Crash Reporter client for other platforms: this Summer we work only on Linux version

#### P.S.

Everything related to GSoC was awesome. Thanks a lot to The Tor Project, especially to Tom Ritter and Georg Koppen, I'm really glad to work with you! And, of course, thanks to Google that made GSoC :)


