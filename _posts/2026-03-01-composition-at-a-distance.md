---
layout: post
title: "Composition at a Distance"
date: 2026-03-01
categories: ai software-engineering allium specification
---

I've been thinking recently about where design thinking happens in software engineering.

For twenty-five years, code has been the primary place to work out what you're building. The Agile Manifesto said "working software over comprehensive documentation", and that has sometimes been taken as permission to skip the specification step entirely.

But the signatories were people who wrote books about design, use cases and test-driven development. They weren't rejecting specification: they were pragmatically routing around the tools of the day that couldn't keep up.

We've been using [Allium](https://github.com/juxt/allium), a behavioural specification language designed for LLMs, to capture what systems should do while implementing the code. When we built a distributed system with Byzantine fault tolerance, the spec and the code iterated together. Contradictions we'd have discovered mid-implementation showed up in the spec before we'd written a line of code.

Code is valuable, but maybe it was never the right place to work out *what* you're building. It's the right place to work out *how* to build it.

The assumption that those two things belong in the same medium might be a historic constraint of the tooling, not a law of nature. I've written about the history behind that assumption and what I think's changed [on the JUXT blog](https://www.juxt.pro/blog/composition-at-a-distance/).
