---
layout: "post"
title: "Configuring Blog  Note"
categories: blog
permalink: /:categories/blog_config
---

## Installation

1. Check whether ruby and gem are installed in your machine.
2. Install jekyll by gem using command: gem install jekyll bundler

## Build Webiste

1. Change to the directory where you want to store your website folders.
2. Type command in the terminal to create the website: jekyll new "Blog Name" // "Blog Name " could be: chris_blog
3. Host the website: bundle exec jekyll serve

## Publish Blog

1. Create the github repository which is used for hosting the blog.
2. Change to the gh-page branch and push all the content for the blog into this branch.
3. Online version should change the base url to be shuaimin_blog whereas local version should delete the 
shuaimin_blog part in the _config.yml file.
