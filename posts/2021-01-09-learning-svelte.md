---
Title: Learning Svelte
PublishedDate: 2021-01-09
---

I've been working on a new project in my free time for a little while now, and figured it was time to start sharing. If you couldn't guess, this project uses [Svelte](https://svelte.dev) - a newer frontend Javasript framework.

I feel like I only write blog posts when I'm really excited about learning something new for a project, but that's just how it goes, I guess...

Svelte is really neat. I have been talking for a long time about how I wished I could just write **reusable HTML components** with a few little `<script>` and `<style>` tags woven in, and include those in other HTML pages. Like reusable React components, but simpler. That's exactly what it feels like to write svelte code.

I love React in a lot of ways -- it's proven itself to be a robust technology that can scale to massive projects. Though, there is a lot of overhead, as I wrote about [here](https://jacobkania.com/2020-02-02-static-sites). It's proven effective, but it's also important to look to the future at what's coming next.

# Writing a svelte component

Have you ever written HTML? Congrats, you know the basics of svelte! I'm half kidding, but seriously, this is an example of a valid svelte component file, in its entirety:

```html
<!-- hello.svelte -->

<script>
  const x = "world!";
</script>

<h1>My homepage</h1>
<div>Hello {x}</div>
```

The beauty of it is that you can then just write another component and import your "hello" message:

```html
<!-- homepage.svelte -->

<script>
  import Hello from "./hello.svelte";
</script>

<div>
  <Hello />
  <span> And goodbye! </span>
</div>
```

And just like that, you have composable HTML. Of course, though, there's way more that you can do. The other biggest highlight is automatic updating of changes in the DOM. No more wondering when a component will re-render on state changes, it just happens automatically.

# Automatic state handling?

Svelte is able to do this because it works in a fundamentally different way to other frontend framworks. Svelte is a compiler instead of a runtime-framework like React. It _doesn't update a virtual DOM_, but instead pre-compiles our `.svelte` files into javascript. Svelte knows which parts of a component could change, and compiles functions to precisely change only what needs to be changed. The svelte blog has a great article that explains [why the virtual DOM is unnecessary for high-performance](https://svelte.dev/blog/virtual-dom-is-pure-overhead).

This greatly reduces overhead while developing, and performance is automatically fast, without all of the computation and diffing that you get from a virtual dom.

There are so many other topics that can be covered --

- Built-in support for global stores (no need for MobX or Redux)
- Excellent plugin support
- Unbelievably fast dev builds with [Snowpack](https://www.snowpack.dev)
- Seamless handling of async requests
- Easy handling of forms, with binding to input fields
- Built-in debugger tags as an alternative to `console.log`

I'll get to these as I keep learning-by-doing, and building this new mystery app! More details to come on that, too...
