[![Build Status](https://travis-ci.org/raphaelmerx/hamutuk_setup_testing.svg?branch=master)](https://travis-ci.org/raphaelmerx/hamutuk_setup_testing)

# homebrew-catalpa

Brew tap repository used at Catalpa. See https://docs.brew.sh/Taps.html


### Usage

1. Install Homebrew
See https://docs.brew.sh/Installation.html

2. Add this tap: `brew tap catalpainternational/catalpa`

3. Pin this tap, so that it's prioritised over homebrew-core: `brew tap-pin catalpainternational/catalpa`

4. Now brew will favor formulae inside of this repository over the ones in homebrew-core


### Adding a formulae

Let's say you want to install package `lanuteen`, and our production servers use version 3.1 of this package. Unfortunately, `brew info lanuteen` says brew would install version 4.2. To make brew install version 3.1, you need to find the formulae `lanuteen.rb` that was used in [homebrew-core](https://github.com/Homebrew/homebrew-core) back when brew was installing version 3.1.

For that:
1. `cd /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core`
2. Using `git log`, find the commit that updated formulae lanuteen to after v3.1
3. Checkout before that commit
4. copy `Formula/lanuteen.rb` from your homebrew-core repository to inside the homebrew-catalpa repository
5. Commit that copied formulae. Please use the commit message `lanuteen as in core/<commit_id>`.

Now all users that have tapped this repository, and pinned the tap (see above), will install version 3.1 when running `brew install lanuteen`

### Caveats

* If you want to use bottles that exist on the homebrew bintray, you should set the `root_url` in for the `bottles` part of your formula. See commit c4c518d60feed15d48ddd82205b18e807040a05a. Otherwise brew will try to find bottles at https://homebrew.bintray.com/bottles-catalpa/, which doesn't exist.

* Bottle URLs are constructed as `"#{root_url}/#{name}-#{version}.#{tag}.bottle.#{revision}.tar.gz"`. For example `https://homebrew.bintray.com/bottles/postgresql@9.5-9.5.12.sierra.bottle.tar.gz`

* If a formula inside this tap depends on another formula inside this tap, the `depends_on` should explicitly mention this tap. For example in the postgis formula, we have `depends_on "catalpainternational/catalpa/postgresql"`

* For our postgresql formula, we use the bottles that Homebrew core uses for postgresql@9.5. They were downloaded from the homebrew bintray, then uploaded to our own using:

  ```bash
  # download from the postgres@9.5 bottles
  wget https://homebrew.bintray.com/bottles/postgresql@9.5-9.5.12.el_capitan.bottle.tar.gz
  mv postgresql@9.5-9.5.12.el_capitan.bottle.tar.gz postgresql-9.5.12.el_capitan.bottle.tar.gz
  # upload to bintray
  curl -T postgresql-9.5.12.el_capitan.bottle.tar.gz 'https://username:api_key@api.bintray.com/content/raphaelmerx/homebrew-catalpa/postgresql/9.5.12/postgresql-9.5.12.el_capitan.bottle.tar.gz?publish=1&override=1'

  ```
