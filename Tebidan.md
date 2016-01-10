---
title: Tebidan
permalink: /Tebidan/
---

Tebidan
=======

A Debian Derivative for Pragmatic People

Goals
-----

-   More frequent stable releases than Debian
-   I see four major use cases for a linux distribution, in order of difficulty
    -   servers
    -   developer workstations
    -   embedded hardware
    -   end-user machines

(servers and developer workstations are sort of paired - you want to be deploying on (a subset of) what you run locally, and you want them both to not be embarassingly outdated.)

Do we want a rolling release?

Identity / branding
-------------------

People will ask, is this a fork? Is this a downstream distro? We should have some concise statement of purpose....

I think one nice brief one is "what Ubuntu was in 2004, except with no commercial entity behind it". This immediately gives us two subgoals:

-   We're trying to get a release out frequently, possibly matching the Ubuntu model of sixmonthly time-based releases (Ubuntu took a few years before starting doing the LTS, we can too). We're importing Debian, but reserve the right to patch things for the goal of getting a release out.
    -   We're willing to merge things ahead of Debian, though, like Ubuntu did with multiarch.
    -   A maintainer or uploader of a package in Debian can freely push updates to the corresponding Tebidan package, except during our own release freezes (which should be maybe one month of every six).
    -   When Debian is frozen, humans will import from experimental, packaging repos, etc. to try to keep things up-to-date.

<!-- -->

-   We're trying to provide reasonable security support for multiple distros, rebuild everything, manage transitions, etc. This means we need to put a lot of focus into automation and infrastructure, since we have a small team. Code improvements to infrastructure should go back to Debian.

We should try to be more firmly "upstream first" than Ubuntu, both to be a good community member and for our sanity. We might want a rule re [patch tagging guidelines](http://dep.debian.net/deps/dep3) and the "Forwarded" header -- everything must be "not-needed" or "yes" with a link, and all "no"s are release-critical. If we're patching things in git (see below) we may want a canonical formatting of the tags inside git commit messages. We may also want a similar rule re local packages -- the initial changelog entry must say it's not needed in Debian, or close a Debian ITP.

We like Debian as a community, but not as a bureaucracy. We're thankful for the time that the release managers put into making them happen, but we think time-based releases are cooler and we think we can automate out the hard parts. We can experiment, since we have a smaller (currently zero) user base.

Initial release goals
---------------------

(Initial release will be Tepidan 0 "insanity wolf")

-   Initial release should target amd64 and servers (and workstations for the really grumpy)
-   build packages for i386 but not an installer (because apt gets whiny otherwise, and you sometimes need to run 32-bit software on a server)
-   EC2 AMI
-   basic support for developer style workstations
-   DEBUGGING SYMBOLS FOR EVERYHTING!!!one
    -   We can just use, like, pkg-create-dbgsym from Ubuntu. We may want to patch out separate -dbg packages or something, but that seems annoying.

Build infrastructure
--------------------

-   No FTP / dak / dput nonsense
-   No building packages on uploaders' personal machines
-   We need an automatic build environment. Presumably sbuild, since we all know it. Maybe also wanna-build, but do any of us know it?

<!-- -->

-   We need a way to track patched source packages. Some form of git import might be the right answer, but let's be less terrible than [UDD](https://wiki.ubuntu.com/DistributedDevelopment).
    -   TODO: Why is UDD terrible?
    -   TODO: Should we start with bzr fast-export of UDD | git fast-import? Probably not.
    -   TODO: Understand git-dpm better

<!-- -->

-   We need an authentication mechanism for developers. It's probably going to end up being Kerberos, frankly.
    -   Of course it's going to be Kerberos. And when we need shared file space, what do you think we're going to use for that? :-)
        -   Uh, NFSv4?
        -   So, is shared filespace the way to upload source packages? Or is pushing tags the way?
        -   oh god pushing tags. (If it's a choice between those two)
    -   So, we need some sort of web-based mechanism for provisioning Kerberos accounts

Source packages: "sources are canonically tracked on git.tebidan.org, preferably importing an upstream VCS if such exists. Source packages are all format 3.0 (native) because source packages are dumb. If you actually care about anything other than source diving / having sources for gdb / etc., you want a git clone, not an apt-get source. We'll ignore the ftp-master problems with distributability of 3.0 (git) packages, by just rebasing everything if a DMCA takedown happens. Mirrors only need to distribute the current binaries and the current 3.0 (native) source packages."

AntiGoals
---------

-   Divisive space-alien user interfaces
    -   That said, Unity isn't in Debian because it patches too many libraries. It might be worth having Unity to attract the crowd that likes it, but it will involve some packaging cleverness.
    -   I think supporting unity for the people who like that sort of thing is a great idea (it's a collection of not-bad UI ideas) but supporting on X (and, presumably, eventually Wayland) might need more resources than we have.

StretchGoals
------------

-   arm6hf for the raspberry pi people

Other
-----

-   Bender seems to (at least temporarily) be our mascot
-   Also, this [hot dog](http://fedoraproject.org/wiki/Features/Hot_Dog) thing might be a good idea.
-   The masses will need a name that's easier to remember and pronounce. Since I don't think any of the contributors have kids to name it after, here are some random words for your amusement. Let the bikeshedding begin!
-   dozen mother finch there ensue apse shot taper browse filch naked tacit toast comic impute canopy forge keys hath bandy
