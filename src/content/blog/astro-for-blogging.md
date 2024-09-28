---
title: 'Astro for Blogging'
description: 'Getting this site set up technically'
pubDate: 'Sep 28 2024'
heroImage: '/astro-logo-dark.svg'
tags: ['meta', 'code', 'fediverse']
---

The code for this site is at https://github.com/gnarg/everything-counts.

I knew I wanted a static site that I could host cheaply and easily. I decided on [Astro](https://astro.build) partly because it seemed well recommended on the internet, and partly because I just finished a couple static sites in [SvelteKit](https://kit.svelte.dev/). While SvelteKit definitely supports static sites, it seems like it offers a little resistance, like it really wants to be a full stack. Astro is similar, but more focused on being *only* a static site generator.

I started with the basic blog template described in the documentation `npm create astro@latest -- --template blog` but quickly decided I wanted to add tagging support.

The [Generate tag pages](https://docs.astro.build/en/tutorial/5-astro-api/2/) tutorial looks promising but I ran into trouble with the code given for getting a list of all posts.
```javascript
const allPosts = await Astro.glob('../posts/*.md');
```
The `post` objects returned are missing `url` attributes, so this code doesn't work at all.
```javascript
{posts.map((post) => <li><a href={post.url}>{post.frontmatter.title}</a></li>)}
```
The more idiomatic way to get posts seems to be this.
```javascript
const allPosts = await getCollection('blog');
```
Which then works fine to list posts like this.
```javascript
{posts.map((post) => <dt><a href={`/blog/${post.slug}/`}>{post.data.title}</a></dt><dd>{post.data.description}</dd>)}
```
All of this you can see [here](https://github.com/gnarg/everything-counts/blob/master/src/pages/tags/%5Btag%5D.astro).

Having gotten this working, I'm going to experiment with using [Bridgy](https://brid.gy) for [POSSE](https://indieweb.org/POSSE) syndication to [Mastodon](https://indieweb.org/POSSE).
