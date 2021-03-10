+++
title = "Getting Started"
date = "1986-09-17"
author = "Sean"
cover = "img/getting-started.png"
coverAlt = "git clone of Sean Jones app in a terminal"
description = "First steps towards documenting a journey, creating a space to document"
+++

# The original plan

Start a github blog. mapped "seanjones.app" domain to github pages and got SSL setup. demo site ran great. installed jekyll and tried to build a site but kept getting errors:

```
vscode ➜ /workspaces/seandjones92.github.io (initial-setup) $ bundle exec jekyll serve --livereload
Configuration file: /workspaces/seandjones92.github.io/_config.yml
            Source: /workspaces/seandjones92.github.io
       Destination: /workspaces/seandjones92.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.992 seconds.
                    ------------------------------------------------
      Jekyll 4.2.0   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
Traceback (most recent call last):
        43: from /usr/local/bundle/bin/bundle:23:in `<main>'
...
...
         4: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/base.rb:42:in `each'
         3: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/base.rb:47:in `block in configure'
         2: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/linux.rb:34:in `_configure'
         1: from /usr/local/lib/ruby/gems/2.7.0/gems/listen-3.4.1/lib/listen/adapter/linux.rb:34:in `new'
/usr/local/lib/ruby/gems/2.7.0/gems/rb-inotify-0.10.1/lib/rb-inotify/notifier.rb:69:in `initialize': Function not implemented - Failed to initialize inotify (Errno::ENOSYS)
```

Since this was an issue with `inotify` I assumed it was because I had not configured `/proc/sys/fs/inotify/max_user_instances` to allow enough instances, which I had seen before a while back.

after looking at the [Docker Desktop](https://www.docker.com/docker-community) Slack channel it appeared that I was not the only one having trouble with Jekyll and the new M1 macs. As it turns out there seems to be a C extention for the `inotify` gem that doesn't really work with the M1 Macs yet.

Instead of fighting with this much more I decided to try finding a site generator that _would_ work with my machine. I landed on Hugo since it seemed like a straight forward and simple solution and was built with Golang, which as of version 1.16 now supports the M1 chips.

I've now replaced Jekyll with Hugo as my site generator but that's only part of the puzzle. Jekyll is pretty much made to be used with Github pages for hosting, which is free and easy. But since I'm trying a new generator I figured I might as well look at a new hosting solution.

After some research (and seeing that Hugo provides docs on how to do it) I landed on Netifly. It has a free tier which includes "serverless functions" which GitHub does not. So already it's a better value for someone trying to build a cloud based lab while spending as little cash as possible.

The next post will go over what it was like for me to get this site up and running with my custom domain and SSL on netifly. I'll also do some initial exploration of the netifly platform and write about it a bit.
