<div align="center">

<img src="https://quiblo-iptv.github.io/quiblo-wiki/brand/quiblo-wordmark-1280x320.png" alt="Quiblo" width="128" height="128">

# Quiblo

**A free, open source IPTV player for Android phones and Android TV.**
Bring your own playlist. No ads, no accounts, no tracking, no backend.

[![Download](https://img.shields.io/github/v/release/quiblo-iptv/quiblo-app?include_prereleases&sort=semver&label=download&color=3DDC84)](https://github.com/quiblo-iptv/quiblo-app/releases/latest)
[![Licence](https://img.shields.io/badge/licence-GPLv3-blue)](https://github.com/quiblo-iptv/quiblo-app/blob/main/LICENSE)
[![Sponsor](https://img.shields.io/badge/sponsor-GitHub_Sponsors-EA4AAA?logo=githubsponsors&logoColor=white)](https://github.com/sponsors/quiblo-iptv)
[![Patreon](https://img.shields.io/badge/support-Patreon-FF424D?logo=patreon&logoColor=white)](https://www.patreon.com/c/Quiblo)

[**Download**](https://github.com/quiblo-iptv/quiblo-app/releases/latest) ·
[**Read the wiki**](https://quiblo-iptv.github.io/quiblo-wiki/) ·
[**The code**](https://github.com/quiblo-iptv/quiblo-app)

</div>

---

## What it is

Quiblo plays Live TV, films and series from playlists **you** already have — an M3U or M3U8
URL, a file on your device, or an Xtream Codes account.

There are two apps in every release and they are not interchangeable. One is for phones and
tablets. The other is for Android TV and Google TV, with an interface built for a remote
rather than a phone layout stretched across a television. They share everything below the
screen and nothing above it.

It is free software under the GPLv3. Every feature is in the build everybody gets. Nothing is
paywalled, now or later.

## What it is not

This matters more than the feature list, so it goes first.

**Quiblo ships with no channels, no films and no way to find any.** It is a player, in the same
category as VLC. If you do not already have a playlist or a subscription, there is nothing here
to watch — and this project will not help you find one. Requests for sources, providers or
bundled content are closed without discussion, and posting a playlist URL, a provider hostname
or a credential anywhere in this organisation gets you banned immediately.

That is not a legal disclaimer bolted on at the end. It is the reason several things are built
the way they are.

**It has no server.** There is no account, no telemetry and no backend. Quiblo makes no network
request to any host you did not configure yourself. You can check that with a packet capture,
and one of the acceptance criteria says exactly that.

---

## The other reason this exists

I built Quiblo with [Claude Code](https://claude.com/claude-code), and I kept the whole record
of it in the open — including the parts that went badly.

Most write-ups about coding with an agent are a screenshot of something working. That is the
least useful half. What I wanted was the other half: what an agent gets confidently wrong, what
it cannot see, and what a person still has to do. So the reasoning lives in the repository
alongside the code, in eleven dated rounds, and none of it has been tidied up after the fact.

**A few of the things that went wrong, because they are the parts worth reading:**

- **Nine features existed in the code and could not be reached from a running app.** They were
  in the README's feature table. Two of the settings controls had an empty click handler and a
  hardcoded selection. Picture-in-picture could never have worked — the manifest flag was
  absent, so the call threw into a bare `catch` every single time. All nine were deleted, then
  four were rebuilt properly.
- **Subtitles had a menu, a working track selector, and a bug closed on giving both apps a way
  in — and had never put one word on screen.** The engine reports subtitle cues and something
  has to draw them; both apps drew into a bare surface with nowhere to put them. Every check
  asked "can you reach it", and the answer was yes for months.
- **A rate limiter ran at exactly twice its documented rate**, with a passing test. The test
  measured one request's wait. The bug only exists across several.
- **A cache stored failures as answers.** Every metadata error became "this title matches
  nothing", cached for a fortnight, invisible one poster at a time.
- **The account got blocked by the provider twice.** The second time, the cause was that a
  concurrency cap is not a rate limit — three requests in flight at 100ms each is thirty a
  second, and a fling composes hundreds of rows.
- **One screen shook when scrolling, and it took four wrong answers to find out why.** Two of
  the four were shipped. The thing that finally solved it was a test that measured the movement
  frame by frame, not another argument about what the cause probably was.

The pattern in all of it: **an agent is very good at building the thing and very bad at knowing
whether the thing is real.** Green tests, a clean build and a confident explanation are not
evidence that a feature works. Somebody has to go and look.

If you are learning to build this way, [`agile/`](https://github.com/quiblo-iptv/quiblo-app/tree/main/agile)
and [`docs/`](https://github.com/quiblo-iptv/quiblo-app/tree/main/docs) are the point of the
repository. The code is the easy half.

---

## Ask me anything

**I am happy to help.** If you are new to this — to Android, to Kotlin, or to working with a
coding agent — open a [discussion or an issue](https://github.com/quiblo-iptv/quiblo-app/issues)
and ask. Questions about how something is built, why a decision went the way it did, or how to
get an agent to do something useful are all welcome, and no question is too basic.

Two exceptions, and they are the only ones: **do not ask where to get channels, and do not post
a playlist or a credential.**

---

## Where to start

| | |
|---|---|
| **Just want the app** | [Latest release](https://github.com/quiblo-iptv/quiblo-app/releases/latest) — two APKs, one for phones, one for TV |
| **Want to know what it does** | [The wiki](https://quiblo-iptv.github.io/quiblo-wiki/) |
| **Want to read the code** | [`quiblo-app`](https://github.com/quiblo-iptv/quiblo-app) — Kotlin, Compose, Media3, multi-module |
| **Want the honest account of building with an agent** | [`agile/`](https://github.com/quiblo-iptv/quiblo-app/tree/main/agile) |
| **Want to help** | Star it, file a good bug report, test a release on hardware we do not have, or fix something in the wiki |

## Supporting it

Quiblo is free and stays free. If it is useful to you and you are able,
[GitHub Sponsors](https://github.com/sponsors/quiblo-iptv) or
[Patreon](https://www.patreon.com/c/Quiblo) help — but read what that does and does not buy
first, because we would rather be clear than funded:

- **No feature is ever paywalled.** If it exists, it is in the free build for everyone.
- **No sponsor influences what gets built.**
- **No sponsor logo goes in the app.** Quiblo makes no request to a host you did not configure,
  and a logo fetched at runtime would break the one promise this project most cares about.

What the money actually goes to, named plainly: a second television and another phone to finish
testing on real hardware, an account to test against, and the developer accounts needed to
bring Quiblo to Samsung and LG televisions.

---

<div align="center">

Built by [**@maxmya**](https://github.com/maxmya) with [Claude Code](https://claude.com/claude-code).<br>
GPLv3 — free software, and free to stay that way.

</div>
