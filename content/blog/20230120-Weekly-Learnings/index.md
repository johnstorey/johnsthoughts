---
title: "20230120 Weekly Learnings"
date: 2023-01-20T20:01:00-06:00
---
This week has been solidifying a first cut at infrastructure to develop side projects on. Lots of research currently has me settled on the following technologies.

## Tech Stack Evolution

This week I rethought the infrastructure for software development and deployment. With a career of enterprise software behind me I had to re-orient based on priorities around side hustle projects. To do so I looked into what individual developers use, and was very happy with what I found.

### BaaS

The number of great Backend as a Service (BaaS) solutions since I last looked years ago has exploded. Starting with Firebase, then looking at Supabase, I ended up with Pocketbase. The determining factor was that Pocketbase can also be packaged as a Go framework, letting it be much more easily extended for application specific needs. Packaged as a single Go binary, I was able to start coding against it 5 minutes after downloading it. The major downsides are that it is tied to SQLite right now, and only scales vertically. With the currently envisioned projects that should not be a huge limitation.

### Git Forge

Not much thought here -- I already had Gitea running for my other projects. I just made as an organization to work together in.

### PaaS

There turns out to be a large number of open source PaaS solutions if you don't want to be on the big cloud providers. Many people seem to be excited by CapRover, but I ended up wth the more proven Dokku. It works in ways I understand, setting it up both on Digital Ocean and locally in a container was relatively simple, and I hosted PocketBase directly inside of Dokku.

Dokku has a reputation on the internet as being solid, and it seems to be holding up in practice, so we are diving in. Pre-existing plugins also include RabbitMQ and infrastructure monitoring. The downside is that, unless you use the deploy to Kubernetes option, it is a single point of failure. So not needed for five nines, and enabling backup seems pretty critical. Again, that's not an issue in the use cases we are thinking of.

## Front End Development

The short answer is that we ended up with Flutter, for a variety of reasons.

With the Fable F# to Flutter compiler that is coming I hope to bring F# back to my front end solutions. Meanwhile the stack will allow it for any additional backend services I might need.

## Networking and Hardware

I've added one of my home servers as a Tailscale node letting us have a basically free shared server. Applications are run in either Docker containers, or using Vagrant on VirtualBox. Scripts back these up to AWS S3 on a regular basis.

To prove it out, I did deploy and test Dokku on Digital Ocean, with Dokku hosting PocketBase. So products developed on the home server should translate to there very easily. Until then there is a reverse proxy on a Droplet that is a Tailscale node reverse proxied to any service we want to share with a larger audience.
