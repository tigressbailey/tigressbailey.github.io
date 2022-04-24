---
title: Migrate my blog from Jekyll to GatsbyJS and life
date: "2022-04-24T22:40:32.169Z"
template: "post"
draft: false
slug: "migrate-my-blog-from-jekyll-to-gatsbyjs"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
  - "Life"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

I've been using Jekyll for years.
It is quite popular and convenient.

As I've build a GatsbyJS site for my company last year, I've decided to migrate from Jekyll to GatsbyJS.

The only key point is to migrate the content from the `_posts` folder to `content/posts` folder.

Gatsby enables developers to build fast, secure, and powerful websites using a React-based framework and innovative data layer that makes integrating different content, APIs, and services into one web experience incredibly simple.

It is indeed superfast. By employing the speed of Gatsby and the power of PWA, we will be able to build a mobile app like experience, where the user will feel like switching pages in a mobile app, rather than a website.

There are amazingly useful plugins that are available at Gatsby's official website, which we can npm instal or yarn add ( ways of installing packages to our project ). Some useful plugins are:

- gatsby-plugin-manifest : Make our site a installable and a PWA with this plugin
- gatsby-plugin-offline : Make our site run offline with this plugin.
- gatsby-plugin-google-analytics : Use Google analytics for our website with this plugin
- gatsby-remark-embed-youtube : Embed YouTube videos into our website
- gatsby-source-contentful: Integrate Headless CMS - Contentful to create pages for our website with this plugin
- gatsby-plugin-react-i18next: Use react-i18next to translate our website with this plugin
- gatsby-plugin-s3: Use S3 to upload our website to S3 with this plugin

There are a sufficient amount of GatsbyJs Starter repositories and templates that is as simple as cloning and changing the content of them. In this way we can completely develop websites like this blog, super quick and efficient. The documentation is super friendly, and I would definitely recommend you starting from there if you want to learn Gatsby.

GatsbyJS is using Jamstack architecture.
Jamstack is the culture of using JavaScript, APIs and Markup to build website. They mainly focus on how we can use these technologies to build websites that don't depend on managing servers, but decouple the front-end and the backend, supported by the CDN ( Content delivery Network ).
Building websites with Gatsby is again a way of following the Jamstack culture. This results in a better performance, increased security, cheap and easy scaling, better developer experience.

Since it is nothing but ReactJs itself, we can use the existing techniques and components of ReactJs here. The ReactJs community is powerful with a lot of existing components that are one click away. This is a huge advantage as we need not worry about how new GatsbyJs is.

We can use Markdowns in Gatsby and convert it into a blog based on Markdown. Everytime we need to write a new article, all we need to do is add a new .md file and push the code, that is it! How simple is that?

The use of GatsbyJs enables our websites to harness a lot of optimization techniques used by the internal code. This results in our website being ranked higher on search engines. There are also a lot of plugins that can help we boost this.

And Graphql, it support Graphql natively and quite easy to use.

From my perspective, GatsbyJS is a great framework for building static websites.

---

I've been working remotely over 1 year.

I just feel this is really legit for me.

I spend more time with my family, and all the energies and inspirations are increasing in my head and body.

I learn Flutter early this year and successfully build a feature using it.

And I plan to learn more Web 3.0 technologies like Ether.js, Web3.js, Truffle, Solidity, and more.

---

Attached some screenshots of my old website for memorization.


- Desktop View
![Desktop](/media/screenshot-tigressbailey.github.io-2022.04.23-20_23_01.png)

- Mobile View
![Mobile](/media/screenshot-tigressbailey.github.io-2022.04.23-20_23_30.png)
