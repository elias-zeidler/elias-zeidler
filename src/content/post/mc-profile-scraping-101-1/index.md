---
title: "Maintaining one of the largest Minecraft profile indexes 101"
description: "This post is for testing and listing a number of different markdown elements"
publishDate: "24 Mar 2024"
updatedDate: 24 Mar 2024
tags: ["minecraft", "mojang", "hobby", "private"]
---

## Context
Alright, so before I'm going to start with this post, I want to give you a little bit of context. I'm a huge fan of Minecraft and I've been playing it since 2014.

Essentially, Minecraft accounts are stored in a database and are accessible through the Mojang API. Using this API returns data such as the account's UUID, username, current skin and cape. In order to access this data, you need either input the username or UUID of the account you want the data from.

## Introduction
Doing some math (which I'll probably explain in a future post), I've calculated that there are roughly around 61 million Minecraft accounts. This is a rough estimate, but it's a good starting point.

My initial idea was to create a system that tries to enter a lot of different usernames into the Mojang API and store the returned data (if any) in my own database. This way, I could create a huge index of Minecraft accounts.
Though, it doesn't seem as easy as it sounds. The Mojang API has a few restrictions and it's not possible to get data from all 61 million accounts in a short amount of time with just a simple program.

## Problems
### Problem 1: Rate limit
The first problem I encountered was the rate limit (which nowadays is dynamic). This means that you can only send a specific amount of requests to the API within a given timespan. If you exceed this limit, you'll get a 429 status code (too many requests) and you'll have to wait for a certain amount of time before you can send more requests.
This should theoretically protect the API from being abused, which is a good thing for Mojang.

From a few of my friends who are working on similar projects, I've heard that they tend to use proxies to bypass this rate limit. But my alarm bells rang when I heard this, because proxies are known for a. being slow and b. being unbelievably expensive.

The solution? Our beloved IPv6!
Since IPv6 is 128-bit, there are 2^128 possible addresses. This means that providers (ISPs and hosting companies) tend to sell IPv6 /64 (2^64 addresses) or /48 (2^80 addresses) blocks for a low price (sometimes even completely free). This way, you can configure your server to use a different IPv6 address for each request or just allow you to freely set the TCP source address. This way, you can cycle through all of your IPv6 addresses and not get rate limited (since the limit is based on the IP address).

### Problem 2: Server power
The second problem I encountered was the server power. Since I'm planning to request a lot of data from the Mojang API within an acceptable amount of time, I need a server that can handle this.

I thought about this issue for a while before I finally realized that the only performance bottleneck isn't just the server itself, but also the code that's running on it. I've seen a lot of people using Python, JS and even Java for this kind of project. I don't want to say that these languages are bad or just generally slow, but they're not the best choice for the speed I'm looking for. It's also not just the language itself, but also runtime overhead and the way the code is written.

My first approach was to use C++ for this project, but I eventually decided to use Rust, as I wasn't that experienced with C++. I'm not even that experienced with Rust either, but I had a good feeling about it (+ one of my friends offered to help me with it).

This, uhh... wasn't the best idea either. We had a lot of struggles and didn't get the performance we were looking for (due to our code, not Rust itself - and the motivation to continue this project in Rust was gone).

One of my friends texts me frequently about how he adores Go and that I should try it out. I've heard a lot of good things about Go, but I never really thought about learning it. But since I was desperate for a solution, I decided to give it a try. Fortunately, I was able to learn and use Go in a short amount of time and I was able to get the performance I was looking for.

## Conclusion

I'm currently running a server that's requesting data from the Mojang API and storing it in my own database.
With my current speed, it's possible for me to update the data of roughly 59,500,000 accounts every 2.5 hours.

I'm planning to write a post about the architecture of this system and how I've set it up. I'm also planning to write a post about the math I've done to calculate the amount of Minecraft accounts.

This blog post rather serves as a first entry to this series of posts hence my it's not as detailed as the upcoming posts will be. Stay tuned!