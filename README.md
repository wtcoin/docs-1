BlockCypher's Docs
========

This repository contains all of [BlockCypher's](http://www.blockcypher.com) API documentation, presented with the help of [GitHub Pages](https://pages.github.com/) and [Slate](https://github.com/tripit/slate).

Running BlockCypher's Docs Locally
------------------------------

We welcome Pull Requests to help us improve our documentation. If you're making edits, it's a good idea to run your fork of our docs locally before submitting a Pull Request; to do so, follow these instructions straight from [Slate.](https://github.com/tripit/slate)

### Prerequisites

You're going to need:

 - **Linux or OS X** — Windows may work, but is unsupported.
 - **Ruby, version 1.9.3 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

 1. Fork this repository on Github.
 2. Clone *your forked repository* (not our original one) to your hard drive with `git clone https://github.com/YOURUSERNAME/slate.git`
 3. `cd slate`
 4. Install all dependencies: `bundle install`
 5. Start the test server: `bundle exec middleman server`

Or use the included Dockerfile! (must install Docker first)

```shell
docker build -t slate .
docker run -d -p 4567:4567 slate
```

You can now see the docs at <http://localhost:4567>. 

*Note: if you're using the Docker setup on OSX, the docs will be
availalable at the output of `boot2docker ip` instead of `localhost:4567`.*

Now that you're running the docs locally, you'll probably want to learn more about [editing Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax).

Found an error or bug?
--------------------

Just [submit an issue](https://github.com/blockcypher/docs/issues). And, of course, feel free to submit Pull Requests with bug fixes or proposed changes.

Special Thanks
--------------------
- [Slate](https://github.com/tripit/slate)
- [GitHub Pages](https://pages.github.com/)
