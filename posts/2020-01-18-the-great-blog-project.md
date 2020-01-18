---
Title: The great Blog project
PublishedDate: 2020-01-18
---
I just had a (post)[https://jacobkania.com/2020-01-14-crappy-personal-projects] a few days ago about how I believe that creating crappy personal projects is a good thing. This is my full relevant story.

Writing the software to run a blog was a big challenge for me. Not because writing the software itself was challenging, but because I kept wasting time on creating the wrong thing. I had a whole list of requirements for the blog website. It had to:

1. Support markdown for text input.
2. Allow me to add posts easily, and edit everything about them.
3. Be secure, without requiring any constant security updates or ongoing maintenance.
4. Just work. I didn't want to be constantly fiddling with settings.
5. Allow me to customize anything about it that I wanted. Colors, themes, etc etc etc.

I knew about *github pages* existing, but wasn't sure of the best way to get a whole static site built to host there. Jekyll was the default that they offered, but it seemed too weak to fit most of my needs without lots of customisation. I didn't want to learn to use static site generators that already existed -- they were out there, but they all seemed to need a ton of configuration. So I wrote my own "blog hosting engine".

## The first try

I created [(bla)](https://github.com/jacobkania/bla) in Winter of 2018.

It was fairly simple, but included a few good features: auth for admin users to write posts, a frontend ui for writing posts, and a way to host content on customisable HTML pages. The biggest downside though, was that this was a Go web-server application. It needed to be running 24/7 on a server and wasn't easily scalable. This would be a big problem for me, since I expected occasional spikes in traffic, while normally very minimal regular visitors. This would mean that I'd need to spend a lot of money on a powerful enough server for those unexpected spikes, while it would sit idle for days or weeks without much traffic at all.

## The 'settling' for something

I ended up abandoning Bla after that realization, and because I found a platform that fit many of my needs: [Svbtle](https://svbtle.com). This was a fully managed blog site, very minimalist themed (which I like), and allowed for markdown in the posts. It automatically scaled, and cost me $6.00 per month. Not too bad. But it didn't check all of my boxes, and I didn't like how they organized my index page. It was good enough for a while, but eventually I felt unmotivated to really use it.

## The pointless projects

I started two or three more projects over the course of the next year, playing around with different technologies. I build a React frontend with a Ruby backend to host the blog. I tried building it all in ruby using HTML templates. I tried building it all in vanilla Javascript using config files to load all of the pages and extract metadata (SO SLOW!).

I churned through 3 or 4 different pointless projects while I had Svbtle hosting my blog, all the while barely writing new posts and paying them their monthly fee.

## The solution

Finally, I figured I'd make my own static site generator. It would be simple to use, easy to template for, and I could host my entire blog on github pages. This allowed me to hit all 5 of my requirements. I also hadn't written Go in a while, so this was a good way to get back into it.

I wanted the software itself to be very minimal -- the MVP that I was creating needed to be small and not be cluttered with anything unnecessary. The name came naturally -- Most Minimal Static Site Generator (MMSSG).

You create two html templates. One for the index page, and one to use for each blog post. You can fill them in with whatever sort of website that you want, and you just need a couple of template tags to make it work. The index page needs a tag for where to put the list of posts, and each page needs a tag for the title, date, and post content. That's it!

This solution works really well for me, and I hope it'll help you too. The code is all available, with instructions of how to get started at [https://github.com/jacobkania/mmssg](https://github.com/jacobkania/mmssg).