# tusk

A tiny command line wrapper for creating OAuth2 apps for the
[Mastodon](https://mastodon.social) API and using them with `curl`.

# Installation

1. Install [jq](https://stedolan.github.io/jq/).
2. Put `tusk` somewhere in your PATH: `install tusk /usr/local/bin`
3. Run `tusk --help` for instructions on how to create an app and token.

# Examples

Once you're set up with a token, a `tusk` command line is basically like
a `curl` command line, but instead of curl options (if needed) followed
by a URL, it goes in this order:

1. `tusk` options (to set the Mastodon instance and token)
2. An API resource name (only the part after the base API URI)
3. `curl` options (if needed)

To illustrate, here's how you would change your display name after
creating a token in `token.json`:

    tusk -t token.json /accounts/update_credentials -X PATCH -F display_name="Today's display name"

(This is actually why I wrote `tusk`! Instead of a literal value there,
I'm using my nickname followed by a new randomly-selected emoji each
day, as a goof on the early Mastodon meme of putting a ✅ emoji after
your name as a fake "verified" symbol.)

# Thanks

Darius Kazemi implemented
[a hybrid web form/curl command method of creating an app and token](https://tinysubversions.com/notes/mastodon-bot/index.html),
which inspired this.

# Contact

If you create a bot using this script, I'd love to hear about it.
I'm `@decklin@mastodon.social` and am going to set up some bots on
`botsin.space` soon.
