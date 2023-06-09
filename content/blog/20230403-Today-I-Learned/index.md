---
title: "20230403 Today I Learned"
date: 2023-04-03T08:29:09-05:00
draft: false
---

Technically "this weekend I learned" but still. Maybe even better "what I learned in the 2 - 3 hours I was able
to squeeze out for coding this weekend". ... anyways ...

I've been using my free time to continue to work on the Docker Registry with bittorrent distribution project. I had
initially been very happy with the ChatGPT generated code. Unfortunately when I really started debugging it I did not
find an cohesive logic that allowed me to understand the intent of the code. That made fixing the bugs that were
in the generated code difficult, and I could see that it would not be very maintainable. I did see a very good
overall architecture I'll borrow from though.

Well, a professional might start from an OpenAPI specification anyways. A search on Google yielded such a specification.
When run through openapi-generator it yielded a Rust Tokio solution. I'm getting more familiar with Rust, so afer some study the
intended usage of the generated code made sense.

Soon after I ran into what appears to be my first experience of Rust breaking its promisese, specifically helpful
and working compiler suggestions to fix your code. The issue is this line here:

{{< highlight rust>}}
        Ok(V2NameBlobsDigestGetResponse::
            TheBlobIdentifiedByDigestIsAvailable_2(
                ByteArray::from(swagger::ByteArray(contents))))
{{< / highlight >}}

The last expression in the ByteArray::from() call warns that "swagger::ByteArray" is not needed, and suggests I remove it. The Rust compiler
nicely refuses to compile with warnings.

Ok! This is the promised helpful compiler messages in action, right? Let's take that redundant code out.

Now the code won't compile because "contents" is the wrong type. Rust seems to want only explicit type conversions.

After a few cycles of that I found the directive to override warnings and compile anyways. The code now looks like

{{< highlight rust>}}
       #[allow(warnings)]
        Ok(V2NameBlobsDigestGetResponse::
            TheBlobIdentifiedByDigestIsAvailable_2(
                ByteArray::from(swagger::ByteArray(contents))))
{{< / highlight >}}

I'm actually pleased that the code refused to compile with warnings. I've seem code bases with over ten thousand warnings of 
potential nil defreference in C# that the developers happily ignored. My belief is warnings should be treated as 
errors by default. Rust seems to hold to that belief.

That does not look like much of an accomplishment, but I spend a bit of time in documentation learning about other Rust language features along the
way, debugging some match statements by studying generated code to see the generation pattern, and reading lots of Tokio 
documentation to understand what the existing code does and what I am intended to fill in.

All in all, a short but satisfying session. I see why people want Go for web services, especially serverless though. A memory leak in a serverless function will initially seem to have no effect because the function shuts down between calls. Until you start to 
scale and the cloud provider never gets a chance to shut it down -- then your issue will show up. In other words, Go lets you get it out
faster; Rust works on keeping your long term maintenance costs low. It's the maintenance costs that matter in the long term, but a
startup might thing time to market is more important.