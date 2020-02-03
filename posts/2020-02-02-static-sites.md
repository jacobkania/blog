---
Title: The state of static websites
PublishedDate: 2020-02-02
---
It is rare to find a modern static website these days. The first things that come to mind when thinking of a static website would probably be: "clunky", "ugly", "geocities-era", "comic sans", etc.

It's 2020 (Happy Palindrome day!), and JavaScript is great. There are lots of libraries out there which make it possible to build really powerful websites, and those websites can do amazing things. It's like people used to say about Java or Oracle: *"Nobody ever got fired for choosing to use React".*

I am a minimalist at heart. I like to make things simple for myself (within reason), and I see a lot of value in not over-engineering things.

# Is using React/Angular/Vue over-engineering?

This is a tough question, with an unfulfilling answer: **it depends.**

I use React at work, and we have a very complicated application. We have multiple users who all need web-form data to be synchronized in real-time. There is a lot of information that needs to be shared between pages, and a lot of background tasks that run on our servers, pushing updates to the users of our web-apps. Clearly, we need a lot of JavaScript.

It mostly makes sense to use a big JS framework when you're already heavily reliant on JavaScript as we are. Large parts of our app wouldn't work without it, so it's a requirement anyways.

It's also good to use a big JS framework if you'll have a lot of component re-use -- specifically with a good internal component library. Being able to write something generally once and apply it in many places is one of the biggest strengths of React.

**However, I think it's unnecessary for many websites.**

Take this blog for example. Each page that you look at is just a single HTML page. No external requirements to display content. This was intentional. A blog is mostly static, with some content changing whenever I write something new. There's no reason to waste tons of processing power on rendering it with JavaScript every time someone looks at it -- I just use [my program](https://github.com/jacobkania/mmssg) to generate all of the pages once from a template, and that's it.

This makes the page load faster than it could *ever* be if it were built with a JS framework, easier to understand for a future developer, and it just feels good to have no dependencies. I also didn't need to think at all about architecture, best practices, maintanability, performace, or longevity: HTML and CSS will work for a long time.

# What can you do?

Well, this is tough. Using your engineering mind to build websites is genuinely more fun, and I do really enjoy experimenting with architectures in React. I just don't know if it can be justified for all of the simple websites out there, especially with the extensive tooling that exists for serving static content. Github Pages is free, for example -- and if they ever shut down that service, it's absolutely trivial to move to another platform.

If you have something big and complicated to build, and if it needs lots of scripting built into the core functionality, then yeah you probably should use a JS framework. However, there are many cases where an application really could be a few simple HTML pages (or templates), containing a few slivers of [vanilla JS](http://vanilla-js.com/) sprinkled around the website for interactions that you do need.

I really believe anyone claiming either way that *everything should be a static site* or that *everything should be JS* is very wrong. I think that both have their places, but I also believe that more websites could be static than currently exist today.