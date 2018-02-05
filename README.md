# homebrew-catalpa
Brew tap repository used at Catalpa. See https://docs.brew.sh/Taps.html


### Usage

1. Install Homebrew
See https://docs.brew.sh/Installation.html

2. Add this tap
`brew tap catalpainternational/catalpa`

3. Pin this tap, so that it's prioritised over homebrew-core
`brew tap-pin catalpainternational/catalpa`


### Adding a formulae

Let's say you want to install package `lanuteen`, and our production servers use version 3.1 of this package. Unfortunately, `brew info lanuteen` says brew would install version 4.2. To make brew install version 3.1, you need to find the formulae `lanuteen.rb` that was used in [homebrew-core](https://github.com/Homebrew/homebrew-core) back when brew was installing version 3.1.

For that:
1. `cd /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core`
2. Using `git log`, find the commit that updated formulae lanuteen to after v3.1
3. Checkout before that commit
4. copy `Formula/lanuteen.rb` from your homebrew-core repository to inside the homebrew-catalpa repository
5. Commit that copied formulae. Please use the commit message `lanuteen as in core/<commit_id>`.

Now all users that have tapped this repository, and pinned the tap (see above), will install version 3.1 when running `brew install lanuteen`
