---
title: Exploring News Through Data
summary: Insightful Journalism Meets Analytics
tags:
  - NLP
  - Data Journalism
  - Breaking Italy
  - YouTube Data API
  - Sentiment Analysis
date: '2023-08-15T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: Breaking Italy keyword cloud
  focal_point: Smart
  width: 100
  height: 150

links:
  - icon: github
    icon_pack: fab
    name: Project Code
    url: https://github.com/MauroCE
  - icon: twitter
    icon_pack: fab
    name: Follow
    url: https://twitter.com/MauroCamaraE
url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: example
---

Breaking Italy has been my favorite infotainment YouTube channel since I was in high school. It's one of the largest YouTube communities in the Italian scene, and it features a mix of news and opinions. What I love the most about the channel is that Alessandro Masala, the creator and host of the show, always makes an effort to provide the backstory for the news, and this allows you to have an eagle-eye on what is happening in the world. Alessandro also provides a wealth of references that any user can use to read more about each topic discussed in the video.

I have often found myself looking for a reference, be it a newspaper article or a report, that I wanted to read again but I was unable to because I had no way to quickly search through the thousands of videos Breaking Italy has posted over the years. This presented me with a great opportunity to try the YouTube Data API and use my coding skills. At the time, I was mostly interested in finding a Gender Gap report that Alessandro talked about in one of his videos.


### What's in the Mix
I rolled up my sleeves and jumped into the data behind the scenes. With 1068 videos in the mix, I took the YouTube Data API for a spin, snagging titles, keywords, and descriptions. Here's three tasks I worked on:

1. **Word Cloud Synthesis**: Ever felt the pulse of news and info? I certainly have. I sifted through the keywords, ditching the fluff, and crafted a word cloud that captures the essence of those videos in a single captivating visual. [Give it a go](https://colab.research.google.com/drive/16yfS7oRSjJbhcZGoKDyWco1rn2pBEiXu?usp=sharing).

2. **Crafting Connections**: I extracted URLs from video descriptions and wove them together with keywords, titles, and descriptions. The result? A dynamic dictionary that helps you navigate through a sea of knowledge, making connections like a true journalist. Hold tight as we map the trail of references scattered across video descriptions. Think of it as your guide to a labyrinth of insights, where every keyword holds a treasure trove of URLs. [Give it a go](https://maurocamaraescudero.netlify.app/breaking_italy.html) and [Try the code](https://colab.research.google.com/drive/1ldJPBgfyRRGr2QYfp8zg6Ar0PPefn3vr?usp=sharing).

3. **Sentiment Analysis and LDA**: COMING SOON.

**Search Made Smarter:**
No more sifting through haystacks. I built a search bar with HTML and JavaScript: just type in your keywords and let my program do the heavy lifting. The magic happens when URLs crisscross, giving you the good stuff without the noise.

### Why it Resonates with Me
I'm a data journalism enthusiast to the core, and Breaking Italy is one of my go-to for insights. However, I'd watch videos, get all pumped up, and months later, I'd struggle to find those golden nuggets buried in descriptions. So, I put my data science hat on and thought, "Why not blend my passions?". This was a genuine itch I needed to scratch. I dug deep into YouTube's data treasure trove to create something practical—something that helps not just me, but anyone who's ever found themselves lost in the vast sea of videos.
