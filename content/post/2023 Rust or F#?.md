---
title: 2023 Rust or F#?
tags:
categories:
date: 2022-12-11
lastMod: 2022-12-14
---
# Why Am I Choosing a New Language

Programmers periodically need to keep up to date with the world around them. Tools, methodologies, and languages are in a constant state of improvement. One must continually reevaluate and learn new technologies. In addition, I feel it is just time for me to go all in on a new ecosystem.

So here is my paper evaluation of what to program with, personally, in 2023.

# What Am I Looking For

The days where it made sense for NULLs to be a commonly used language construct have passed. Too many days, nights, and weekends of my life have gone to dealing with that foul demon. Yours as well I bet. In my choice, NULLs must be recognized for the pernicious gremlin they are. Ideally it will be impossible to write code that causes a null pointer dereference.

Secondly I want productivity. My 2023 project plans can broadly be described as SaaS applications implemented with a serverless architecture. These personal projects are likely to continue as one man efforts with limited time to devote to them. So productivity is a key metric for success. Since I'll maintain the code over time, anything that makes it easier to come back to in a year, grok, and maintain is a huge win. I will measure that as expressive code that does the job with the fewest needed coding patterns.

Thirdly, I want a good type system that I can extend easily. F# is the king of this in the languages I have used. However Rust has it's New Wrapper pattern that seems to do the same thing with only slightly more ceremony.

Fourth, I hate slow code. Forget forgiving performance budgets; forget it's a business app that we can scale horizontally; I just don't like it. I firmly believe 90% of all software going forward is going to be serverless, and scaling something that takes time to cold start in a serverless function is going to be a needless expense. AWS is doing interesting things here with .NET and AOT, but that does not necessarily help my home Kubernetes cluster, or my OpenFaaS server. So compiled with no garbage collection sounds very attractive to me.

Lastly, personal ramp up time in the language and ecosystem around it must be as minimal from my current skillset as is practicable. There are problems to solve, and I need to get to them.

(As an aside, I do play with machine learning and data science a bit, so Python would be under consideration. But I know enough to use it for those purposes, and it's not a personally interesting language to me.)

# Why Are These the Options?

I'm choosing these languages as candidiates for reasons of personal interest. Haskell was nearly on the list as well, but in the end did not seem right for me at this time. Rust attracted the speed demon and null hater in me. My current position uses C# and thus the .NET ecosystem, and F# would let me leverage that existing knowledge. Also the F# type system is just so nice to work with.

# Comparison

Keep it simple right? Make a table, and rank each from 1 to 3, with 3 being the best rating. That's not a nuanced discussion, but it's a good overview of where things fall.

| Language | Allows NULL | Productivity | Type System | Performance | Ramp Up Time |
|---             | ----               | ----           | ----             | ----              | ---
| F#            | No | 3 | 3 | 1 | 2 | 
| Rust | No | 2 | 2 | 3 | 1 |

You can argue with my ratings as they are personal. I gave Rust a low rating for ramp up time as I'd need to internalize the borrow checker. Then I'd need to constantly think about what I'm really doing when programming in the language, dropping the productivity rating. 

F# got a high productivity rating as my work tools and learnings would be synergistic with F#. It sacrifices performance though as it's not compiled and has a garbage collector. As mentioned earlier there is .NET AOT, and that might provide a solution if it becomes an issue. 

# Decision

The conclusion for me seems obviously to be F#. I'll be honest -- that's not the answer I want. I feel the tooling in F# is lacking. I want to use the Rust Yew web framework. Performance is an almost painful desire for me. But I won't get the serverless backed SaaS apps implemented nearly as quickly, and F# has that great type system.

Do your own analysis and decide what makes sense for you. This is a very individual choice. As my experience with F# grows the evaluation may change. But that's all part of the gorgeous journey of programming. You grow, change, and reevaluate all the time.

