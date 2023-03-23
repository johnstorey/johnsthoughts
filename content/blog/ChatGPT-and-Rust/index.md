---
title: "ChatGPT and Rust"
date: 2023-03-23T07:40:14-05:00
draft: false
---

A couple of weeks back I had an exchange of messages on the Fediverse that ended in me speculating there should be a Docker Registry that supports Bittorrent for image distribution. Forget paying a centralized entity to host your containers; host them yourself. For big OSS projects, or inside large enterprises, this would make lots of sense.

After a search on Google I found some prior art. One was a weekend project that did not quite fit what I was thinking of, and a better one had a hard dependency on Kubernetes. These existing solutions don't seem suitable for the use cases I am thinking of.

The catch here is that I am currently deep into learning the F# ecosystem, and loving it. This project screams for a systems language, and while Rust has long been on my "to learn" list it's difficult to master two ecosystems, have a job, have a family, and other concerns all at once. That's a recipie for burnout for me.

But this project is something the world needs, and personally, I find it exciting.

So what to do? I decided to try to get ChatGPT to write it for me.

An unknown language, unknown ecosystem, and using AI to accelerate both my learning curve and productivity?

Exciting!

## Results

I was able to get the framework from ChatGPT in 1 hour. For comparison, if I knew Rust better, my estimate is this would be .5 to 1 man days of work. The code does not compile, undoubtedly has logic bugs, and in general is a far sight from production ready. But it would be better code, or at least compile. But I'll tell you after decades of programming: the bones are good.

ChatGPT probably just sped up my work by 4X to 8X for this part of the development.

After that, it requires a human right now. I could probably get some basic error checking out of ChatGPT, but don't trust it to avoid logic errors for example. I did make alot of progress feeding the errors back into ChatGPT, but there were limits to the effectiveness of that. Not tested was feeding the whole repository to ChatGPT, along with errors, and seeing if it could do better with that context to work with.

Along the way I was educated by ChatGPT about the current way Bittorrent distributes files, the types of Rust client libraries I could use, and aspects of the language that helped me level up.

You can take a look at the resulting code [here](https://github.com/johnstorey/dockerrush).

## From Here

This has motivated me to learn Rust and its ecosystem now. That is despite the fact I am in the midst of the same process with F#. (F# is a beauty btw.)

But Rust an awesome systems language. Using it reminds me of early in my career when I was a consultant working with object databases. You have to keep track of the memory model a bit, but in return you should get screaming performance. Algorithms trump language, sure, but Rust makes it much, much easier.

I'm going to fix this code, expand it, and put it out there. I can't wait until its time to think through a network of managed nodes here. Should we just use the existing Bittorrent model? Are there solution specific issues that require different distributed management? It's going to be fun.

Since this is my vehicle for really mastering Rust (I learn best if I do while learning) so don't judge the code too harshly. I normally work with a local git server, but put this one on GitHub because I hope others also believe P2P networked, privately controlled, networks of Docker Registry servers would provide significant value to the worl.