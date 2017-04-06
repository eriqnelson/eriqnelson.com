+++
title = "Tumblr To Hugo"
weight = 20
draft = false
+++
## Here’s my Problem

While building this website to host my blog I thought it might be nice to pull a bunch of my older illustrations off of Tumblr and rehost them in a section here. I’m using Hugo to build this site and so I’ll need to do some conversions to the source files so they’ll render correctly.


From the hugo docs site I cloned the repo for [tumblr-to-hugo](https://github.com/jipiboily/tumblr-to-hugo) locally and after some initial reading I made a run that gave me an error message.


```
$ ruby t2h.rb APIKEY eriqgnelson.tumblr.com
/usr/local/Cellar/ruby/2.0.0/lib/ruby/2.0.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- pry (LoadError)
        from /usr/local/Cellar/ruby/2.0.0/lib/ruby/2.0.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from t2h.rb:2:in `<main>'
```
Alright, that’s not great. What version of Ruby is installed?


`ruby -v`


I get back

```
ruby 2.0.0p648 (2015-12-16 revision 53162) [universal.x86_64-darwin16]
```

That’s pretty out of date, let’s update ruby

`brew install ruby`

Then run the command again


`$ ruby t2h.rb APIKEY eriqgnelson.tumblr.com`


The same error message comes back, this time with a different revision
```
/usr/local/Cellar/ruby/2.4.1_1/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- pry (LoadError)
        from /usr/local/Cellar/ruby/2.4.1_1/lib/ruby/2.4.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from t2h.rb:2:in `<main>'
```


Well, that didn’t solve anything. Looking at the gemfile I see that this is calling for a version of Ruby that isn’t the latest and isn’t the older one I had installed.


```
source 'https://rubygems.org'
ruby '2.3.2'
```
I’m assuming that there’s something similar to Python’s virtualenv for Ruby. A bit of googling later I’m installing rbenv. Cool.
```
$ brew update
$ brew upgrade rbenv ruby-build
```
Following the [rbenv instructions for macOS](https://github.com/rbenv/rbenv#homebrew-on-mac-os-x) to add it to my path.


```
$ rbenv init
# Load rbenv automatically by appending
# the following to ~/.bash_profile:


eval "$(rbenv init -)"
$ atom ~/.bash_profile
```
I prefer using Atom over nano, vi or emacs. Whatever, fight me.


Now to source the shell.
```
$ source ~/.bash_profile
```
Next to install the version of Ruby I’ll need to get this tool to run


```
$ rbenv install 2.3.2
```
Then the whole thing falls apart


```
ruby-build: use openssl from homebrew
Downloading ruby-2.3.2.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.2.tar.bz2
Installing ruby-2.3.2...
ruby-build: use readline from homebrew


BUILD FAILED (OS X 10.12.3 using ruby-build 20170201)


Inspect or clean up the working tree at /var/folders/29/151mkpxs5tx3h4vz2qptg2xm0000gn/T/ruby-build.20170402120157.22586
Results logged to /var/folders/29/151mkpxs5tx3h4vz2qptg2xm0000gn/T/ruby-build.20170402120157.22586.log


Last 10 log lines:
compiling init.c
compiling constants.c
linking shared-object psych.bundle
installing default psych libraries
linking shared-object zlib.bundle
linking shared-object socket.bundle
linking shared-object tcltklib.bundle
installing default tcltklib libraries
linking shared-object ripper.bundle
make: *** [build-ext] Error 2
```
The whole log is [available on gist. ](https://gist.github.com/eriqnelson/30fcc22d9f68b590458fe65cb38fa3b2)


Ten minutes of searching and I find the [ruby-build ticket for this issue.](https://github.com/rbenv/ruby-build/issues/526)


From there I found the associated [language ticket](https://bugs.ruby-lang.org/issues/9630)


Seems like this is a known issue for this Ruby version and homebrew. I’ve wandered into a *language* issue when I want to do is make some API calls. An even bigger issue for me is that I’ve gone off the reservation from my initial goal. I’m reading up on clang and LLVM, wondering if I need to do a bunch more homebrew homework and maybe rollback a library or two for this compilation. This is not what I set out to do this morning.


## Solving My Problem


I could dig in with both hands into resolving the underlying issue with my local installation. Jacob recommended an alternative to rbenv in [rvm](https://rvm.io/). While that might solve the particular roadblock of getting rbenv installed and get the script to run it adds another piece of unfamiliar software to the mix. Instead I’m reaching for one of my favorite and more familiar tools; a Docker container.


### A Container for Tumblr-To-Hugo
To start off with I’ll need to identify a starting position for this container. I know it’s Ruby so I’ll start off with a read through the [Docker Hub Ruby Pages.](https://hub.docker.com/_/ruby/)


There I find this

>### Run a single Ruby script
>For many simple, single file projects, you may find it inconvenient to write a complete `Dockerfile`.
>In such cases, you can run a Ruby script by using the Ruby Docker image directly:
> `$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp ruby:2.1 ruby your-daemon-or-script.rb
`

Perfect, this sounds like me and I do want to run this one script.


If you’re not used to using Docker the commands for running containers can be pretty opaque. Here’s a breakdown of what each section is doing in this command string:
```
$ docker run \ # The docker run command itself
-it \ # Sets the container to interactive mode and allocates a tty to it
--rm \ #  Removes intermediate containers
--name my-running-script \ # Give the container a name. Otherwise it gets a HEX string as an ID.
-v "$PWD":/usr/src/myapp \ # This maps your current directory on your host to the container as “/usr/src/myapp”
-w /usr/src/myapp \ # Changes the working directory of the container to this directory
ruby:2.1 \ # Specifies the image used to create this container
ruby your-daemon-or-script.rb \ # The command passed to the container
```
More information on Docker run commands can be [found here](https://docs.docker.com/engine/reference/run/)




Adapting it to the script I’m trying to run I’ll rewrite the command. I’ll need to ensure the correct Ruby version is called and that I add the runtime arguments for the script.


`$ docker run -it --rm --name tumblr-to-docker-to-hugo -v "$PWD":/usr/src/myapp -w /usr/src/myapp ruby:2.3.2 ruby t2h.rb APIKEY eriqgnelson.tumblr.com`


That run failed with an error


```
/usr/local/lib/ruby/site_ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- httparty (LoadError)
        from /usr/local/lib/ruby/site_ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require'
        from t2h.rb:1:in `<main>'
```
Looks like I need to install the dependencies first, so I strip out the script command and bring up a shell.


`$ docker run -it --rm --name tumblr-to-docker-to-hugo -v "$PWD":/usr/src/myapp -w /usr/src/myapp ruby:2.3.2 bash`


Then the dependencies


`bundle install`


Then try again


`ruby t2h.rb APIKEY eriqgnelson.tumblr.com`


Great victory is mine, the posts are exported to a new directory called `blog` on my host machine. They have the expected TOML frontmatter and should port to Hugo without too much more issue.
```
├── 2013-03-30-in-the-stream-being-a-person-not-a-personality.html
├── 2013-04-11-draw-friends-live.html
├── 2013-04-26-how-to-dress-people-up-in-stuff.html
├── 2013-05-09-brittany-vs-room.html
├── 2013-06-12-looking-for-roommate.html
├── 2013-09-10-cartozia-tales.html
├── 2013-09-21-soft-show-6-good-humor-man.html
├── 2013-11-21-sexual-harassment-in-comics-next-steps.html
├── 2015-03-16-the-beyond-anthology.html
└── 2015-06-26-all-hail-the-matriarchs.html


```


Oh this is just the text posts.  Looking at the source code it makes sense:


```
class Tumblr
 TUMBLR_API_KEY = ARGV[0].freeze
 TUMBLR_DOMAIN = ARGV[1].freeze
 BASE_URL = "https://api.tumblr.com/v2/blog/#{TUMBLR_DOMAIN}/posts/text?api_key=#{TUMBLR_API_KEY}"
```
The `BASE_URL` is only set up to pull text posts.
I read the [README](https://github.com/jipiboily/tumblr-to-hugo/blob/master/README.md) and I didn’t expect it to skip all of the illustration posts given the wording:


>When you run this, it will create a file per post you have on Tumblr, with the proper title and a path that makes sense (/blog/the-title-of-the-post) and also create a CSV file with the original URL and the new path on Hugo, to help you setup the redirections.


It doesn't explicitly call out images, sounds or video. I assumed that it would be able to handle the media types that Tumblr does. That was incorrect.




#### Never Gonna Give You Up


I haven’t solved my initial problem of pulling images down from Tumblr but have learned some important things.


* Homebrew and Ruby don’t always play nice
* Containers can help mitigate dependency issues
* A short README may mean; go read the source instead


I’ll update this when I’ve found a way to handle pulling image posts from Tumblr to Hugo.
