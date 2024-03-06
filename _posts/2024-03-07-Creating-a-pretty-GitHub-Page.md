---
title: Creating a pretty Github page
author: chulmin
date: 2024-03-07 00:00:00 +0800
categories: [HOW TO START GITHUB BLOG for beginners, Setting up GitHub page]
tags: [Ruby, Jekyll, GitHub blg, beginner, html, blog, chirpy]
---

This guide is tailored for Windows 10 .

## Choosing a theme
The first step starts with selecting a theme from the following sites:

- <http://jekyllthemes.org/>
- <https://jekyllthemes.io/free>
- <http://themes.jekyllrc.org/>
- <https://github.com/topics/jekyll-theme>

Choose any theme that aligns with your purpose, whether it's for a name card, blog, or CV. I have chosen [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy), an appealing option for blogging.


## Creating a static website on your local computer
Let me proceed with [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy). Download the repository using "Download Zip" and extract it into the directory where you previously created "hello world!". Next, execute the following commands:

```ruby
bundle install
bundle update
bundle install
```
{: file='Start Command Prompt with Ruby'}

To generate a static website, run:

```ruby
bundle exec jekyll serve
```
{: file='Start Command Prompt with Ruby'}


This command executes the code implemented by the author in the directory. You'll notice the creation of directories such as '_site'. Now, type <http://localhost:4000/> into the address bar. Voila! Your website is now generated locally.

<br/>


## Customizing the generated website
Apparently, the website you have now is barebone; it is not yours. The first step is to modify:


- **_config.yml**

Simply input your information, and Jekyll will generate the website accordingly. Here's an example of what I've done:


```yml
# Change to your timezone › https://kevinnovak.github.io/Time-Zone-Picker
timezone: America/New_York

# jekyll-seo-tag settings › https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md
# ↓ --------------------------

title: Grand master Chulminator # the main title

tagline: CodeCraft Chronicles # it will display as the sub-title

description: >- # used by seo meta and the atom feed
  Posting programming skills and mechanics

# Fill in the protocol & hostname for your site.
# e.g. 'https://username.github.io', note that it does not end with a '/'.
url: "https://Chulminator.github.io"

github:
  username: Chulminator # change to your github username
```
{: file='./_config.yml'}

Additionally, make changes in these files:

- **./assets/css/jekyll-theme-chirpy.scss**
- **./_include/*.html**
- **./_sass/typography-light.scss**

These are the things I adjusted for my own customization. For instance, the default setting for the left side bar was in order of photo, title, and some buttons. I rearranged the elements by changing '**./_include/sidebar.html**' from:

```html
<a href="{{ '/' | relative_url }}" id="avatar" class="rounded-circle">
  {%- if site.avatar != empty and site.avatar -%}
    {%- capture avatar_url -%}
      {% include img-url.html src=site.avatar %}
    {%- endcapture -%}
    <img src="{{- avatar_url -}}" width="112" height="112" alt="avatar" onerror="this.style.display='none'">
  {%- endif -%}
</a>

<h1 class="site-title">
  <a href="{{ '/' | relative_url }}">{{ site.title }}</a>
</h1>
<p class="site-subtitle fst-italic mb-0">{{ site.tagline }}</p>
```
{: file='./_include/previous_sidebar.html'}

to:

```html

<h1 class="site-title">
  <a href="{{ '/' | relative_url }}">{{ site.title }}</a>
</h1>
<p class="site-subtitle fst-italic mb-0">{{ site.tagline }}</p>

<a href="{{ '/' | relative_url }}" id="avatar" class="rounded-circle">
      {%- if site.avatar != empty and site.avatar -%}
        {%- capture avatar_url -%}
          {% include img-url.html src=site.avatar %}
        {%- endcapture -%}
        <img src="{{- avatar_url -}}" width="112" height="112" alt="avatar" onerror="this.style.display='none'">
      {%- endif -%}
    </a>
```
{: file='./_include/sidebar.html'}

## Conclusion

I hope you can successfully create your customized website. If you have any questions, feel free to contact me.


## reference
<https://wlqmffl0102.github.io/categories/github-blog/>