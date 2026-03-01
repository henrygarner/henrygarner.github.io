---
layout: post
title: "From Specification to Stress Test"
date: 2026-02-11
categories: ai distributed-systems specification
---

Over the weekend I described the behaviour I wanted from a distributed system and let Claude Code build it.

Byzantine fault tolerance, strong consistency, crash recovery under arbitrary failures, and I didn't write a line of code. 48 hours later my load and resilience tests were all passing.

I wasn't sure this would work. I've spent enough time with these problems to know how subtle their errors can be. But Claude's crash-recovery testing found a race condition that only surfaces when two nodes fail simultaneously.

What caught it wasn't me reading the code. The specification defined correct behaviour precisely enough to demonstrate that the implementation was wrong and what the fix should look like.

I didn't write those specifications either. I described to Claude what I wanted from the system and we worked through trade-offs and implications together. The reason it worked, I think, is that I knew what to ask for.

Knowing what your system needs to do has always been the hard part. That hasn't changed, even if everything around it looks completely different now.

I wrote up the process, the bugs and what behavioural specifications made possible [on the JUXT blog](https://www.juxt.pro/blog/from-specification-to-stress-test/).
