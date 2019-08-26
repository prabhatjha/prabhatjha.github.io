---
layout: post
title:  "How to setup your own blog using Jekyll on Github"
tags: [howto]

---

I am not new to blogging. My blog on [Blogspot](https://prabhatkjha.blogspot.com) is still "active", I have experimented with Wordpress, Medium, HubSpot and used several other platforms powering blogs at my employers. They all either seem bloated and are bloated because some require a database, some are not suited for engineering blog. A good engineering blog often requires code highlights and usually don't need bells and wishtles you often see on corporate blogs. 

After exploring several alternatives and looking at blogs of sotware professionals I enjoy I settled on `Jekyll` powered blog on Github. [Documentations from Github](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll) are great but I still needed to do interweb search to get end to end setup working with https and custom domain. In true Open Source Software spirit here is a simple howto explaining the steps needed.

### First, pros of `Jekyll` and `Github Pages` :

* No database needed. No server needed.
* Easy to setup local development environment 
* Use all your `Git` and `Github` knowledge
* Jekyll has minimalistic theme which is great for text heavy writeups
* Don't need to know fancy `CSS` and `Javascript` to have a clean and well organized blogs
* Amazing community to experiment with different plugins and themes
* Get a SSL cert from Github to serve traffic under `https`. 

### Here are the steps:

1. **Install and configure `ruby` and `jekyll`**

	Best is to go straight to the source at [https://jekyllrb.com/docs/installation/macos/](https://jekyllrb.com/docs/installation/macos/)
	* ` jekyll new blog` which creates a directory called `blog` on your localhost. You can call this directly anything you want.
	* `jekyll build` 
	* `jekyll serve` 

	`jekyll serve --trace` if you want to see more details of any errors.
	This runs a local webserver at `http://localhost:4000` where you can see a running blog with default theme.


	* Customize default installation

	- Update `_config.yaml` with your name and social media coordinates. Any changes in this file requires a restart of jekyll process so make sure to `ctrl+c` and restart by `jekyll serve` from the `blog` directory.
	- Update about.markdown to tell the world about you.

	* Add a new blog 

		Jekyll requires blog post files to be named according to the following format under the directory `_posts`:

		`YEAR-MONTH-DAY-title.MARKUP`

		Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file.

2. **Github project setup**

	* Create a new repo on your Github 
		The repo name needs to follow  `{github.user.name}.github.io`. For me, it's `prabhatjha.github.io`
	* Setup this repo as remote origin on your `blog` project and push your code .
		Example: 

		`git remote add origin git@github.com:prabhatjha/prabhatjha.github.io.git`

		`git push -u origin master`
	* Your new blog should be available at http://`{github.user.name}.github.io`

3. **Custom Domain**

	* I use `godaddy.com` for my domain name where I have bought `prabhatkjha.com` . I could not get `prabhatjha.com` :-( 
	* Under GoDaddy's DNS Management interface add following `A` records. The IP addresses point to Github's IPs reserved for github pages.



	![GoDaddy A Record](/assets/godaddy-a-records-github.png)

	* Add a `CNAME` for `www` subdomain pointing to your github page.

		`cname 	www 	prabhatjha.github.io` 

	* Go to settings page for your new repo and github and follow the setup for custom domain connection. Github has pretty good [documents with screenshots.](https://help.github.com/en/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site). This process adds a new file `CNAME` in your repo.
	* Keep in mind that DNS resolutions take some time so you have to be patient. For me changes would take about 5-10 minutes before they were working.
	* For SSL configuration it took Github about 4 hours before cert was generated. It's important that your A records and CNAME are setup properly at your registrar otherwise Github won't issue a certificate. 


That's it. With these steps I have https://prabhatkjha.com working. After all you are reading this blog. ;-)