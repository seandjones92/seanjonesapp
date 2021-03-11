+++
draft=true
title = "Putting a site on Netifly"
date = "2021-03-10"
author = "Sean"
cover = "img/putting-a-site-on-netifly/putting-a-site-on-netifly-cover.png"
coverAlt = "Netlify can host your Hugo site with CDN, continuous deployment, 1-click HTTPS, an admin GUI, and its own CLI"
description = "Putting a Hugo site up on Netifly for the first time"
+++

Now I'll be doing my best to get this Hugo site up just by following [this documentation](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/).

## Starting out

To get started I first had to create an account with Netifly. This is pretty straight forward as they allow you to just sign in and get started with your GitHub profile. Once I did that I was greeted with the following dashboard:

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 1.png" alt="Netifly Dashboard" position="center" style="border-radius: 8px;" >}}

I already like how upfront this service is about how much of my resources I've used for the month. The next step was to see if my site would build. I hit the "New site from Git" button and started working through the deployment wizard. After stepping through GitHub integration again (not sure why this is a separate step as my account is already created by leveraging my GitHub profile) and selecting the repository I wanted to build from I was very pleasantly surprised that the wizard was able to pickup the correct build settings automatically:

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 2.png" alt="Create New Site Wizard for Netifly" position="center" style="border-radius: 8px;" >}}

Since the wizard finished up the final form for me I was feeling pretty luck and jumped straight to the "Deploy site" button!

## A bump in the road

Once you hit the deploy button Netifly will clone your selected repository and attempt to build it. At first it looked like I was going to be lucky and have everything just work as expected. Almost immediately though my build command failed as my selected theme was not compatible with the version of Hugo that Netifly was using and the below error was thrown:

```sh
5:16:52 PM: ────────────────────────────────────────────────────────────────
5:16:52 PM:   1. Build command from Netlify app
5:16:52 PM: ────────────────────────────────────────────────────────────────
5:16:52 PM: ​
5:16:52 PM: $ hugo
5:16:52 PM: ERROR 2021/03/11 00:16:52 HELLO-FRIEND theme does not support Hugo version 0.54.0. Minimum version required is 0.74
5:16:52 PM: Building sites … ERROR 2021/03/11 00:16:52 render of "Posts" failed: "/opt/build/repo/themes/hello-friend/layouts/_default/rss.xml:5:18": execute of template failed: template: _default/rss.xml:5:18: executing "_default/rss.xml" at <$pctx.RegularPages>: can't evaluate field RegularPages in type *hugolib.PageOutput
ERROR 2021/03/11 00:16:52 render of "Sean Jones" failed: "/opt/build/repo/themes/hello-friend/layouts/_default/rss.xml:9:19": execute of template failed: template: _default/rss.xml:9:19: executing "_default/rss.xml" at <.Site.Config.Service...>: can't evaluate field RSS in type services.Config
ERROR 2021/03/11 00:16:52 render of "Categories" failed: "/opt/build/repo/themes/hello-friend/layouts/_default/rss.xml:9:19": execute of template failed: template: _default/rss.xml:9:19: executing "_default/rss.xml" at <.Site.Config.Service...>: can't evaluate field RSS in type services.Config
Total in 28 ms
5:16:52 PM: Error: Error building site: failed to render pages: render of "Tags" failed: "/opt/build/repo/themes/hello-friend/layouts/_default/rss.xml:9:19": execute of template failed: template: _default/rss.xml:9:19: executing "_default/rss.xml" at <.Site.Config.Service...>: can't evaluate field RSS in type services.Config
5:16:52 PM: ────────────────────────────────────────────────────────────────
5:16:52 PM:   "build.command" failed
5:16:52 PM: ────────────────────────────────────────────────────────────────
5:16:52 PM: ​
5:16:52 PM:   Error message
5:16:52 PM:   Command failed with exit code 255: hugo
5:16:52 PM: ​
5:16:52 PM:   Error location
5:16:52 PM:   In Build command from Netlify app:
5:16:52 PM:   hugo
```

So my theme requires at least Hugo version 0.74 and Netifly was attempting to build it with version 0.54.0. When I went to double check my local Hugo install I saw that I was running 0.81.0. I needed to find a way to get everything building in a consistent way.

Luckily there was already [documentation](https://www.netlify.com/blog/2017/04/11/netlify-plus-hugo-0.20-and-beyond/) on how to handle this exact issue!. I followed the steps outlined and set my desired build version in `netifly.toml` which I had created at the root of my repository. I commit this change and pressed the button asking for another deployment attempt. This also failed with the same error. For some reason the newly created configuration file wasn't getting picked up.

After some more searching I found that I can instead configure this as a variable in Netifly directly.

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 3.png" alt="Netifly Environment Variables" position="center" style="border-radius: 8px;" >}}

I tried once again to get a deployment off the ground. This time the environment variable method did the trick and the site was built and deployed in seconds! The only problem now is the name that it was hosted with is not great:

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 4.png" alt="My first hosted build on Netifly" position="center" style="border-radius: 8px;" >}}

## What's in a name?

Now that the site is building and deploying without issue, I need to clean up the domain name. The wizard that steps you through this process is once again very easy and took almost all the guess work out of the process.

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 5.png" alt="Netifly Domain Management" position="center" style="border-radius: 8px;" >}}

Once I hit the "Add custom domain" button I supplied the domain name I wanted to use, told the wizard I'd like it to handle DNS configuration for me, and then it gave me a list of DNS Name Servers to add to my Google Domains configuration which now looks like this:

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 9.png" alt="Google Domains Name Server configuration screen" position="center" style="border-radius: 8px;" >}}

Once the DNS configuration propagated my Certificate was issued by [Let's Encrypt](https://letsencrypt.org) and I was able to access my site by the proper domain name

{{< image src="/img/putting-a-site-on-netifly/putting-a-site-on-netifly 12.png" alt="Final deployment with custom domain applied" position="center" style="border-radius: 8px;" >}}
