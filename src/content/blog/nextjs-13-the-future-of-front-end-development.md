---
title: "Next.js 13: The Future of Frontend Development"
description: "This blog post is a summary of the new features and improvements introduced in Next.js 13, the latest version of the React framework for building dynamic web applications. It covers how to use RSC, Image component, font system, dynamic HTML streaming and Turbopack in Next.js 13 projects. It also provides links to articles and documentation for more details and examples."
pubDate: "Aug 18 2023"
heroImage: "/nextjs-13-blog-post.png"
---

Next.js 13 is the latest version of Next.js, a React framework for building fast and modern web applications. It introduces some significant changes and features that make frontend development easier and more powerful than ever. In this post, we will explore some of the highlights of Next.js 13 and show you how to use them in your own projects.

## The app/ directory

One of the biggest features of Next.js 13 is the new `app/` directory structure. This allows you to organize your pages, components, layouts, and data fetching logic in a more intuitive and flexible way. You can also use React Server Components (RSC) inside the `app/` directory, which enable you to write components that can run on both the server and the client.

The `app/` directory has three subdirectories: `pages/`, `components/`, and `data/`. The `pages/` directory is where you put your page components that define your routes. The `components/` directory is where you put your reusable UI components that can be imported by any page component. The `data/` directory is where you put your data fetching functions that can be used by any component.

For example, let’s say you have a blog application with two pages: `index.js` and `posts/[slug].js`. You also have a layout component called `Layout.js` that wraps around every page component. And you have a data fetching function called `getPosts.js` that fetches a list of posts from an API.

Your `app/` directory would look something like this:

```
app/
├── pages/
│ ├── index.js
│ └── posts/
│ └── [slug].js
├── components/
│ └── Layout.js
└── data/
└── getPosts.js

```

To use RSC inside the `app/` directory, you need to add a `‘use client’`; directive at the top of your file. This tells Next.js to convert your component into a client-side component that can use suspense for data fetching.

For example, let’s say you have a `PostList.jsx` component that renders a list of posts using `getPosts` as its data source. You can write it as an RSC like this:

```javascript
// app/components/PostList.jsx

"use client";

import { useServerData } from "next/server-data";
import Link from "next/link";
import getPosts from "../data/getPosts";

export default function PostList() {
  const posts = useServerData(getPosts);

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.slug}>
          <Link href={`/posts/${post.slug}`}>
            <a>{post.title}</a>
          </Link>
        </li>
      ))}
    </ul>
  );
}
```

Then you can import it in your index page like this:

```javascript
// app/pages/index.jsx

"use client";

import Layout from "../components/Layout";
import PostList from "../components/PostList";

export default function Home() {
  return (
    <Layout title="Home">
      <h1>Welcome to my blog</h1>
      <PostList />
    </Layout>
  );
}
```

The advantage of using RSC is that they allow you to write components that can fetch their own data without blocking the rendering of other components. They also enable streaming HTML responses from the server, which improves performance and user experience.

You can learn more about the `app/` directory and RSC in [this article](https://blog.logrocket.com/next-js-13-new-app-directory/) or check out [the documentation](https://beta.nextjs.org/docs/routing/fundamentals#the-app-directory).

## The Image component

Another feature of Next.js 13 is the improved Image component. This component allows you to easily display images without layout shift and optimize files on-demand for increased performance.

The Image component automatically handles resizing, cropping, optimizing, lazy loading, and serving images in different formats (such as WebP) based on browser support. It also supports placeholder images and blur-up effects for better user experience.

To use the Image component, you need to import it from `‘next/image’` and pass it an src prop with the image URL. You can also pass other props such as _width_, _height_, _layout_, _quality_, _placeholder_, _blurDataURL_ etc.

For example,

```javascript
// app/components/ImageExample.jsx

'use client';

import Image from 'next/image';

export default function ImageExample() {
  return (
    <div>
      <Image
        src="/images/cat.jpg"
        alt="A cute cat"
        width={300}
        height={200}
        layout=“responsive”
        placeholder=“blur”
        blurDataURL=“/images/cat-blur.jpg”
        />
    </div>
    );
  }
```

You can learn more about the Image component and its props in [this article](https://nextjs.org/blog/next-13#image-component) or check out [the documentation](https://nextjs.org/docs/api-reference/next/image).

## The font system

Next.js 13 also introduces a new font system that automatically optimizes your fonts, including custom fonts. It removes external network requests for improved privacy and performance, and it self-hosts any font file for you. It also prevents layout shift by using the CSS `size-adjust` property.

To use the font system, you need to install `@next/font` as a dependency and add it to your `next.config.js` file. You can also configure some options such as _domains_, _formats_, _variable_ etc.

For example,

```javascript
// next.config.js

const withFont = require("@next/font");

module.exports = withFont({
  font: {
    // Enable font optimization
    optimize: true,
    // Enable Google Fonts optimization
    google: true,
    // Enable local fonts optimization
    local: true,
    // Specify custom domains to fetch fonts from
    domains: ["example.com", "fonts.com"],
    // Specify custom formats to use for fonts
    formats: ["woff", "woff2"],
    // Use CSS variables for fonts
    variable: true,
  },
});
```

Then you can use any Google or local font in your CSS files by importing them from `@next/font/google` or `@next/font/local`.

For example,

```javascript
/* app/styles/global.css */

@import '@next/font/google/css?family=Roboto';
@import '@next/font/local/css?family=Open+Sans';

body {
  font-family: 'Roboto', sans-serif;
}

h1 {
  font-family: 'Open+Sans', serif;
}
```

You can learn more about the font system and its options in [this article](https://blog.logrocket.com/next-js-font-optimization/) or check out [the documentation](https://nextjs.org/docs/basic-features/font-optimization).

## Dynamic HTML streaming

Another feature of Next.js 13 is dynamic HTML streaming support. This means that Next.js can stream HTML responses from the server as they are generated, instead of waiting for the whole page to be rendered. This improves performance and user experience by reducing time to first byte (TTFB) and time to interactive (TTI).

Dynamic HTML streaming works automatically when you use RSC inside the `app/` directory. You don’t need to do anything else to enable it. However, if you want to use dynamic HTML streaming without RSC, you can do so by using a new API called `next/stream`.

the `next/stream` API allows you to create streamable components that can be used inside any page component. You can also use suspense for data fetching inside streamable components.

To use `next/stream`, you need to import it from `‘next/stream’` and wrap your component with `stream()`. You can also pass some options such as `fallback` etc.

For example,

```javascript
// app/components/StreamExample.jsx

"use client";

import { stream } from "next/stream";
import { useServerData } from "next/server-data";
import getUser from "../data/getUser";

function StreamExample() {
  const user = useServerData(getUser);

  return (
    <div>
      <h1>Hello {user.name}</h1>
      <p>Your email is {user.email}</p>
      <p>Your role is {user.role}</p>
    </div>
  );
}

export default stream(StreamExample);
```

Then you can import it in your page component like this:

```javascript
// app/pages/about.jsx

"use client";

import Layout from "../components/Layout";
import StreamExample from "../components/StreamExample";

export default function About() {
  return (
    <Layout>
      <h1>About</h1>
      <p>This is a demo page using dynamic HTML streaming.</p>
      <StreamExample />
    </Layout>
  );
}
```

You can learn more about dynamic HTML streaming and `next/stream` in [this article](https://nextjs.org/blog/next-13#dynamic-html-streaming) or check out [the documentation](https://nextjs.org/docs/advanced-features/dynamic-html-streaming).

## Turbopack

The last feature of Next.js 13 that we will cover in this blog post is Turbopack. Turbopack is an incremental bundler optimized for JavaScript and TypeScript, written in Rust, and built into Next.js 13. It updates up to 700x faster than Webpack on large applications.

Turbopack can be used in Next.js 13 in both the pages and app directories. You just need to create a next.config.js file with turbopack: true option.

For example,

```javascript
// next.config.js

module.exports = {
  turbopack: true,
};
```

Then you can use any JavaScript or TypeScript file as your page component or custom app component. Turbopack will automatically bundle them for you.

Turbopack also supports some configuration options such as alias, extensions etc. You can check them out in [the documentation](https://beta.nextjs.org/docs/configuring/turbopack).

Turbopack is still in alpha stage and may not support all Next.js features yet. However, it is a promising technology that will improve both local development and production builds in the future.

You can learn more about Turbopack and its benefits in [this article](https://nextjs.org/blog/next-13#turbopack) or check out [the documentation](https://nextjs.org/docs/advanced-features/turbopack).

## Conclusion

Next.js 13 is a huge release that brings many new features and improvements to the framework. It enables you to be dynamic without limits by using RSC, Image component, font system, dynamic HTML streaming and Turbopack.

We hope you enjoyed this blog post and learned something new about Next.js 13. If you want to try it out yourself, you can create a new project with `npx create-next-app` or upgrade your existing project with `npm install next@latest`.

Thank you for reading! 🙏
