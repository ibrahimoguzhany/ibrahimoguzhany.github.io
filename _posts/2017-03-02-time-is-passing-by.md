---
id: 4229
title: Time is passing by
date: 2017-03-02T00:00:07+00:00
author: gbotweb
excerpt: Every selector has the potential to have unintended side effects by targeting unwanted elements or clashing with other selectors. More surprisingly, our selectors may even lose out in the global specificity war, ultimately having little or no effect on the page at all. Any time we make a change to a CSS file, we need to carefully consider the global environment in which our styles will sit. No other front end technology requires so much discipline just to keep the code at a minimum level of maintainability.
layout: post
guid: https://undsgn.com/uncode/?p=4229
permalink: /2017/03/02/time-is-passing-by/
slide_template:
  - ""
vc_teaser:
  - 'a:2:{s:4:"data";s:115:"[{"name":"title","link":"post"},{"name":"image","image":"featured","link":"none"},{"name":"text","mode":"excerpt"}]";s:7:"bgcolor";s:0:"";}'
categories:
  - Business
tags:
  - Travel
  - Tech
  - Life
  - Sport
---
CSS selectors all exist within the same global scope. Anyone who has worked with CSS long enough has had to come to terms with its aggressively global nature — a model clearly designed in the age of documents, now struggling to offer a sane working environment for today’s modern web applications. Every selector has the potential to have unintended side effects by targeting unwanted elements or clashing with other selectors. More surprisingly, our selectors may even lose out in the global specificity war, ultimately having little or no effect on the page at all.

Any time we make a change to a CSS file, we need to carefully consider the global environment in which our styles will sit. No other front end technology requires so much discipline just to keep the code at a minimum level of maintainability. But it doesn’t have to be this way. It’s time to leave the era of global style sheets behind.

_It’s time for local CSS._

> In other languages, it’s accepted that modifying **the global environment** is something to be done rarely, if ever.

In the JavaScript community, thanks to tools like Browserify, Webpack and JSPM, it’s now expected that our code will consist of small modules, each encapsulating their explicit dependencies, exporting a minimal API.

Yet, somehow, CSS still seems to be getting a free pass.

Many of us — myself included, until recently — have been working with CSS so long that we don’t see the lack of local scope as a problem that we can solve without significant help from browser vendors. Even then, we’d still need to wait for the majority of our users to be using a browser with proper Shadow DOM support.

We’ve worked around the issues of global scope with a series of naming conventions like **OOCSS**, **SMACSS**, **BEM** and **SUIT**, each providing a way for us to avoid naming collisions and emulate sane scoping rules.

We no longer need to add lengthy prefixes to all of our selectors to simulate scoping. More components could define their own foo and bar identifiers which — unlike the traditional global selector model—wouldn’t produce any naming collisions.

<pre><strong>import</strong> styles <strong>from</strong> './MyComponent.css';
<strong>import</strong> React, { Component } <strong>from</strong> 'react';
<strong>export default class</strong> MyComponent <strong>extends</strong> Component {
 <strong>render</strong>() {
    return (
      &lt;div&gt;
        &lt;div className={<strong>styles.foo</strong>}&gt;<strong>Foo</strong>&lt;/div&gt;
        &lt;div className={<strong>styles.bar</strong>}&gt;<strong>Bar</strong>&lt;/div&gt;
      &lt;/div&gt;
    );
  }</pre>

The benefits of global CSS — style re-use between components via utility classes, etc. — are still achievable with this model. The key difference is that, just like when we work in other technologies, we need to explicitly import the classes that we depend on. Our code can’t make many, if any, assumptions about the global environment.

> Writing maintainable CSS is now encouraged, not by **careful adherence to a naming convention**, but by style encapsulation during development.

Once you’ve tried working with local CSS, there’s really no going back. Experiencing true local scope in our style sheets — in a way that works across all browsers— is not something to be easily ignored.

Introducing local scope has had a significant ripple effect on how we approach our CSS. Naming conventions, patterns of re-use, and the potential extraction of styles into separate packages are all directly affected by this shift, and we’re only at the beginning of this new era of local CSS.

<pre>process.env.NODE_ENV === 'development' ?<strong>
    '[name]__[local]___[hash:base64:5]</strong>' :
    '<strong>[hash:base64:5]'
</strong>)</pre>

Understanding the ramifications of this shift is something that we’re still working through. With your valuable input and experimentation, I’m hoping that this is a conversation we can have together as a larger community.

_Note: Automatically optimising style re-use between components would be an amazing step forward, but it definitely requires help from people a lot smarter than me._