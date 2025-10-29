---
title: "Here We Go... Again!"
date: 2025-10-29
categories: 
  - "blog"
  - "projects"
  - "technology"
---
So... We meet again!

This has got to be the 5th time I have restarted this blog. I'm not proud of that, but it is what it is. Let me jot down some thoughts about that!

## "So, you made *another* blog?"

Yeah, yeah, yeah... Yes, I restarted this blog, again! I realize that I posted last year (a post that you can't read because I haven't imported everything from the old blog) about content consistency, and the self-imposed pressure of writing things. I don't generate a lot of content. I have four small children, and a full-time job, and a ton of hobbies and obligations. Things that I do are rarely 'produced' to the point where I'm confident sharing them via this medium. The effort involved with taking something that works in-person and adapting it to something that you can read, understand, and enjoy, is sometimes difficult, and usually always time consuming. I have tried to make regular posts, but honestly, I don't feel like I have that much to say. At least, not much that I think people would want to read. Again, not without work to massage it to convey the right information with the right tone.

## Why again?

It's a fair question. I have routinely stated that I struggle with content consistency because of me spending my time on other things. It isn't mandated that everyone have a blog. Are blogs even cool anymore? Were they ever?

Since 2024 I have been helping with the [PowerShell + DevOps Global Summit's website](https://www.powershellsummit.org/). It used to be built on Wordpress, just like my previous blog. Recently, the decision was made to rebuild the website using Hugo and Markdown. For those that don't know, [Markdown](https://en.wikipedia.org/wiki/Markdown) is a markup-based document language and [Hugo](https://gohugo.io/) is a static site generator that uses Markdown. This allows us to write content in Markdown and have Hugo assemble that content into a fast static set of files for consumption. This is significantly different from how Wordpress handles things.

Wordpress is an entire content management system (CMS). It requires a database (MySQL/MariaDB), configuration, and updates, but the benefit is a complete, high-level usable product. There are  repos that allow one-click (or clost to it) installations of themes and plugins. Site configurations are available via menus. The post/page editor is full-featured, integrating richtext editing, images, videos, AI summarization, meta tags, etc... And that is before adding plugins!

## So, why leave Wordpress?

A while back I migrated my Wordpress blogs **TheRealGill.com** and [**Experience Points**](https://exppoints.com) from paid hosting to running on my local Unraid server. Part of this move was cost savings while part of it was to challenge myself to host my own things. Cloudflare handled routing and security, and *bang!* local sites delivered to **you**! I needed to host a database and a webserver, and provide routing for the sites to work, which is usually handled by the hosting company you pay, but this was technically trivial using Docker. So if it was easy and taken care of, why leave?

For one; Wordpress is a resource hog. Every time a request for a page is made, Wordpress needed to run database lookups and build that page dynamically. Perhaps I was too frugal with the resources I exposed to the containers running the site, but I spent considerable time and effort researching plugins to convert the dynamic content to static content so the pages would load faster. The benefits of having an entire CMS to provide easy content creation mattered less when I wasn't creating things frequently, and the overhead of the platforms that provided that rarely-used functionality slowed the site down.

For another; I live in Louisiana. We have bad weather here sometimes. Despite having a UPS on my server, it won't stay online forever. It will shut itself down when that UPS drops to 15% remaining. That is about 30 minutes on battery. When the server went down so did everything it hosted. And, the server went down for reasons that were not weather related! Internet outages, server upgrade issues, literally anything!!! I may have 92-95% uptime, but over the course of a year that is over 18 days of downtime at best. Abysmal.

I started a side-project and wanted to host it via Github Pages. GHP is a way to host websites via Github. The side-project never got off the ground, but eventually in helping with the Summit website I got to use the skills I acquired on that failed side-project, and it re-inspired me. Less maintenance, more uptime, simpler components. Why not?!

The really neat thing about this particular setup is that the site configuration and contents are all commited to Github source control. When changes are ready to be made public I can merge them into the main branch and a Github Action will automatically build the static version of the website and make it available.

## And nothing went wrong?

*"Oh sweet summer child!"* No, that would've been nice, but tons of things went wrong. I tried to migrate all content from the old blog to this one. Yeah, no. There are Wordpress plugins that claim to do the needful, but I couldn't find one that worked well enough. I tabled the migration efforts for a while. When I resumed work on it I found a node-based converter that got me most of the way there. The conversion created subdirectories with `year\month\post_title` formatted data. Wordpress had formatted things using `year\month\day\post_title` and if I went forward with the converted hierarchy all previous links I had posted to social media and anywhere else would have been broken. Thankfully, I was able to grab date data from the posts' file and with a little bit of PowerShell...

```powershell
$exportDir = "D:\tmp\export\"
$items = Get-ChildItem -Path (Join-Path -Path $exportDir -ChildPath 'posts' ) -Recurse -Filter *.md
foreach($item in $items) {
    $day = $item.Directory.Name.split('-')[2]
    $dayPath = Join-Path -Path $item.DirectoryName.Replace($item.DirectoryName.split('\')[7],"") -ChildPath $day
    if(-not (Test-Path -Path $dayPath) ){
        New-Item -ItemType Directory -Path $dayPath
    }
    Move-Item -Path $item.DirectoryName -Destination $dayPath
    (Join-Path -Path $dayPath -ChildPath $item.Directory.Name | Rename-Item -NewName $item.Directory.Name.Substring(11))
}
```

I was able to reorganize things to work appropriately. This was before I found out about the [`url` attribute in Hugo content's frontmatter](https://gohugo.io/content-management/urls/#url). 

Additonally, many of the images that I had used in my blog posts were hosted by the Wordpress engine, and needed to be re-linked. Since Wordpress used [Jetpack](https://jetpack.com/) there were a variety of CDN-based links that I needed to clean up.

And then there was Github Pages... Don't get me wrong, I get it now. But at the start it was just another *different* thing to throw on the pile of *different* things. Getting the actual site to build on commit was easy enough, but including the CNAME file and getting the domain to work was a bit of a pain. Turns out that when I was working on it Github just so happened to be experiencing some sort of outage that was affecting the domain verification process. I floundered for a bit before asking for help, and being told that the internet was just on-fire.

## This version looks like it is missing some content

Content creation joking aside, I didn't migrate everything over. All of the 2025 content is migrated over, and over time I will try to bring over the posts that still matter to me, but I doubt that all the content will make the cut. If I waited to release the new blog until everything had moved over, I don't know that it would ever see the light of day.

So for now, this is what we get. I think I have removed all of the barriers that I can. Time to get to writing!